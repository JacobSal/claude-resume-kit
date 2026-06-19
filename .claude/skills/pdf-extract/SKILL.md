---
name: pdf-extract
description: Read PDF documents using the pdf-mcp MCP server
user-invocable: true
mcp-required:
  - pdf-mcp
---

# /pdf-extract

**User input:** `$ARGUMENTS`

Read a PDF file using the `pdf-mcp` MCP server and extract its content. The `pdf-mcp` MCP server is pre-configured in `.claude/settings.local.json` and its tools will be automatically used for PDF extraction.

Parse `$ARGUMENTS`:
- File path to a PDF (e.g., `career_files/knowledge_base/papers/paper.pdf`)
- Empty → prompt user for the PDF path

---

## Tools & Usage
For the pdf-mcp MCP server, eight specialized tools cover document introspection, content reading, search, and cache management. The typical pattern: call `pdf_info` first to plan, then `pdf_search` to locate, then `pdf_read_pages` or `pdf_read_all` to consume.

| Tool | What it does |
|------|--------------|
| `pdf_info` | Page count, metadata, TOC summary, scanned-page detection. **Call first.** |
| `pdf_get_toc` | Full table of contents for documents with >50 bookmarks |
| `pdf_read_pages` | Read specific pages or ranges; OCR-on-demand; embedded images + tables |
| `pdf_read_all` | Read entire document in one call (byte-capped for safety) |
| `pdf_render_pages` | Render pages as PNG for vision models — diagrams, handwriting, scans |
| `pdf_search` | Hybrid RRF search (keyword + semantic), page or section granularity |
| `pdf_cache_stats` | Per-document cache breakdown + total size |
| `pdf_cache_clear` | Clear expired or all cache entries |

Example prompts:

```
"Read the PDF at /path/to/document.pdf"
"Which pages discuss supply chain risks?"
"Find sections about the training process"
"Show me what page 5 looks like"
"OCR pages 3-5 of the scanned PDF"
```

## Startup

1. **Validate input** - Check if the provided path is valid
2. **Resolve path** - Convert relative paths to absolute paths within the project directory
3. **Verify file** - Ensure the file exists and is a PDF

If validation fails, return clear error:
```
**Error: Invalid PDF path**
- Attempted: [path]
- Issue: File not found or not a PDF
- Suggestion: Verify the path and try again
```

Progress: "Reading PDF... [filename]"

---

## Phase 1: Validate File

Extract info using `pdf_info` tool:

```
pdf_info [file_path]
```

Page count, metadata, TOC summary, scanned-page detection. Call first. Use this info to plan extraction strategy (e.g., if >20 pages, consider chunked reading or `pdf_read_all` with byte-capping).

If `pdf_info` fails (e.g., file not found, not a PDF, password protected), return error message to user and halt further processing.

---

## Phase 2: Extract PDF Content via MCP

If document is < 20 pages, use the `pdf_read_all` tool from the **pdf-mcp MCP server** configured in `.claude/settings.local.json`. If the document is larger, consider using `pdf_read_pages` to extract content in chunks. For targeted extraction (e.g., specific sections), use `pdf_search` to find relevant pages, then `pdf_read_pages` to extract those pages.

**The pdf-mcp MCP server provides:**
See **[docs/tool-reference.md](docs/tool-reference.md)** for the complete reference — every parameter, response shape, security contract, and example. For semantic-search model selection, see **[docs/embedding-models.md](docs/embedding-models.md)**.

```
**Integration:**
Claude's MCP infrastructure automatically routes tool calls to the configured `pdf-mcp` server. When invoked:
1. The skill passes the validated file path
2. pdf-mcp tools extracts all text and structure from the PDF
3. Content is returned with formatting preserved
4. Metadata (pages, extraction method) is included

**If extraction fails:**
- Password-protected PDFs: Tool returns error → suggest providing unprotected version
- File access errors: Tool returns error → verify permissions
- Unsupported format: Tool returns error → confirm file is a valid PDF

Progress: "Extracting text from PDF..."

---

## Phase 3: Process & Present Content

After successful extraction:

1. **Count content** - Estimate page/word count (call pdf_info) 
2. **Extract Document Text** - Read the document and format content into a markdown format (call pdf_read_pages)
3. **Format output** - Preserve document structure where possible
4. **Present to user** with:
   - Filename
   - Content metrics (pages, words, sections)
   - Full extracted text
   - Any quality notes (OCR artifacts, scanned docs, etc.)

**Output format:**

```
## PDF: [filename]

**Source:** [file_path]
**Content extracted:** [X pages / X words]

---

[Full extracted PDF content]
```

---

## Use Cases

- **Literature review:** Extract and analyze research paper content
- **Career materials:** Read PDFs from career_files knowledge base
- **Document processing:** Convert PDFs to searchable text
- **Integration:** Feed extracted text to other skills (critique, analysis, etc.)

---

## Architecture

```
User invokes: /pdf-reader <file_path>
    ↓
Skill validates file: pdf_reader_helper.py
    ↓
MCP-based extraction:
    1. pdf_info → plan extraction strategy
    2. pdf_search → locate relevant sections
    3. pdf_read_pages → extract content in chunks
    4. read the entire pdf (optional) → extract entire document using multiple calls to pdf_read_pages with different page ranges OR use pdf_read_all with byte-capping for safety
    ↓
Present content to user
```

The skill acts as a user-facing interface to the mcp-pdf-reader MCP server.

---

## Error Handling

| Error | Cause | Recovery |
|-------|-------|----------|
| File not found | Path doesn't exist | Verify path exists |
| Not a PDF | Wrong file format | Confirm file is .pdf |
| Password protected | PDF encrypted | Provide unprotected version |
| MCP unavailable | Server not running | Check MCP server configuration |

---

## Metadata

```yaml
name: pdf-reader
description: "Read PDF documents via the pdf-mcp MCP server. Validates file paths, extracts text content, and presents formatted output to the user. Gracefully handles validation and extraction errors."
compatibility:
  - mcp: "pdf-mcp"
  - requires: "pdf-mcp configured in .claude/settings.local.json"
  - mcp-tool: "pdf_info, pdf_get_toc, pdf_read_pages, pdf_read_all, pdf_render_pages, pdf_search, pdf_cache_stats, pdf_cache_clear"
version: "2.0"
```

---

*End of pdf-reader skill specification.*
