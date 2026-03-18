---
name: latex-resume-verification
description: Verifies a tailored LaTeX resume against a specific job posting using resume_guidelines.txt, computes a match score, and suggests targeted improvements.
---

# LaTeX Resume Verification

## When to use this skill

- After `/tailor-resume` has been run for a given job posting.
- When the user asks for a **match score** or wants to know if further improvements are possible.
- Before sending or uploading the resume for a specific role.

## Verification workflow

1. **Re-extract job signal**
   - From the provided job posting (text or URL), extract:
     - Role/title and level.
     - 8–12 core skills/keywords (languages, tools, domains, OS, etc.).
     - 3–5 core responsibilities (what the role actually does day to day).

2. **Load guidelines and current resume**
   - Read `resume_guidelines.txt` and treat it as the canonical checklist.
   - Read the relevant LaTeX sections:
     - `sections/skills.tex`
     - `sections/experience/*.tex`
     - `sections/projects/projects.tex` and the referenced project files
     - `sections/courses.tex`

3. **Section-by-section checks**
   - **Technical Skills**
     - Confirm that each listed skill is supported by at least one bullet in Experience, Projects, or Courses.
     - Flag any skills that are neither mentioned in bullets nor clearly implied by coursework.
   - **Experience**
     - Check that at least one role strongly supports the top JD skills and responsibilities.
     - Ensure bullets follow guidelines: action + what changed + tech + outcome/metric, 1–2 lines, no brackets.
   - **Projects**
     - Confirm there are at most 3 projects shown and each has at most 2 bullets.
     - Check that chosen projects provide “proof” for important JD skills (e.g., Python, testing, automation, OS).
   - **Courses**
     - Confirm that `Relevant Courses` is rendered as a single `\item` line with 3–6 short course names.
     - Ensure selected courses support the role (e.g., real-time, systems, OS, hardware, testing).

4. **Bolding and keyword alignment**
   - Build the set of JD skills/keywords.
   - For each bolded technology or skill (`\textbf{...}`):
     - Check whether it appears in the JD or is a clear synonym.
     - If not, treat it as a candidate for de-bolding to reduce noise.
   - Confirm that the **most important JD skills** (e.g., main language/OS) are bolded at least once in relevant bullets or headings.

5. **Layout and one-page constraints**
   - Ensure:
     - The resume compiles to a single page for this template.
     - Optional sections and long lists (skills, courses) have been trimmed or commented out when needed.
   - If likely overflowing:
     - Identify specific bullets or sections to trim following the heuristics in `resume_guidelines.txt`.

6. **Compute match score**
   - Compute a 0–100 score using:
     - ~40%: coverage of top JD skills/keywords on the page.
     - ~30%: coverage of core responsibilities in Experience/Projects.
     - ~30%: adherence to `resume_guidelines.txt` (style, bolding, layout, no brackets).
   - Optionally run up to **3 quick “what-if” adjustments** (e.g., tweak bolding, swap one project, adjust one bullet) and recompute, using the best score.

7. **Report back**
   - Return:
     - The final match score.
     - 3–6 concrete observations (e.g., “Python is present and bolded”, “Docker is listed but not in JD – consider de-bolding”).
     - 3–5 **targeted suggestions** if the score is below ~85, referencing specific sections/files to tweak.

