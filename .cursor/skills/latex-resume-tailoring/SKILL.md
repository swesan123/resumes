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
     - Main responsibilities
     - 8–12 core skills or keywords (technologies, domains, tools)
   - Keep these visible while editing.

2. **Load project canon**
   - Read `resume_guidelines.txt`.
   - Assume the `.cursor/rules/latex-resume-tailoring.mdc` rule is active and authoritative.
   - Prefer these over generic resume advice unless the user explicitly overrides.

3. **Plan edits**
   - Identify which sections to adjust:
     - Experience bullets
     - Projects bullets
     - Skills or summary sections, if present
   - Favor **minimal-diff** LaTeX edits: adjust existing bullets before adding or removing bullets.

4. **Edit bullets**
   - For each changed or new bullet:
     - Follow the structure: **Action verb + what was built/changed + key technologies + measurable result**.
     - Focus on **impact and metrics**, not duties.
     - Use `\textbf{...}` for only **2–4 critical skills or keywords per bullet**.
     - Do **not invent** responsibilities, scope, or technologies that are not already demonstrated in the user’s experience.
   - Keep bullets **1–2 lines** each.

5. **Apply changes**
   - Edit the relevant `.tex` section files directly in this repo.
   - Preserve existing LaTeX structure and macros.
   - Keep diffs as small as possible while achieving the tailoring goals.

6. **Summarize for the user**
   - Briefly describe:
     - Which sections changed (for example, “Updated 3 bullets in projects, 2 in experience”).
     - How the edits align with the job’s key responsibilities and keywords.
   - Avoid repeating large LaTeX code blocks in the summary.

## Trigger phrases

Treat this skill as applicable when the user says things like:
- “Tailor this resume for [ROLE] at [COMPANY]”
- “Rewrite my bullets for this job”
- “Optimize this LaTeX resume for this posting”
- “Make a version focused on [backend/ML/infrastructure/etc.]”

