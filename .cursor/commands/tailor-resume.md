## tailor-resume

Tailor my LaTeX resume in this repo to a specific role, using:
- the `latex-resume-tailoring` **rule** in `.cursor/rules/latex-resume-tailoring.mdc`, and
- the `latex-resume-tailoring` **skill** in `.cursor/skills/latex-resume-tailoring/SKILL.md`.

Steps:
- If the argument is a URL, fetch the job posting from that link and extract the full job description from the page.
- If the argument is free text, treat it as the full job description.
- From the resulting description, extract role, seniority, core responsibilities, and 8–12 key skills/keywords.
- Follow `resume_guidelines.txt` for bullet style, bolding, and impact/metrics.
- Directly edit the relevant `.tex` files (e.g., `sections/projects/*.tex`, `sections/experience/*.tex`, `sections/skills.tex`, `sections/courses.tex`, etc.) with minimal diffs to best match the job, without inventing experience.
- Automatically:
  - Keep the resume to a **single page** by trimming low-impact bullets and commenting out lower-priority sections if needed.
  - Select up to **3 projects** that best match the posting and comment out the rest in `projects.tex`.
  - Render `Relevant Courses` as at most one line with 3–6 high-signal courses, commenting out the rest in `courses.tex`.
  - Ensure every skill listed in `Technical Skills` has clear “proof” in at least one bullet, commenting out unproven skills.
- Rebuild the resume if needed and summarize which sections changed and why in the chat.

Input formats:
- `/tailor-resume <paste full job description here>`
- `/tailor-resume <job posting URL>`

Behavior:
- Assume the argument immediately after the command is either a job posting URL or a raw job description.
- If the fetched or provided description seems partial, still proceed with best-effort tailoring instead of asking me for more details.
