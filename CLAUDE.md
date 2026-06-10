# claude-resume-kit — Project Instructions

> This file is auto-loaded by Claude Code. It provides project-wide rules for all skills.

---

## File Map

```
.claude/skills/
├── setup-extract/SKILL.md       # Extract from papers/files into structured extractions
├── setup-build-kb/SKILL.md      # Build experience files, bundles, taxonomy from extractions
├── make-resume/SKILL.md         # Phase 0-2: JD research → bullet plan → resume/CV generation
├── make-cl/SKILL.md             # Cover letter generation from session file
├── edit-resume/SKILL.md         # Edit resume/CV from critique or user feedback
└── critique/SKILL.md            # 8-dimension critique of full package

resume_builder/
├── reference/
│   ├── shared_ops.md            # Session startup, derivation, workflow — ALL skills
│   ├── resume_reference.md      # Resume/CV rules — /make-resume, /edit-resume
│   ├── cl_reference.md          # CL rules — /make-cl, /edit-resume (CL edits)
│   ├── critical_rules.md        # Compact re-read — /make-resume Phase 2
│   ├── session_file_template.md # Session file format
│   └── critique_framework.md    # 8-part critique system
├── templates/                   # LaTeX .cls + .tex templates
├── helpers/                     # char_count.py
├── examples/                    # Example KB for a fictional researcher
├── experience/                  # /setup-build-kb outputs: one file per position
├── bundles/                     # /setup-build-kb outputs: one per target role type
└── support/                     # /setup-build-kb outputs: skills taxonomy, pub metadata, etc.

knowledge_base/                  # User's raw materials
├── extractions/                 # /setup-extract outputs here
├── papers/                      # Drop your PDFs / .tex source here
└── notes/                       # Any other reference material

config.md                        # User configuration (email, provenance, role types)
```

---

## Core Rules — Canonical Source

> **The hard rules (Your Role, User Focus Directives, Anti-Fabrication, Generation Rules,
> LaTeX notation) now live in a single canonical file:**
> [`resume_builder/reference/core_rules.md`](resume_builder/reference/core_rules.md)
>
> Every skill reads `core_rules.md` as the FIRST step of Startup, in both standalone and
> plugin modes. Edit the rules THERE — not here — so the two modes never drift.
>
> In **plugin mode** this file (`CLAUDE.md`) is NOT auto-loaded; the skills load
> `core_rules.md` from the working copy via the root pointer instead (see each SKILL.md
> "Resume Kit Root" bootstrap block).

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

## Active Sessions

_Update this section when starting/finishing a JD._

| Session | Status | Next Command |
|---------|--------|-------------|
| (none active) | — | — |

---

## KB Corrections Log

_See `config.md` for user-specific corrections. Add verified errors here as you find them._
