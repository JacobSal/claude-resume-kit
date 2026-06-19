# claude-resume-kit — Agent Instructions

> Cross-agent source of truth for this repository. Claude Code loads it via `CLAUDE.md`
> (`@AGENTS.md`); other agents (Codex, Copilot CLI) should read this file directly.
> All project-wide rules for the resume/CV/cover-letter skills live here.

---

## What this repo is

A resume / CV / cover-letter / research-statement **generation kit** for Claude Code. You
extract your papers and reports once into a structured knowledge base, then point the
skills at a job description (JD) to produce accuracy-checked LaTeX you compile locally.
Built for researchers and engineers with lots of source material applying to many roles.

---

## Two operating modes (READ FIRST)

This kit runs in **two modes**. Which one you are in determines where files resolve.

| | **Standalone** (clone-and-run) | **Plugin** (marketplace plugin) |
|---|---|---|
| How | `git clone` this repo, run `claude` inside it | Enabled as a plugin in a host project |
| Root | the repo root itself | `RESUME_KIT_ROOT` (a working copy, e.g. host `_resume_kit_files/`) |
| Root pointer | n/a | `_resume_kit_files/.schema/root.json` → `root` value |
| State file | `CLAUDE.md` (Active Sessions, KB Corrections) | `kit_state.md` under `RESUME_KIT_ROOT` |
| Auto-loaded context | `CLAUDE.md` is auto-loaded | `CLAUDE.md` is **NOT** auto-loaded — skills read files explicitly |
| Hard rules | `resume_builder/reference/core_rules.md` | same file, resolved under `RESUME_KIT_ROOT` |

Every `SKILL.md` opens with a `## 0. Resume Kit Root — READ FIRST` bootstrap that resolves
the root and loads `core_rules.md` before anything else. In plugin mode, all relative paths
named below (`config.md`, `resume_builder/…`, `knowledge_base/…`, `output/…`, `JDs/…`)
resolve under `RESUME_KIT_ROOT`, not the repo root.

---

## Skill location — single source of truth

**`.claude-plugin/plugin.json` is authoritative** for where skills live. It declares:

```json
"skills": ["./skills"]
```

So the kit's skills are at **`skills/`** (repo root), invoked as `/skill-name`. Do **not**
restate skill paths elsewhere — point at `plugin.json`. (The unrelated `.claude/skills/`
directory holds local tooling skills — firecrawl, pdf-extract, pmat-*, init-better — not
the resume kit.)

The 7 skills:

| Skill | Purpose |
|-------|---------|
| `setup-extract` | Extract structured data from papers/files into `knowledge_base/extractions/` |
| `setup-build-kb` | Synthesize extractions into experience files, role bundles, support files |
| `make-resume` | JD → bullet plan → tailored resume/CV (`.tex`) + session file |
| `make-cl` | Session file → matching cover letter (`.tex`) |
| `make-research-statement` | Session + KB → academic research statement (vision, significance, open-science) |
| `edit-resume` | Edit resume/CV/CL from critique or user feedback |
| `critique` | Independent 8-dimension quality review of the full package |

---

## File Map

```
claude-resume-kit/
├── .claude-plugin/plugin.json    # Plugin manifest — declares skills at ./skills (authoritative)
├── CLAUDE.md                     # @AGENTS.md + standalone-mode state tables
├── AGENTS.md                     # This file — cross-agent source of truth
├── config.md                     # Personal configuration (template; fill in your details)
├── skills/                       # The 7 skills (invoked as /skill-name) — see table above
│   ├── setup-extract/SKILL.md
│   ├── setup-build-kb/SKILL.md
│   ├── make-resume/SKILL.md
│   ├── make-cl/SKILL.md
│   ├── make-research-statement/SKILL.md
│   ├── edit-resume/SKILL.md
│   └── critique/SKILL.md
├── resume_builder/
│   ├── reference/
│   │   ├── core_rules.md            # CANONICAL hard rules — loaded FIRST by every skill
│   │   ├── shared_ops.md            # Session startup, derivation, workflow — ALL skills
│   │   ├── resume_reference.md      # Resume/CV rules — /make-resume, /edit-resume
│   │   ├── cl_reference.md          # CL rules — /make-cl, /edit-resume (CL edits)
│   │   ├── critical_rules.md        # Compact re-read — /make-resume Phase 2
│   │   ├── session_file_template.md # Session file format spec
│   │   └── critique_framework.md    # 8-part critique system
│   ├── templates/                   # LaTeX .cls classes + .tex templates
│   ├── helpers/                     # char_count.py (bullet character counter)
│   ├── support/                     # ai_fingerprint_rules.md, skills taxonomy, pub metadata
│   ├── examples/                    # Fictional "Dr. Jordan Chen" — full worked example
│   ├── experience/                  # /setup-build-kb output: one file per position
│   └── bundles/                     # /setup-build-kb output: one per target role type
├── knowledge_base/
│   ├── extractions/                 # /setup-extract output
│   ├── papers/                      # Drop your PDFs / .tex source here
│   └── notes/                       # Any other reference material
├── JDs/                             # Job descriptions (text files)
└── output/                          # Generated .tex, session files, critiques
```

---

## Core Rules — Canonical Source

> **The hard rules (Your Role, User Focus Directives, Anti-Fabrication, Generation Rules,
> LaTeX notation) live in a single canonical file:**
> [`resume_builder/reference/core_rules.md`](resume_builder/reference/core_rules.md)
>
> Every skill reads `core_rules.md` as the FIRST step of Startup, in both standalone and
> plugin modes. Edit the rules THERE — not in this file or in any SKILL.md — so the two
> modes never drift. The AI-fingerprint-avoidance rules live in
> [`resume_builder/support/ai_fingerprint_rules.md`](resume_builder/support/ai_fingerprint_rules.md),
> loaded by all generation and critique skills.

---

## LaTeX Scientific Notation (MANDATORY)

All templates load `mhchem` (`\usepackage[version=4]{mhchem}`). Use these conventions:

| Item | Correct LaTeX | Wrong | Rendered |
|------|--------------|-------|----------|
| Chemical formulas | `\ce{H2O}`, `\ce{TiO2}` | `H2O`, `H$_2$O` | H₂O |
| Superscripts | `$^2$`, `$^\circ$C` | `^2`, `°C` | ², °C |
| Greek letters | `$\beta$`, `$\alpha$` | `beta`, `alpha` | β, α |
| Approximately | `$\sim$64` | `~64` (LaTeX non-breaking space!) | ~64 |

**CRITICAL:** `~` in LaTeX is a non-breaking space, NOT a tilde. Use `$\sim$` for "approximately."

For char counting: `\ce{TiO2}` → 4 rendered chars, `$\beta$` → 1 rendered char.

---

## State files

Skills track per-JD progress and verified corrections in a **state file**:

- **Plugin mode (canonical):** `kit_state.md` under `RESUME_KIT_ROOT`. `shared_ops.md` and
  every SKILL.md bootstrap point here.
- **Standalone mode (fallback):** the **Active Sessions** and **KB Corrections Log** tables
  at the bottom of `CLAUDE.md`.

Per-user provenance corrections also live in `config.md` (KB Corrections Log section).
