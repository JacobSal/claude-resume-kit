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
| _Example: First-author method paper_ | _Published_ | _Journal, vol:pages (year). Note any interpretation caveats here._ |
| _Example: Contributing-author paper_ | _Published_ | _Your specific role (e.g., "data analysis only — not model implementation")._ |
| _Example: Manuscript in review_ | _Under review_ | _Never say "published". Use "under review" or "in preparation"._ |
| _Example: Internal/unpublished tool_ | _Unpublished_ | _"computational infrastructure I developed" — never imply peer-reviewed._ |

See `resume_builder/examples/example_config.md` for a fully worked example (fictional Dr. Jordan Chen).

---

## KB Corrections Log

Verified errors to never re-introduce. Add entries as you catch mistakes.

| Correction | Details |
|-----------|---------|
| _Example: tool/resource not to claim_ | _Do NOT list X — no direct experience. Use generic "..." instead._ |
| _Example: metric precision_ | _Correct value is 0.82, not 0.85. Confirmed in published Table 2._ |
| _Example: authorship verb_ | _Co-developed with [collaborator]. Always "Co-developed", never "Developed" alone._ |
| _Example: over-specific detail to drop_ | _Omit "(24%+)" / parenthetical abbreviations — use the generic phrasing._ |

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
