# Configuration

> Edit this file with your personal details. Every skill reads this file.

---

## Personal Info

- **Name:** [Your Full Name]
- **Degree suffix:** [e.g., Ph.D., M.S., or leave blank]
- **Email:** [your@email.com]
- **Phone:** [+1 XXXXXXXXXX]
- **Location:** [City, State ZIP]
- **LinkedIn:** [URL or leave blank]
- **Google Scholar:** [URL or leave blank]
- **ORCID:** [URL or leave blank]
- **Website:** [URL or leave blank]

---

## Document Preferences

- **Resume pages:** 2
- **CV pages:** 5
- **Resume bullet variant:** 2L (all variable bullets are 2-line)
- **CV bullet variant:** 2L/3L mix
- **Skills config (resume):** 4-3-2-2-2 (13 lines, 5 groups)
- **Skills config (CV):** 4-4-3-3-3 (17 lines, 5 groups)
- **Immigration line:** Yes | "Authorized to work in the United States"

---

## Provenance Flags

Track the publication status of your work. Skills check this table before every output.

| Item | Status | Correct Framing |
|------|--------|----------------|
| Salminen2026 (Interstride EEG) | Published | IEEE TNSRE 34:952-965 (2026). Interpretation: aging affects peripheral neuromuscular factors (not cortical constraints). |
| Salminen2025 (Speed-dependent EEG) | Published | Journal of Neurophysiology 133(6):1761-1794 (2025). Analysis: 7 brain regions examined. |
| DeVol2026 (Aperiodic EEG + BART) | Published | IEEE TNSRE 34:2516-2529 (2026). User role: EEG preprocessing, methods development (not direct ML implementation). |
| Pliner2026 (CRUNCH model) | In-press | PLOS Aging and Health (2026). User role: EEG data analysis. Framing: age differences in cortical recruitment patterns. |
| Liu2025 (Visual compensation) | Published | Imaging Neuroscience 3:IMAG.a.1039 (2025). |
| Liu2024 (Parametric terrain) | Published | Imaging Neuroscience 2:1-33 (2024). |

---

## KB Corrections Log

Verified errors to never re-introduce. Add entries as you catch mistakes.

| Correction | Details |
|-----------|---------|
| XSEDE/HiperGator reference | Do NOT include XSEDE in CV/resume — user does not have direct experience with XSEDE. Use generic "High-Performance Computing" or "HPC" instead. |
| Salminen2026 interpretation | Paper finding (lower EEG variability + higher biomechanical variability in older adults) indicates aging affects PERIPHERAL NEUROMUSCULAR factors, not cortical constraints. Always cite the Conclusions section of papers for accurate interpretation. |
| Salminen2025 brain regions | Analysis covered 7 brain regions, NOT 8+. Correct count: sensorimotor, prefrontal, parietal, occipital (4 main + 3 others = 7 total). |
| DeVol2026 author role | User assisted with EEG preprocessing, methods development, and artifact removal — NOT direct BART model application. Never claim direct implementation of the ML model; stick to "assisted with preprocessing and methods development." |
| BART model metrics | Do NOT include R²=0.70 in resume/CV bullets — unnecessary technical detail. Do NOT include "FDR" — replace with generic "multiple comparison correction." |
| Clinical statistics phrasing | Use "clinical statistics and mobility assessment" NOT "(SPPB, mobility assessment)" — avoid parenthetical abbreviations in CV bullets. |
| PhD GPA | Jacob's PhD GPA is exactly 3.9, NOT 3.9+ or 4.0. Always list as 3.9. |
| Accumulated Local Effects | Do NOT include "Accumulated Local Effects (ALE)" in technical expertise — not relevant to reproducibility focus. Replace with generic "model interpretation and validation." |
| Fused LASSO regression | MUST include fused LASSO regression in Machine Learning & Statistical Methods section. It is a key skill. |
| Missing data handling detail | Do NOT include "(24%+)" in missing data handling bullet — this is an overly specific detail. Use generic "missing data handling" instead. |

---

## Role Types

Define the role types you're targeting. Each gets a bundle during setup.

| Role Name | Target Employers | Tier | Bundle File |
|-----------|-----------------|------|-------------|
| _Example: National Lab_ | _DOE labs, national facilities_ | _1_ | _bundle_national_lab.md_ |
| _Example: Industry R&D_ | _Tech companies, R&D divisions_ | _2_ | _bundle_industry_rd.md_ |

**Tier guide:** 1 = strongest evidence, full portfolio | 2 = strong with targeted emphasis | 3 = viable with careful framing

---

## Role-Type Decision Tree

Customize this to map JD keywords to your role types.

| If JD mentions... | Primary profile | Secondary (hybrid) |
|-------------------|----------------|-------------------|
| _[your domain keywords]_ | _[role type]_ | _[secondary or --]_ |

---

## FIXED Sections

List template sections that should NEVER be modified during generation.
These are copied verbatim from your template every time.

- Education
- Publications (CV)
- Honors & Awards
- Header block (name, contact, links)
- _[Add any other fixed sections]_

---

## Output Rules

- **Email in all outputs:** [same as Personal Info email]
- **Resume package:** [N] pages + 1-page cover letter
- **CV package:** [N] pages + 1-2 page cover letter
- **Output .tex files ONLY** — user compiles locally
