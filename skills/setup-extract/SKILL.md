---
name: setup-extract
description: Extract structured information from research papers, PDFs, or code into knowledge base extractions
user-invocable: true
---

# /setup-extract

**User input:** `$ARGUMENTS`

Parse `$ARGUMENTS`:
- File path to a paper (e.g., `papers/Smith2024_catalyst.pdf`, `papers/project_report.tex`) → read that file
- Multiple paths separated by spaces → batch mode (process each sequentially)
- Empty → ask the user for the paper path or paste content

---

## 0. Resume Kit Root — READ FIRST (plugin bootstrap)

Before anything else, establish where the working files live and load the hard rules:

1. **Find the root.** Read `_resume_kit_files/.schema/root.json` and let **RESUME_KIT_ROOT** = its `root` value. If the pointer is missing, fall back to `_resume_kit_files/` (relative to the project root) and warn: "root.json missing — using default _resume_kit_files/. Re-run setup-resume-kit.ps1."
2. **Resolve every path under the root.** Every relative path named in this skill — `config.md`, `kit_state.md`, `resume_builder/...`, `knowledge_base/...`, `output/...`, `JDs/...` — resolves under RESUME_KIT_ROOT. Example: `config.md` -> `<RESUME_KIT_ROOT>/config.md`; `knowledge_base/extractions/_INVENTORY.md` -> `<RESUME_KIT_ROOT>/knowledge_base/extractions/_INVENTORY.md`.
3. **Load the hard rules NOW.** Read `<RESUME_KIT_ROOT>/resume_builder/reference/core_rules.md` — anti-fabrication, verb discipline, provenance, generation rules, and LaTeX notation. These are MANDATORY and override convenience.
4. **State file, not CLAUDE.md.** Wherever this skill says "Read `CLAUDE.md`" for **Active Sessions** or **KB Corrections**, read `<RESUME_KIT_ROOT>/kit_state.md` instead. In plugin mode the submodule `CLAUDE.md` is not auto-loaded; `kit_state.md` is the working-copy state file.

---

## Startup

1. Read `kit_state.md` — check KB Corrections Log for known issues
2. Read `config.md` — load Personal Info (to identify user's author position), Provenance Flags
3. Read `knowledge_base/extractions/_INVENTORY.md` — see what's already extracted, avoid duplicates

If the paper is already in the inventory:
- Show the existing extraction path
- Ask: "This paper is already extracted. Re-extract (overwrite) or skip?"
- Wait for user response before proceeding

---

## Phase 1: Read & Understand the Paper

Read the paper using the appropriate method (see core_rules.md "Reading Source PDFs"):
- **PDF files:** Use **pdf-mcp**, NOT the Read tool. Load via ToolSearch (`select:mcp__pdf-mcp__pdf_info,mcp__pdf-mcp__pdf_read_pages,mcp__pdf-mcp__pdf_search`). **Pass a project-relative or `/mnt/c/...` WSL path, NEVER a `C:\` path — pdf-mcp runs in WSL and a Windows path fails silently.** Run `pdf_info` first for the page count, then `pdf_read_pages` in ranges (1-3, 4-7, 8-11, … through results/discussion) and/or `pdf_search` for specific sections. Read the WHOLE paper — do NOT stop at pages 1-3, or you will miss results and implications (the bug that produced "Level 1, pages 1-3 only" extractions).
- **.tex source:** Read directly with the Read tool — often more detail than the compiled PDF
- **If both exist:** Prefer .tex for content extraction, use pdf-mcp on the PDF for figures/tables

**While reading, collect:**
1. Full title, all authors, year, journal/venue, DOI (if available)
2. The user's position in the author list (first, co-first, second, middle, last, corresponding)
3. Publication status (check `config.md` Provenance Flags first, then infer: published / under review / draft / internal)
4. All computational methods, experimental techniques, software, and frameworks mentioned
5. Quantitative results — speedups, accuracies, efficiencies, improvements over baselines
6. Novelty claims — "first-ever", "new framework", "novel approach", etc.
7. Collaboration indicators — other groups, institutions, shared resources
8. Funding acknowledgments

Progress: "Reading paper... [title] by [first author] et al., [year]"

---

## Phase 2: Clarify User's Role

If the user's contribution is not obvious from the paper (common for multi-author work), ask:

**Questions to ask (skip any that are already clear from the paper):**
1. "What was your specific contribution? (e.g., all computational work, specific analysis, code development)"
2. "Did you develop any tools, methods, or code used in this paper?"
3. "Were there other groups or institutions involved? What was your group's role?"
4. "Any quantitative results you can personally claim? (e.g., 'I ran all the simulations')"
5. "Is there anything in this paper that should NOT appear on your resume? (e.g., collaborator's experimental data)"

### >>>>>> MANDATORY STOP — DO NOT PROCEED <<<<<<
Present your understanding of the paper and ask the clarifying questions above.
**You MUST wait for the user's explicit text response before continuing.**

---

## Phase 3: Write Extraction

Create the extraction file at `knowledge_base/extractions/<AuthorYear_short_descriptor>.md`

**Naming convention:** `<FirstAuthorLastName><Year><2-3_word_descriptor>.md`
- Examples: `Smith2024_protein_stability.md`, `Chen2023_binding_affinity.md`
- If the user is first author: use their last name
- Normalize to lowercase with underscores

**Extraction format:**

```markdown
# [Full Paper Title]

## Metadata
- **Authors:** [author list — highlight user's name with bold]
- **Year:** [year]
- **Journal:** [journal/venue or "unpublished"/"internal"/"under review at X"]
- **DOI:** [DOI or "N/A"]
- **User's role:** [first author / co-first / contributing / corresponding]
- **Status:** [published | under review | draft | internal]

## Methods & Tools
- **Computational methods:** [e.g., MD, ML, FEA, CFD, etc. — be specific about methods, force fields, etc.]
- **Software/frameworks:** [e.g., GROMACS, PyTorch, ABAQUS, custom code, etc.]
- **Hardware/HPC:** [if mentioned — clusters, GPU resources, etc.]
- **Key techniques:** [specific methodological details that map to resume skills]

## Key Results
[Number each result. Include quantitative metrics wherever possible.]
1. [Result with numbers — e.g., "Achieved 5,000x speedup over brute-force screening"]
2. [Result — e.g., "Screened 8,500 variants, identified 7 top candidates"]
3. [...]

## Novelty Claims
[What's genuinely new — be precise, avoid overclaiming]
- [e.g., "First application of framework X to system Y"]
- [e.g., "New method combining A and B — no prior work exists"]

## Collaboration & Scope
- **Other groups:** [institutions, PIs involved]
- **User's specific contribution:** [from Phase 2 clarification]
- **Shared vs. sole work:** [what the user did alone vs. with others]

## Provenance Notes
- **Publication status:** [matches config.md if listed there]
- **Safe to claim:** [what the user can put on a resume without hedging]
- **Needs hedging:** [claims that require "contributed to" or "supported" framing]
- **Do NOT claim:** [results from collaborators, claims that would be overclaiming]

## Resume Bullet Seeds
[3-5 draft bullets in STAR format. These are seeds, not final text.]
[Use full-ownership verbs only for sole-contributor work. Hedge for shared work.]
1. [Action verb] + [what was done] + [quantitative result/impact]
2. [Action verb] + [method/tool developed] + [what it enabled]
3. [Action verb] + [scope — e.g., "across N systems"] + [outcome]
4. [Optional: collaboration-framed bullet]
5. [Optional: tool/infrastructure bullet]
```

Save the file. Show the user the complete extraction.

Progress: "Writing extraction for [short title]... [N] results identified, [M] bullet seeds drafted"

---

## Phase 4: Update Inventory

Read and update `knowledge_base/extractions/_INVENTORY.md`.

Add a row to the inventory table:

```
| [filename] | [short title] | [user's role] | [status] | [primary methods] | [date extracted] |
```

Present the updated inventory entry to the user.

---

## Phase 5: Next Steps

After extraction is complete, present:

1. **Extraction summary:** [N] methods, [M] quantitative results, [K] bullet seeds
2. **Provenance flags:** Any items that need special handling
3. **Suggested next action:**
   - If more papers to extract: "Run `/setup-extract [next paper path]`"
   - If all papers done: "Run `/setup-build-kb` to synthesize extractions into experience files and bundles"

### >>>>>> MANDATORY STOP <<<<<<
Present extraction summary. Wait for user feedback or next paper.
**You MUST wait for the user's explicit text response before continuing.**

---

## Batch Mode

If `$ARGUMENTS` contains multiple file paths:
1. Process each paper through Phases 1-4 sequentially
2. Ask Phase 2 clarifying questions for ALL papers at once (grouped) before writing any extractions
3. After all extractions: present combined inventory update and summary
4. Single STOP at the end (not per paper)
