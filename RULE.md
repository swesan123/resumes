## Resume Tailoring Rule for This Repository

- **Scope**
  - This rule applies to all work done in this repository to create or update my resume and related LaTeX assets.

- **Source of Truth**
  - Always treat `resume_guidelines.txt` in this repo as the canonical specification for how my resume bullet points should look, read, and be structured.
  - Before making substantial changes to the resume, read `resume_guidelines.txt` and follow its requirements over any generic advice.
  - If there is ever a conflict between generic resume best practices and `resume_guidelines.txt`, prefer `resume_guidelines.txt` unless it is clearly broken or inconsistent with my explicit instructions in the current chat.

- **When I Paste a Job Posting**
  - When I paste a job description and ask to tailor my resume:
    - Extract the role, level, key responsibilities, and 8–12 core skills/keywords from the posting.
    - Use `resume_guidelines.txt` to decide bullet style, phrasing, bolding strategy, and impact/metrics expectations.
    - Propose or apply concrete changes to my LaTeX resume files (for example, `main.tex`, `sections/*.tex`) that best align my experience with the job while staying honest.

- **Bullet Point Expectations**
  - Follow the structure and guidance from `resume_guidelines.txt`, in particular:
    - Focus bullets on impact, not duties (what I did, how I did it, and why it mattered).
    - Use the pattern: Action Verb + What I built/changed + Technologies used + Measurable result.
    - Aggressively look for ways to quantify impact (latency, throughput, errors, scale, revenue, cost, engagement, etc.).
    - Use LaTeX bolding (for example, `\\textbf{Python}`, `\\textbf{React}`, `\\textbf{AWS}`) for the most important 2–4 technologies or skills per bullet, balancing keyword visibility with visual cleanliness.

- **Editing Behavior**
  - Maintain the existing LaTeX structure, macros, and visual style; prefer minimal-diff edits instead of full rewrites, unless I explicitly request a larger redesign.
  - Keep bullets concise (ideally 1–2 lines) and avoid vague language or weak verbs as described in `resume_guidelines.txt`.
  - Reorder or emphasize sections (projects, education, experience, skills, etc.) only when it clearly improves alignment with the target role and does not violate `resume_guidelines.txt`.
  - Do not invent experience, skills, or technologies I do not have; instead, highlight and rephrase what is already true about my background using the guidelines.

- **Interaction Style**
  - Default to directly editing the LaTeX files in this repo rather than only suggesting text, unless I explicitly say otherwise.
  - After edits, briefly summarize:
    - Which sections changed.
    - How those changes were informed by the job posting and by `resume_guidelines.txt`.

