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
   - **Directory layout**: When the employer is known, save under **`postings/<CompanyDir>/`** using a stable company slug (e.g. `AMD`, `NVIDIA`, `Canonical`, matching `resumes/<CompanyDir>/` when practical). When the employer cannot be determined reliably, save at **`postings/`** repo root next to other unknown-company files.
   - **Filename pattern**: `<short-role-slug>_<company-slug>-YYYY-MM-DD.txt` (same basename whether nested or flat)
     - Slugs: ASCII, spaces → underscores, strip unsafe punctuation (keep readability).
     - If company is unknown, omit `_<company-slug>` (still include date).
     - Examples (conceptually): `postings/AMD/Graphics_IP_Validation_Engineer_AMD-2026-03-18.txt`, `postings/NVIDIA/Hardware_Applications_Engineer_New_College_Grad_NVIDIA-2026-05-04.txt`.
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
   - **Skills vs. responsibilities (repo feedback)**:
     - List **repeatable technologies, platforms, and domains** under Technical Skills only when at least one **Experience, Project, or Courses** line proves them.
     - Describe **how work was done** (e.g. out-of-band consoles, integration bring-up, customer rack configs) in **bullets**; do not treat those phrases as interchangeable “skill tags” unless the candidate keeps them as proof-backed tokens. If removed from `skills.tex`, remove matching tokens from the employer **keyword pipe** (or add honest proof in the same pass).
     - **Merge synonyms on the page**: e.g. use **`BIOS/UEFI`** instead of separate **BIOS** and **UEFI** when they represent the same resume story; align the same token in bullets and in `\resumeSubheadingProminent` when both appear.
     - **Umbrella nouns** (e.g. “firmware” as a category): prefer mentioning firmware **inside bullets** (flash, GPU firmware, triage). Add **firmware** as a comma-separated skill only if it is consistently proven and not redundant with **BIOS/UEFI** rows.
   - **Metrics in bullets (repo feedback)**:
     - When the resume includes **counts, time ranges, or percentages**, **bold the headline metric** if the layout still looks balanced (e.g. **100+ runs weekly**, **about 20 minutes to under 5 minutes**).
     - Pair **percent or time-save claims** with a short **because** clause (mechanism, scope, or before/after behavior) so recruiters and hiring managers can see why the number is credible—do **not** invent metrics; when the candidate only has a qualitative win, keep prose only.
   - **Bullet clarity (repo feedback)**:
     - Write so a **non-specialist recruiter** can follow the first half of the line, while **JD keywords** remain visible for a technical reader.
     - Avoid **two experience bullets** that repeat the same OS list and the same validation-pillar list—split **test orchestration / ownership** from **platform imaging / BMC / BIOS** (or similar) so each line has a distinct job.
   - **Projects**:
     - Enable **exactly 3 projects** (minimum **3**, maximum **3**): pick the three best matches for the posting unless the user explicitly requests four.
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
   - **Compile gate (mandatory)**:
     - Run `latexmk` or `pdflatex` until the log is clean of **LaTeX errors** and the PDF is **exactly one page** (`Output written on main.pdf (1 page, …)`).
     - Scan the log for **`Overfull \hbox`** on experience headings; if present, shorten the keyword line, adjust wrapping in `config/commands.tex`, or reduce duplicate tokens—do **not** ship obvious layout breakage.
     - If you change any macro in `config/commands.tex` (especially argument count), **`grep` all call sites** (e.g. `\resumeSubheadingProminent`) and update every invocation in the same edit pass.
   - **PDF basename**: `<Role-With-Dashes>-YYYY-MM-DD.pdf` (spaces → `-`; shell-safe).
   - **Destination**:
     - When the **employer is known** from the posting or URL, choose `<CompanyDir>` as a stable folder slug under `resumes/` (match existing folders when names align: `AMD`, `Wealthsimple`, `Capital One`, `Canonical`, etc.).
     - Run **`mkdir -p resumes/<CompanyDir>`** so the folder exists, then copy `main.pdf` to **`resumes/<CompanyDir>/<Role-With-Dashes>-YYYY-MM-DD.pdf`**.
     - When the **employer cannot be determined reliably**, save only at the repo root: **`resumes/<Role-With-Dashes>-YYYY-MM-DD.pdf`** (no generic company guess).

10. **Update `missing_skills.txt` (mandatory)**
   - Read `resume_guidelines.txt` → *Missing skills tracking* plus the ledger format described at the top of `missing_skills.txt`.
   - From the JD, take the **8–12 important skills/tools/domains** you used for tailoring.
   - For each one, decide if the **final** `main.tex` stack (Education, Skills, Experience, Projects, Courses, employer keyword line) provides **honest proof** beyond name-dropping—that is: a bullet, explicit course selection, project, or truthful skill row grounded in proof.
   - For each JD item that is **still missing** after tailoring (never invent proof to satisfy this):
     - **Increment** its counter by **1**, or append `Canonical skill name: 1` if no line exists.
     - **At most one increment per JD skill per tailoring pass**, even if the gap appears twice in the JD.
     - **Do not** increment for skills you consciously omitted despite having proof elsewhere on the resume.
   - Canonical names: spell out recognizable industry labels (merge obvious synonyms onto one label, e.g. “Debussy / Verdi-class trace debug”).
   - After edits, rewrite the skill lines so **`Name: N`** entries appear in **ASCII ascending order by `Name`** (case-insensitive). Keep `# …` instructional comments grouped at the top of the file.
   - Mention in the chat summary **which ledger keys moved** (+1 added or new rows).

11. **Summarize for the user**
   - Briefly describe:
     - Path to the saved posting (`postings/…`) and exported PDF (`resumes/…`).
     - Which sections changed (for example, “Updated 3 bullets in projects, 2 in experience”).
     - How the edits align with the job’s key responsibilities and keywords.
     - Any **honest gaps** still missing vs the JD (e.g. Snowflake, C++ without proof)—do not hide these if you dropped a keyword from the page.
     - **`missing_skills.txt`**: note which **`Name`** rows gained `+1` or were newly added in step **10**.
   - Avoid repeating large LaTeX code blocks in the summary.

## Experience heading rules (`sections/experience/*.tex`, `config/commands.tex`)

These rules prevent recurring layout and content clashes:

- **Default macro**: Most employers use `\resumeSubheading` (**4 arguments**). This repo may use `\resumeSubheadingProminent` for long keyword rows—today it is defined as **5 arguments**: company line, keyword tail (usually starting with `$|$`), location, role title, dates. Never change arity without updating **every** call site.
- **What belongs in the keyword pipe**: Technologies, platforms, and domains that mirror **Technical Skills** or the JD (e.g. languages, BMC/BIOS, OSes, frameworks). Prefer ordering keywords **consistently with the skill rows** (Languages → Integration hardware → Systems → Validation → …) unless the user specifies otherwise.
- **What does *not* belong in the pipe**: Soft-skill or prose slogans (e.g. “cross-functional collaboration,” mission statements). Put coordination, stakeholder work, and documentation wins in **bullets** unless the user explicitly demands a phrase in the header.
- **Technical Skills vs employer line**: If `resume_guidelines.txt` / this skill say category-label bold only in **Technical Skills**, that rule **still applies there** even when the user asks to bold tokens after the colon—explain the constraint and use bullets or the employer keyword line for emphasis instead.
- **New claims**: Adding a language or tool to **Technical Skills** or the employer keyword line requires an honest mention in at least one **Experience or Project bullet** in the same tailoring pass (or comment the skill out).

## Lessons learned (avoid common regressions)

Short checklist distilled from real tailoring rounds:

| Issue | What to do instead |
|--------|---------------------|
| Fewer than **3** projects shown | Always enable **exactly 3** `\input{...}` lines in `projects.tex` unless the user explicitly asks for a different count; never ship 2 “to save space” without trimming elsewhere first. |
| Bold inside Technical Skills lists | Category label bold only; technologies after `:` stay roman. Employer/experience **subheading** lines may still bold top JD keywords—that pattern is separate from the Skills block. |
| Silent **bullet replacement** | Comment prior `\resumeItem` text above the new bullet when it would otherwise disappear from the file. |
| Redundant skill pillars | Merge synonyms (RAS vs resets) after confirming with the candidate’s terminology. |
| Inconsistent domain capitalization | Align Skills and Experience on the same capitalized names for formal axes (Performance, System Management, IO). |
| **Two-page** PDF after skills tweak | Merge skill rows or shorten wording before trimming projects below three. |
| Same JD tailored twice | Do not overwrite the existing `postings/<CompanyDir>/…` or `postings/*.txt`; point to the existing file and refresh `main.pdf` / export only. |
| Missing `resumes/<Company>/` | When the employer is known, **`mkdir -p`** that folder and drop the PDF inside; root-level `resumes/<Role>-<date>.pdf` is only for unknown employers. |
| Invented tooling claims | Never add MAAS/ROCm/etc. unless the candidate or existing bullets support it; when they assert it, phrase narrowly and honestly. |
| **Soft skills / coordination** stuffed into employer keyword header | Keep the pipe **tech- and domain-heavy**; express collaboration, leadership, and documentation in **bullets**. |
| **`Overfull \hbox`** on `\resumeSubheadingProminent` | Use the split-size layout in `config/commands.tex`, shorten keywords, or widen/wrap the left column—recompile until clean. |
| **Macro arity drift** | Changing `\newcommand{\resumeSubheadingProminent}[N]` without fixing **all** `\resumeSubheadingProminent{...}` blocks causes silent or noisy failures—grep and fix in one changeset. |
| **User requests violate `resume_guidelines.txt`** | Follow repo canon (e.g. no post-colon bold in Technical Skills). Say what you did instead and **do not** silently break formatting rules. |
| **BIOS and UEFI doubled** | Use **`BIOS/UEFI`** in Technical Skills, employer keyword line, and bullets for a single consistent token. |
| **Header lists token dropped from `skills.tex`** | Re-add proof and the skill, or remove the orphan token from `\resumeSubheadingProminent` in the same edit. |
| **Metric without a reason** | Add a brief mechanism, qualification, or scope clause—or drop the number if it cannot be defended. |
| Forgot **`missing_skills.txt`** after tailoring | Mandatory step **10**: increment or append missing JD skills lacking honest proof before final summary. |

## Trigger phrases

Treat this skill as applicable when the user says things like:
- “Tailor this resume for [ROLE] at [COMPANY]”
- “Rewrite my bullets for this job”
- “Optimize this LaTeX resume for this posting”
- “Make a version focused on [backend/ML/infrastructure/etc.]”

