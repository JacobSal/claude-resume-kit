---
name: make-research-statement
description: Generate a tailored academic research statement (research vision, significance of prior work, open-science statement) from a session file and the knowledge base
user-invocable: true
---

# /make-research-statement

**User input:** `$ARGUMENTS`

Parse `$ARGUMENTS`:
- Session file path (e.g., `output/KU_Leuven/session_ku_leuven_postdoc.md`) → read that session file
- Session name (e.g., `ku_leuven_postdoc`) → find session file via shared_ops.md derivation
- Text in quotes → inline directives (e.g., `"one page max"`, `"emphasize reproducibility"`)
- Empty → check `kit_state.md` Active Sessions for the latest

---

## 0. Resume Kit Root — READ FIRST (plugin bootstrap)

Before anything else, establish where the working files live and load the hard rules:

1. **Find the root.** Read `_resume_kit_files/.schema/root.json` and let **RESUME_KIT_ROOT** = its `root` value. If the pointer is missing, fall back to `_resume_kit_files/` (relative to the project root) and warn: "root.json missing — using default _resume_kit_files/. Re-run setup-resume-kit.ps1."
2. **Resolve every path under the root.** Every relative path named in this skill — `config.md`, `kit_state.md`, `resume_builder/...`, `knowledge_base/...`, `output/...`, `JDs/...` — resolves under RESUME_KIT_ROOT.
3. **Load the hard rules NOW.** Read `<RESUME_KIT_ROOT>/resume_builder/reference/core_rules.md` — anti-fabrication, verb discipline, provenance, generation rules, web-search tool, and LaTeX notation. These are MANDATORY and override convenience.
4. **State file, not CLAUDE.md.** Wherever this skill says "Active Sessions" or "KB Corrections," read/write `<RESUME_KIT_ROOT>/kit_state.md` (the submodule `CLAUDE.md` is not auto-loaded in plugin mode).

---

## Safety Rules (ALWAYS ENFORCED)

**Accuracy > Relevance > Impact > ATS > Brevity**

Read `config.md` Provenance Flags before writing any content. Verify every claim against that table.

- Use the name/email from `config.md` Personal Info.
- **Citations must be accurate.** Cite first-author work only to the correct years/venues (check the Provenance Flags table — e.g., do NOT invent a year the author has no paper for). Contributing-author work is cited without first-author implication.
- **Verb discipline.** Full-ownership verbs (led, built, designed) only for independent first-author work; hedge (contributed, supported, helped) for contributing work.
- **Open-science honesty.** Claim only what is true: Open Data / Open Materials / open code if released. Do NOT claim preregistration or null-result publications unless the Provenance Flags confirm them — frame missing practices as forward commitments ("I will preregister…"), never as done.
- **Funding is not an award.** Fellowship intent (e.g., FWO) and grant-writing experience (e.g., NIH/NSF applications) are framed as intent/experience, never as awards held. Institutional project funding is not a personal fellowship.
- **No tool authorship.** Third-party tools (iCanClean, AMICA, FieldTrip, SimNIBS, etc.) were integrated, not authored.
- Source content from the knowledge base (see Phase 1) — never fabricate findings, populations, or numbers.

---

## User Input During Execution

If the user provides feedback, corrections, or suggestions at any point:
1. Acknowledge the input immediately
2. If it affects already-written content: fix it, re-verify length and anti-patterns
3. If it changes the framing: note the change in the session file Framing Strategy
4. Never restart — resume from current position

---

## Startup

Read `resume_builder/reference/shared_ops.md` for session startup and file derivation.

Then:
1. Read `kit_state.md` — check Active Sessions and KB Corrections Log
2. Read `config.md` — load Provenance Flags, Personal Info/email, Research Interests, Knowledge Base Sources
3. Find and read the session file
4. **Recovery check:**
   - If a research statement already exists for this session (`output/<Folder>/research_statement_<name>.md`) → "Research statement already exists. Re-generate? Waiting for confirmation."
   - Otherwise proceed to Phase 1

---

## Phase 1: Load Context

Read in this order:
1. **Session file** — specifically: JD Info, Company Context, Critique Context, Framing Strategy, CL hooks. The PI/lab mission and "why them" angle drive the vision section.
2. **Research-statement KB** (role: source/style_prior) — from the `## Knowledge Base Sources` table in `config.md`, load the `research_statements/` folder: the master `research_statement.md` and any prior variants. These set voice and the author's own stated vision.
3. **Extractions** (role: build) — `knowledge_base/extractions/`:
   - `*_research_statement.md` (the synthesized research-vision extraction)
   - Paper extractions for the significance-of-prior-work section (first-author papers first; honor each paper's authorship role)
4. `resume_builder/support/ai_fingerprint_rules.md` — banned words and structural rules (research statements are prose-heavy; apply with full weight).
5. `resume_builder/templates/researchstate_template.tex` — the moderncv-based LaTeX template you will fill (the output is a `.tex`, like the resume/CV/CL — not markdown).
6. The JD (path from the session file JD Info) — re-read for the exact statement requirements (many academic postings ask for SEPARATE documents: research vision + 5-year plan, significance/originality of prior work, and a transparent/reproducible-research statement). Note any page/word limit and any preprint/preregistration rules.

Update session file Status: `Research Statement: IN_PROGRESS`

Progress: "Loading research-statement context — [institution], PI [name], [N] paper extractions..."

---

## Phase 2: Plan Structure

Detect what the JD asks for and choose the structure. Default academic-postdoc research statement (1-2 pages):

1. **Opening / Vision** — who you are, the through-line of your work, the 5-year vision, and one explicit sentence on how it fits THIS lab/PI's program (name the PI and their site/mission from the session Company Context).
2. **Significance & Originality of Prior Work** — 2-4 grouped accomplishments drawn from extractions. Lead with first-author work; state what was new and why it matters. Cite accurately.
3. **Transparent & Reproducible Research** — what you actually do (Open Data/Materials/code, named repositories) + honest forward commitments (preregistration, null results) if not yet done. Reference relevant community initiatives only if verified.
4. **Future Directions** — the concrete next program, the honest bridge from your domain to theirs, funding intent (fellowship), and (optional) longer-term/translational aim.

If the JD requests the three documents separately, produce them as clearly delimited sections under one file (or separate files if the user asks), each self-contained.

Apply any inline directives (length caps, emphasis/de-emphasis).

---

## Phase 3: Generate (.tex from template)

Build the output by filling `resume_builder/templates/researchstate_template.tex` — do NOT invent a new preamble. The output is a `.tex` file, compiled like the resume/CV/CL.

1. **Header:** set name/contact from `config.md` Personal Info (`\firstname`, `\familyname`, `\title`, `\address`, `\mobile`, `\email`). `\title` is the document title (e.g., "Research Statement" or "Research Vision Statement").
2. **Sections:** replace the example `\section{...}` bodies with the planned sections (Vision; Significance & Originality; Transparent & Reproducible Research; Future Directions) using moderncv `\section{}`. Drop the `Teaching` section unless the JD asks for a teaching+research statement. Remove `\usepackage{lipsum}` and all `\lipsum`/placeholder text.
3. **Content rules:**
   - Author's own voice from the master statement; no invented persona.
   - Findings/populations/numbers ONLY from extractions; first-author vs contributing framing exact; citations accurate (years/venues per Provenance Flags).
   - Honest domain bridge; never imply experience you lack.
   - Anti-AI-fingerprint: no banned words (ai_fingerprint_rules.md), max 2 em-dashes (`---`) total, varied sentence length, no rhetorical Q+A, no "-ing analysis" endings.
   - LaTeX scientific notation per core_rules.md (`\ce{}`, `$\beta$`, `$\sim$`).

Save to `output/<Folder>/research_statement_<name>.tex`.

### Class / dependency handling (MANDATORY)
The template uses the **moderncv** document class — a CTAN package that is installed system-wide (the cover letter uses it too), so it needs NO custom `.cls` copied into the output folder.
- If a template's `\documentclass{...}` ever names a CUSTOM class shipped in `resume_builder/templates/` (e.g., `cv.cls`, `resume.cls`), copy that `.cls` (and any `.png`/asset it loads) into `output/<Folder>/` so the output compiles standalone — mirror how the CV build copies `cv.cls`.
- Do NOT use `researchstate.cls` as a class: it is a mislabeled duplicate of this template, not a LaTeX class. The canonical template is `researchstate_template.tex`.

### Hook Verification Gate (MANDATORY before presenting)

Use **Firecrawl** `firecrawl_search`/`firecrawl_scrape` (load via ToolSearch; not the built-in WebSearch — see core_rules.md "Web Search Tool") to verify every external reference used:
- PI name + cited research area/mission, lab/group name, program
- Any named initiative, consortium, dataset, or paper you cite (title, URL, what it is)

Present evidence as:
> **Claim:** [what the statement says] → **Evidence:** [what the search found] → **Source:** [URL]

Flag anything unverified as **"UNVERIFIED — please confirm"**. Do NOT present until hooks are verified or flagged.

---

## Phase 4: Compile, Verify & Present

**Compile** with the latex-server MCP (preferred), falling back to pdflatex:
- MCP: `compile_latex(file_path="output/<Folder>/research_statement_<name>.tex")` — latex-server paths are relative to the resume-kit root (`RESUME_KIT_ROOT`). Check `success: true` and `errors: []`.
- Fallback: `pdflatex -interaction=nonstopmode -output-directory=output/<Folder> output/<Folder>/research_statement_<name>.tex`

Use the Read tool to view the compiled PDF (page count, fill, no orphans/overfull). If a custom-class output failed because a `.cls`/asset is missing from the output folder, copy it (see Phase 3 class handling) and recompile.

| Gate | Check | If FAIL |
|------|-------|---------|
| Compile | latex-server `success:true` (or pdflatex exit 0); PDF produced | Fix LaTeX errors; copy missing `.cls`/assets |
| Length | Within JD limit (default 1-2 pages; ~500-1000 words/page) | Trim/expand |
| Provenance | All claims match config.md Provenance Flags; citations accurate | Fix before presenting |
| Open-science honesty | No preregistration/null-result claim unless verified | Reframe as commitment |
| Funding framing | Fellowship = intent, grants = experience, never awards | Fix |
| AI fingerprint | ≤2 em-dashes, no banned words, varied sentences | Rewrite |
| Fit | PI/lab named with a specific, verified connection | Add specificity |

Update session file:
- Add the statement to Output Files
- Status: `Research Statement: DONE`
- Note next step (e.g., `/critique` covers resume+CL; research statement reviewed here)

Update `kit_state.md` Active Sessions if status changed.

### >>>>>> MANDATORY STOP <<<<<<
Present: statement summary (sections, length, PI/hooks used, provenance confirmations).
**You MUST wait for the user's explicit text response before continuing.**

If the user requests changes: apply, re-verify, update the session file.
