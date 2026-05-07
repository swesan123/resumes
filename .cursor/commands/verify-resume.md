## verify-resume

Verify that my current LaTeX resume in this repo matches a specific job posting, using:
- the `latex-resume-tailoring` **rule** in `.cursor/rules/latex-resume-tailoring.mdc`,
- the `latex-resume-tailoring` **guidelines** in `resume_guidelines.txt`, and
- the `latex-resume-verification` **skill** in `.cursor/skills/latex-resume-verification/SKILL.md`.

Steps:
- If the argument is a URL, fetch the job posting from that link and extract the full job description from the page.
- If the argument is free text, treat it as the full job description.
- Re-extract the role, 8–12 core skills/keywords, and core responsibilities from the posting.
- Compare the job signal against:
  - Experience bullets,
  - Projects and their bullets,
  - Technical Skills,
  - Relevant Courses.
- Check that:
  - The most important job skills and responsibilities appear on the page (ideally in bullets).
  - Bolded skills only include technologies that appear in the job description (or clear synonyms).
  - Every skill listed in `Technical Skills` has “proof” in at least one bullet or course.
  - **Technical Skills ↔ employer keyword line**: tokens in the experience `\resumeSubheadingProminent` pipe match what is (still) listed in `skills.tex`; no orphan keywords; **BIOS/UEFI**-style merged synonyms are not duplicated as separate BIOS and UEFI unless intentional.
  - **Metrics**: key numbers are plausible, not orphaned (each major % or time-save has a clear “why”), and bolding of quantities matches the repo’s tailoring guidance.
  - The resume respects `resume_guidelines.txt` (1–2 line bullets, simple language, no brackets, one page, **exactly 3 projects**, Technical Skills with bold **only** on category labels, single-line courses).
- **Technical verification** (when you can run tools): compile `main.tex`, confirm **one page** and **no errors**, scan for **`Overfull \hbox`**, read **`config/commands.tex`**, and **`grep` custom heading macros** so **argument counts match** call sites (see `.cursor/skills/latex-resume-verification/SKILL.md`).
- Check **employer keyword lines** stay keyword-heavy (not soft-skill slogans); coordination belongs in **bullets** unless the user insisted otherwise.
- Compute a **0–100 match score** based on skills coverage, responsibility coverage, and guideline compliance; apply **hard penalties** for compile/layout failures or macro/skill proof violations per the verification skill.
- If the score is below ~85, suggest 3–5 targeted edits (and, when appropriate, apply small, safe fixes such as de-bolding noisy skills or trimming an extra bullet).

Input formats:
- `/verify-resume <paste full job description here>`
- `/verify-resume <job posting URL>`

Behavior:
- Assume the argument immediately after the command is either a job posting URL or a raw job description.
- Do not ask me extra questions unless the job text is clearly unusable; instead, perform a best-effort verification and report findings.

