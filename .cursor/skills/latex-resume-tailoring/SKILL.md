---
name: latex-resume-tailoring
description: Tailors LaTeX resumes in this repository to specific job postings using resume_guidelines.txt and the latex-resume-tailoring rule. Use when the user pastes a job description and asks to customize, refine, or target this LaTeX resume for a role.
---

# LaTeX Resume Tailoring

## When to use this skill

Use this skill when:
- The user pastes a job description and mentions tailoring, targeting, or optimizing this LaTeX resume.
- The user asks to rewrite, refine, or improve bullets for a specific role or company.
- The user wants role-specific variants (e.g., ML, backend, infra) of the same resume.

## Workflow

1. **Understand the job**
   - Extract:
     - Role/title and level
     - Employer/company name (when stated in the posting or URL)
     - Main responsibilities
     - 8–12 core skills or keywords (technologies, domains, tools)
   - Keep these visible while editing.

2. **Persist the posting** (`postings/`)
   - Save the **full job description text** you tailored against as a new file under `postings/` (do **not** overwrite existing postings unless the user asks—if the same role and date already exist, reuse that path and mention it in the summary).
   - **Filename pattern**: `<short-role-slug>_<company-slug>-YYYY-MM-DD.txt`
     - Slugs: ASCII, spaces → underscores, strip unsafe punctuation (keep readability).
     - If company is unknown, omit `_<company-slug>` (still include date).
     - Examples (conceptually): `Graphics_IP_Validation_Engineer_AMD-2026-03-18.txt`, `Validation_Engineer_1_AMD-2026-05-01.txt`.
   - Put **optional metadata** at the top as comments (lines starting with `#`): role title, company, source URL if known, date saved.

3. **Load project canon**
   - Read `resume_guidelines.txt`.
   - Assume the `.cursor/rules/latex-resume-tailoring.mdc` rule is active and authoritative.
   - Prefer these over generic resume advice unless the user explicitly overrides.

4. **Plan edits**
   - Identify which sections to adjust:
     - Experience bullets
     - Projects bullets
     - Skills or summary sections, if present
     - Relevant courses line and which courses are shown
   - Favor **minimal-diff** LaTeX edits: adjust existing bullets before adding or removing bullets.

5. **Edit bullets**
   - For each changed or new bullet:
     - Follow the structure: **Action verb + what was built/changed + key technologies + measurable result**.
     - Focus on **impact and metrics**, not duties.
     - Use `\textbf{...}` for only **2–4 critical skills or keywords per bullet** (usually JD-aligned terms). This applies to **Experience and Projects** bullets and optional employer subheadings—not to comma-separated lists after colons in **Technical Skills** (see step 6).
     - Do **not invent** responsibilities, scope, or technologies. If the **candidate explicitly states** a tool or scope (e.g. MAAS, internal frameworks), reflect it in bullets and skills **only** as they described it, without fabricating metrics or ownership.
   - Keep bullets **1–2 lines** each.
   - When **replacing** an active bullet’s wording, preserve the prior version as a **`%`-commented** `\resumeItem{...}` immediately above the new line when it is not already saved elsewhere—reduces confusion about “deleted lines” and keeps rollback easy.
   - For **industry acronyms** (BKC, RAS domains, etc.), prefer clarity: spell out or define on first use when the posting audience may be mixed unless the candidate wants acronym-heavy wording for specialists.

6. **Sync skills, projects, and courses**
   - **Technical Skills**:
     - Only keep skills that have clear support in at least one bullet (Experience, Projects, or Courses).
     - Prefer commenting out unused skills over leaving them visible without proof.
     - **Bolding**: In the Technical Skills block, bold **only** the category labels (e.g. `\textbf{Languages}{: Python, C}`). Do **not** bold individual technologies after the colon—lists stay plain text for consistency.
     - **Dedupe and naming**: Do not list synonymous pillars twice (e.g. **RAS** already implies reset-related validation—drop redundant “resets” unless the candidate insists both appear). Use **consistent Title Case** for named validation axes when they read as formal domains (e.g. Performance, System Management) and align the same terms in Experience bullets where those axes appear.
     - **Row budget**: Extra skill rows often trigger **page overflow** before bullets do. After substantive skills edits, **compile**; if the PDF exceeds one page, merge related tokens onto one row (e.g. stack libraries under an existing category) or trim bullets—avoid silently shipping two pages.
   - **Projects**:
     - Always enable **exactly 3 projects** (minimum **3**, maximum **3**): pick the three best matches for the posting.
     - Comment out other `\input{sections/projects/...}` lines in `projects.tex` instead of deleting them.
   - **Courses**:
     - Ensure `courses.tex` renders at most a **single `\item` line** with 3–6 short, relevant course names.
     - Keep the full course list in comments so it can be reused for other roles.

7. **Enforce one-page layout**
   - Aim for a single-page resume by:
     - Trimming or merging low-impact bullets.
     - Commenting out optional sections (e.g., Hobbies, long course lists) when they cause overflow.
   - Only keep additional sections if they clearly support the current job (e.g., hardware-heavy courses for firmware roles).
   - **JD-first density**: When the posting is infra/automation/Linux-heavy but the strongest proof is domain-specific (e.g. GPU validation acronyms), lead with responsibilities and keywords from the **JD**, keep specialist proof second—so screeners see overlap before niche pillars.

8. **Apply changes**
   - Edit the relevant `.tex` section files directly in this repo.
   - Preserve existing LaTeX structure and macros.
   - Keep diffs as small as possible while achieving the tailoring goals.
   - **Do not delete** lines to remove content; **comment out** with `%` (skills, `\input{sections/projects/...}`, bullets, sections) so prior variants stay in the file.

9. **Compile and archive the PDF** (`resumes/`)
   - Compile `main.tex` so `main.pdf` is up to date (run from repo root if needed).
   - **PDF basename**: `<Role-With-Dashes>-YYYY-MM-DD.pdf` (spaces → `-`; shell-safe).
   - **Destination**:
     - When the **employer is known** from the posting or URL, choose `<CompanyDir>` as a stable folder slug under `resumes/` (match existing folders when names align: `AMD`, `Wealthsimple`, `Capital One`, `Canonical`, etc.).
     - Run **`mkdir -p resumes/<CompanyDir>`** so the folder exists, then copy `main.pdf` to **`resumes/<CompanyDir>/<Role-With-Dashes>-YYYY-MM-DD.pdf`**.
     - When the **employer cannot be determined reliably**, save only at the repo root: **`resumes/<Role-With-Dashes>-YYYY-MM-DD.pdf`** (no generic company guess).

10. **Summarize for the user**
   - Briefly describe:
     - Path to the saved posting (`postings/…`) and exported PDF (`resumes/…`).
     - Which sections changed (for example, “Updated 3 bullets in projects, 2 in experience”).
     - How the edits align with the job’s key responsibilities and keywords.
   - Avoid repeating large LaTeX code blocks in the summary.

## Lessons learned (avoid common regressions)

Short checklist distilled from real tailoring rounds:

| Issue | What to do instead |
|--------|---------------------|
| Fewer than **3** projects shown | Always enable **exactly 3** `\input{...}` lines in `projects.tex`; never ship 2 “to save space” without trimming elsewhere first. |
| Bold inside Technical Skills lists | Category label bold only; technologies after `:` stay roman. Employer/experience **subheading** lines may still bold top JD keywords—that pattern is separate from the Skills block. |
| Silent **bullet replacement** | Comment prior `\resumeItem` text above the new bullet when it would otherwise disappear from the file. |
| Redundant skill pillars | Merge synonyms (RAS vs resets) after confirming with the candidate’s terminology. |
| Inconsistent domain capitalization | Align Skills and Experience on the same capitalized names for formal axes (Performance, System Management, IO). |
| **Two-page** PDF after skills tweak | Merge skill rows or shorten wording before trimming projects below three. |
| Same JD tailored twice | Do not overwrite `postings/`; point to the existing `.txt` and refresh `main.pdf` / export only. |
| Missing `resumes/<Company>/` | When the employer is known, **`mkdir -p`** that folder and drop the PDF inside; root-level `resumes/<Role>-<date>.pdf` is only for unknown employers. |
| Invented tooling claims | Never add MAAS/ROCm/etc. unless the candidate or existing bullets support it; when they assert it, phrase narrowly and honestly. |

## Trigger phrases

Treat this skill as applicable when the user says things like:
- “Tailor this resume for [ROLE] at [COMPANY]”
- “Rewrite my bullets for this job”
- “Optimize this LaTeX resume for this posting”
- “Make a version focused on [backend/ML/infrastructure/etc.]”

