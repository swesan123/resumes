## tailor-resume

Tailor my LaTeX resume in this repo to a specific role, using:
- the `latex-resume-tailoring` **rule** in `.cursor/rules/latex-resume-tailoring.mdc`, and
- the `latex-resume-tailoring` **skill** in `.cursor/skills/latex-resume-tailoring/SKILL.md`.

Steps:
- If the argument is a URL, fetch the job posting from that link and extract the full job description from the page.
- If the argument is free text, treat it as the full job description.
- Save the **full posting text** to `postings/<CompanyDir>/` when the employer is known, or `postings/` at repo root when unknown, using the naming and `#` header conventions in `.cursor/skills/latex-resume-tailoring/SKILL.md` (do not overwrite existing files unless asked).
- From the resulting description, extract role, seniority, employer (when known), core responsibilities, and 8–12 key skills/keywords.
- Follow `resume_guidelines.txt` for bullet style, bolding, and impact/metrics.
- Directly edit the relevant `.tex` files (e.g., `sections/projects/*.tex`, `sections/experience/*.tex`, `sections/skills.tex`, `sections/courses.tex`, etc.) with minimal diffs to best match the job, without inventing experience.
- Automatically:
  - **Default to a two-page resume** (standard); do **not** force one page unless the user explicitly asks. Never merge project bullets to one line for layout.
  - Enable the **best-matching 7–8 projects** for the posting (lead with 3–4 strongest matches, then add breadth projects); comment out the rest in `projects.tex`. Keep **2–3 bullets per active project** to fill both pages with substance—never trim bullets to save space.
  - In Technical Skills, bold **only** category labels, not the technologies after each colon.
  - Keep **Technical Skills** aligned with **honest proof** (Experience, Projects, Courses): list technologies and domains, put **how you worked** (OOB sessions, bring-up campaigns, etc.) primarily in **bullets**; merge synonyms such as **`BIOS/UEFI`** instead of duplicating; when you remove a token from `skills.tex`, remove it from the employer **keyword pipe** too (or restore proof in the same pass).
  - Prefer **metrics** with a short **because** (mechanism or scope); **bold** headline quantities when consistent with `resume_guidelines.txt` and layout; do not fabricate numbers.
  - Render `Relevant Courses` as at most one line with 3–6 high-signal courses, commenting out the rest in `courses.tex`.
  - Ensure every skill listed in `Technical Skills` has clear “proof” in at least one bullet, commenting out unproven skills.
- Rebuild `main.tex`, then export the compiled PDF per `resume_guidelines.txt`: **`mkdir -p resumes/<CompanyDir>`** when the employer is known, then save **`resumes/<CompanyDir>/<Role-With-Dashes>-YYYY-MM-DD.pdf`**; if the employer is unknown, save **`resumes/<Role-With-Dashes>-YYYY-MM-DD.pdf`** at repo root only.
- **Quality gates** (see `.cursor/skills/latex-resume-tailoring/SKILL.md` → *Compile gate*, *Experience heading rules*):
  - Compile until **no errors** and **`Overfull \hbox`** on headings is addressed (1–2 pages OK; default 2 for dense resumes).
  - Keep employer keyword pipes **tech/domain-heavy**; put coordination and soft themes in **bullets** unless the user explicitly overrides.
  - Keep **Technical Skills** bolding per `resume_guidelines.txt` (category labels only after each colon).
  - Any change to macro **arity** in `config/commands.tex` requires **`grep` all call sites** updated together.
  - Every visible skill needs **bullet or course proof**; call out remaining JD gaps honestly in the summary.
- After tailoring, **`missing_skills.txt`**: increment or append **`Skill: count`** lines for JD skills that still lack honest proof anywhere on the finalized resume **before** summarizing—see `.cursor/skills/latex-resume-tailoring/SKILL.md` → step **10**.
- Summarize paths (`postings/…`, `resumes/…`), which sections changed, and why (**include which ledger slots in `missing_skills.txt` were updated).

Input formats:
- `/tailor-resume <paste full job description here>`
- `/tailor-resume <job posting URL>`

Behavior:
- Assume the argument immediately after the command is either a job posting URL or a raw job description.
- If the fetched or provided description seems partial, still proceed with best-effort tailoring instead of asking me for more details.
