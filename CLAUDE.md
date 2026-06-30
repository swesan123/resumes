# Claude Code Guide: Resume Tailoring System

This guide documents Swesan's LaTeX resume tailoring workflow for Claude Code. When you open this repository, follow these rules and workflows.

## Source of Truth & Canon

Always treat **`resume_guidelines.txt`** in this repo as the **canonical specification** for how resume bullet points should look, read, and be structured.

- Read and apply `resume_guidelines.txt` before making substantial changes
- Also apply:
  - `guidelines/writing_guidelines.txt` — writing voice, style, and structure
  - `guidelines/technical_skills_guidelines.txt` (SECTION 4B) — skill category placement
  - `guidelines/work_experience_guidelines.txt` (SECTION 12B lab/BMC, 12C banned phrases)
  - `guidelines/projects_guidellines.txt` — project bullet framing

If generic resume advice **conflicts** with `resume_guidelines.txt`, **prefer `resume_guidelines.txt`** unless the user explicitly overrides it in the current session.

## Architecture Overview

```
main.tex                    ← Master resume (structure only)
config/                     ← Formatting config & LaTeX commands
sections/
  heading.tex              ← Contact & name
  education.tex            ← University
  skills.tex               ← Technical Skills (categories + technologies)
  experience/amd.tex       ← AMD employment (primary proof)
  projects/*.tex           ← ~15 available projects (3 selected per tailoring)
  courses.tex              ← Relevant Courses (1 line, 3–6 courses)
  hobbies.tex              ← Hobbies & Interests

postings/                   ← Job descriptions saved by company folder
  AMD/
  NVIDIA/
  Canonical/

resumes/                    ← Compiled PDFs archived by company
  AMD/
  NVIDIA/
  Canonical/

missing_skills.txt          ← Ledger of JD skills lacking honest proof
```

## Resume Tailoring Workflow

When the user asks to tailor the resume for a job posting, follow these **11 steps**:

### 1. Understand the Job

Extract and keep visible:
- Role title and seniority level
- Employer/company name (when stated)
- Main responsibilities (3–5 key themes)
- **8–12 core skills/keywords** (technologies, domains, tools, methodologies)

### 2. Persist the Posting

Save the **full job description text** as a dated file under `postings/`:

- **Known employer** (AMD, NVIDIA, Canonical, etc.):
  - Create `postings/<CompanyDir>/` if it doesn't exist
  - Use filename: `<Role-Slug>_<Company>-YYYY-MM-DD.txt`
  - Example: `postings/AMD/Graphics_IP_Validation_Engineer_AMD-2026-03-18.txt`
  - Include optional metadata comments at top: `# Role: ...`, `# Company: ...`, `# URL: ...`

- **Unknown employer**:
  - Save at `postings/<Role-Slug>-YYYY-MM-DD.txt` (repo root)
  - Example: `postings/Systems_Engineering_Role-2026-03-18.txt`

- **Do not overwrite** existing posting files unless the user explicitly asks; reference existing files in the summary if the same role/date already exists.

### 3. Load Project Canon

Before editing, read:
1. `resume_guidelines.txt` (full document)
2. `guidelines/writing_guidelines.txt`
3. `guidelines/technical_skills_guidelines.txt` — especially SECTION 4B
4. `guidelines/work_experience_guidelines.txt` — especially SECTION 12B (lab/BMC) and 12C (banned phrases)
5. `guidelines/projects_guidellines.txt`

These are your **authoritative source** for style, phrasing, bolding, and structure.

### 4. Plan Edits

Identify which sections to adjust:
- **Experience bullets** in `sections/experience/amd.tex`
- **Project bullets** in `sections/projects/*.tex` (which 3 to enable)
- **Technical Skills** in `sections/skills.tex` (what to add/remove/bold)
- **Relevant Courses** in `sections/courses.tex` (which courses to show)

Favor **minimal-diff LaTeX edits**: adjust existing bullet wording rather than add/remove bullets.

### 5. Edit Bullets

For each changed or new bullet:

- **Structure**: `Action verb + what was built/changed + key technologies + measurable result`
- **Impact focus**: Emphasize outcomes (latency, throughput, errors, scale, revenue, cost), not duties
- **Bolding**: Use `\textbf{...}` for only **2–4 critical JD-aligned skills or keywords** per bullet
  - Apply to **Experience and Projects** bullets and employer subheadings
  - **Do NOT bold** in Technical Skills comma-separated lists (see step 6)
- **No fabrication**: Never invent responsibilities, scope, or technologies
  - Only highlight what is already true about experience
  - Phrase narrowly if using internal frameworks (MAAS, ROCm, etc.)
- **Metrics**: Aggressively seek quantifiable outcomes; pair **numbers** with credible reason/scope—never invent metrics
- **Length**: Keep bullets **1–2 lines** each
- **Rollback**: When replacing a bullet, preserve the prior `\resumeItem{...}` as a `%`-commented line above the new one—preserves history and enables easy rollback

### 6. Sync Skills, Projects, and Courses

**Technical Skills**:
- Only keep skills with clear proof in at least one **Experience, Project, or Courses** bullet
- Comment out skills without proof instead of deleting them
- **Bolding rule**: Bold **only** category labels (e.g., `\textbf{Languages}{: Python, C}`)
  - Do **NOT** bold technologies after the colon—lists stay roman for consistency
- **Deduplication**: Merge synonyms (e.g., `BIOS/UEFI` instead of separate entries)
  - Use consistent Title Case for formal domains (Performance, System Management, IO)
  - Align the same terms in both Technical Skills **and** Experience bullets
- **Umbrella nouns**: Mention "firmware" in bullets (flash, GPU firmware, triage) if it needs proof; don't add as a category unless proven consistently
- **No orphaned skills**: If you remove a skill from `skills.tex`, remove matching tokens from the employer **keyword pipe** in `sections/experience/*.tex` in the same edit pass (or add honest proof)

**Metrics in bullets**:
- **Bold headline quantities** if layout permits (e.g., `**100+ runs weekly**, **20 to 5 minutes**`)
- Pair percent or time-save claims with credible reason—mechanism, scope, or before/after behavior
- Never invent metrics; keep prose-only if quantification is uncertain

**Projects**:
- **Enable 7–8 projects** across both pages (minimum 5, typical 7–8 for two-page layout)
  - Pick the projects that best match the posting's core keywords and themes, then add 2–3 supporting projects that show breadth
  - Lead with your 3–4 strongest matches, then include complementary projects (systems, databases, frontend, hardware, etc.) to demonstrate versatility
- Keep **2–3 bullets per active project**—never collapse to 1 line to save space
- Comment out other `\input{sections/projects/...}` lines in `projects.tex` (do **not** delete) for easy variant generation
- The goal is to fill **page 2 comfortably** with detailed project work that proves your full skill set

**Courses**:
- Render `sections/courses.tex` as **at most one line** with **3–6 high-signal course names**
- Keep the full course list as comments in the file for reuse in other tailorings

### 7. Page Layout (Default: Two Pages)

- **Default: Two pages** — This is the standard for experienced engineers with substantial proof of work
  - Full project bullets (2–3 per project); **never** collapse bullets into single lines just to save space
  - 7–8 projects shown across both pages with complete detail
  - 5–7 experience bullets with full context and metrics
  - Comprehensive Technical Skills and full Courses section visible
  - This creates a complete narrative of your capabilities and breadth
- **Two pages is the goal**, not a problem. Never artificially force one page unless the user explicitly asks
- **If overflow occurs**:
  - Don't merge project bullets or strip content
  - Instead: comment out lower-priority projects or very non-relevant courses (keep at least 3–4 projects active)
  - Trim verbose wording only if truly redundant
  - Keep Hobbies visible—it rounds out the narrative
- **One page only** when the user explicitly requests it (rare; only for specific situations)

### 8. Apply Changes Directly

- Edit the relevant `.tex` section files **directly** in this repo
- Preserve existing LaTeX structure and macros
- Keep diffs minimal while achieving tailoring goals
- **Never delete** content to hide it; **comment out** with `%` instead
  - Skills that lost proof → comment out in `skills.tex`
  - Project lines not selected → comment out in `projects.tex`
  - Bullets being replaced → comment prior version above new one

### 9. Compile and Archive the PDF

1. **Compile** `main.tex`:
   ```bash
   latexmk -pdf main.tex
   # or: pdflatex main.tex
   ```

2. **Compile gate (mandatory)**:
   - Clean log with no **LaTeX errors** (warnings OK)
   - **1–2 pages** (default 2 when dense; do not fail solely on page count unless user requested 1 page)
   - Scan for **`Overfull \hbox`** on experience headings; if present:
     - Shorten keyword line
     - Adjust wrapping in `config/commands.tex`
     - Reduce duplicate tokens
     - Do **not** ship layout breakage
   - If you change **macro arity** in `config/commands.tex`, **grep all call sites** (e.g., `\resumeSubheadingProminent`) and update every invocation in the same edit

3. **Archive the PDF**:
   - **Known employer**: Create `resumes/<CompanyDir>/` if needed (e.g., `AMD`, `NVIDIA`), then save:
     ```
     resumes/<CompanyDir>/<Role-With-Dashes>-YYYY-MM-DD.pdf
     ```
   - **Unknown employer**: Save to repo root:
     ```
     resumes/<Role-With-Dashes>-YYYY-MM-DD.pdf
     ```
   - Filename format: spaces → dashes, shell-safe ASCII
   - Example: `resumes/AMD/Graphics-IP-Validation-Engineer-AMD-2026-03-18.pdf`

### 10. Update `missing_skills.txt` (Mandatory)

The ledger tracks which JD skills still lack honest proof on the resume.

1. From the 8–12 JD keywords you extracted in step 1, check if the **final** resume provides honest proof:
   - A bullet in Experience/Projects mentioning it
   - An explicit course selection
   - A proof-backed skill row in Technical Skills
   - Employer keyword line (if aligned with bullets)

2. For each JD skill **still missing** honest proof after all edits:
   - **Increment** its counter by 1 if a row already exists, **or** append `Canonical skill name: 1` as a new row
   - **At most one increment per JD skill per tailoring pass**—even if the gap appears multiple times in the JD
   - Use recognizable, canonical names (e.g., "Debussy/Verdi-class trace debug" merges synonym tools)

3. After edits, **alphabetize** ledger entries (case-insensitive by skill name) while keeping `#` comment lines at the top

4. **Do not increment** for skills you consciously omitted despite having proof elsewhere—only count genuinely missing skills

5. Mention in the final summary **which ledger keys gained +1 or were newly added**

### 11. Summarize for the User

After tailoring, provide a **brief summary** covering:
- **Posting path**: `postings/<Company>/<Role-Date>.txt`
- **PDF path**: `resumes/<Company>/<Role-Date>.pdf`
- **Sections changed**: e.g., "Updated 3 bullets in projects, 2 in experience, synced Technical Skills"
- **Alignment strategy**: How edits match the JD's key responsibilities and keywords
- **Honest gaps**: JD requirements still missing (e.g., "Snowflake" not proved, "C++" only mentioned in project)
- **`missing_skills.txt`**: Which rows gained `+1` or were newly added

## Experience Heading Rules

These prevent recurring content and layout issues:

### Macro Arity & Call Sites

- **Standard macro**: `\resumeSubheading` (**4 arguments**): company, role title, location, dates
- **Prominent macro** (for long keyword rows): `\resumeSubheadingProminent` (**5 arguments**): company line, keyword tail (usually starting with `$|$`), location, role title, dates
- **Critical**: If you change macro arity in `config/commands.tex`, **grep all call sites** and update every invocation in the same edit pass
  - Failing to sync arity causes silent or noisy LaTeX failures

### Keyword Pipe Content

**What belongs in the employer keyword line** (the pipe after company name):
- Technologies, platforms, domains mirroring Technical Skills or the JD
- Languages, hardware interfaces (BMC, BIOS), operating systems, frameworks, validation axes
- Order keywords **consistently** with Technical Skills rows (Languages → Integration → Systems → Validation → …) unless user specifies otherwise

**What does NOT belong**:
- Soft skills, prose slogans, mission statements (e.g., "cross-functional collaboration," "customer-centric leadership")
- Coordination, stakeholder work, documentation achievements
- **Put these in bullets** instead, unless the user explicitly overrides

### Technical Skills Bolding Rule (Applies Everywhere)

- In **Technical Skills** block: bold only category labels, **NOT** comma-separated technologies
- In **Experience** employer lines: may bold top JD keywords in the subheading line
- In **Project** bullets: may bold 2–4 technologies per bullet (JD-aligned)
- **Reason**: Consistency with repo guidelines and readability—don't over-bold

### Proof Requirements for Skill Visibility

- Every skill listed in **Technical Skills** must have clear "proof" in at least one **Experience bullet, Project, or Course**
- If you add a language or tool to **Technical Skills** or employer keyword line, it **must** appear in a bullet in the same tailoring pass (or comment it out)
- When removing a skill from `skills.tex`, check the employer keyword line in `sections/experience/*.tex` and remove orphaned tokens in the same edit

## Common Regressions (Avoid These)

| Issue | What to do instead |
|-------|-------------------|
| **Fewer than 5–8 projects** shown (2-page default) | Enable 7–8 projects across both pages; lead with 3–4 strongest matches, then add breadth projects. Never collapse to 3 projects just to fit one page. The reference resume shows 8 projects with 2–3 bullets each filling both pages beautifully. |
| **Bold inside Technical Skills lists** | Category label bold only; technologies after `:` stay roman. Employer/experience subheading lines may still bold top JD keywords—that pattern is separate. |
| **Silent bullet replacement** | Comment prior `\resumeItem` text above the new bullet when it would otherwise disappear. |
| **Redundant skill pillars** | Merge synonyms (RAS vs. resets, Debussy vs. Verdi) after confirming with the candidate's terminology. |
| **Inconsistent domain capitalization** | Align Skills and Experience on the same capitalized names for formal axes (Performance, System Management, IO). |
| **Two-page PDF** (standard) | Perfect—this is the goal. Never apologize for or try to force one page. Two pages shows depth and breadth. Reference resume: 2 pages, 8 projects, 7 experience bullets = complete narrative. |
| **Same JD tailored twice** | Do not overwrite the existing `postings/` or `resumes/` files; point to the existing file and refresh `main.pdf` if needed. |
| **Missing `resumes/<Company>/`** | When the employer is known, **`mkdir -p`** that folder and drop the PDF inside; root-level `resumes/<Role>-<date>.pdf` is only for unknown employers. |
| **Invented tooling claims** | Never add MAAS, ROCm, etc. unless the candidate or existing bullets support it. Phrase narrowly and honestly if they assert it. |
| **Soft skills in employer keyword header** | Keep the pipe **tech- and domain-heavy**; express collaboration, leadership, and documentation in **bullets**. |
| **`Overfull \hbox` on `\resumeSubheadingProminent`** | Use the split-size layout in `config/commands.tex`, shorten keywords, or adjust wrapping—recompile until clean. |
| **Macro arity drift** | Changing `\newcommand{\resumeSubheadingProminent}[N]` without fixing **all** call sites causes failures—grep and fix in one changeset. |
| **User requests violate `resume_guidelines.txt`** | Follow repo canon (e.g., no post-colon bold in Technical Skills). Explain the constraint and suggest alternatives in bullets or employer line. |
| **BIOS and UEFI doubled** | Use **`BIOS/UEFI`** in Technical Skills, employer keyword line, and bullets for a single consistent token. |
| **Header lists orphaned token** | If you remove a skill from `skills.tex`, remove matching tokens from the employer keyword line in the same edit. |
| **Metric without reason** | Add a brief mechanism, qualification, or scope clause—or drop the number if it cannot be defended. |
| **Forgot `missing_skills.txt`** | Mandatory step 10: increment or append missing JD skills lacking honest proof before final summary. |

## When to Apply This Guidance

Apply this workflow when the user:
- Pastes a job description and asks to tailor the resume
- Mentions targeting, customizing, or optimizing for a specific role/company
- Asks to rewrite, refine, or improve bullets for a posting
- Requests a role-specific variant (ML, backend, infrastructure, etc.)

Otherwise, treat resume-related requests as general file edits unless the job posting context is clear.
