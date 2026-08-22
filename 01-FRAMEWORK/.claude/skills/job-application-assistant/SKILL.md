---
name: job-application-assistant
description: >
  Assists with job applications: evaluating job postings, tailoring CVs, writing cover letters,
  and preparing for interviews. Triggers on keywords like: job posting, job application, CV,
  cover letter, resume, interview prep, job fit, career, application, apply, ansøgning, stilling
allowed-tools: Read, Glob, Grep, WebFetch, WebSearch, Bash, Edit, Write, AskUserQuestion
framework_version: 1.3.4
---

# Job Application Assistant

---

## Workflow

When the user provides a job posting (URL or text), follow this workflow:

### Step 1: Research & Evaluate Fit
- Fetch the job posting content (use WebFetch for URLs). **A 403 is not a dead end** - follow the escalation order in `09-web-research.md` before concluding a page is unavailable, and prefer the employer's own careers posting over an aggregator listing
- Keep the **full posting text verbatim** for Step 3b to archive - never a summary
- Analyze the posting for required competencies, keywords, and priorities
- Research the company (website, LinkedIn, mission, recent news), per `09-web-research.md`
- Score the posting against the candidate's profile using the framework in `04-job-evaluation.md`
- Present the evaluation table and verdict
- Suggest whether the candidate should call the employer before applying (see `04-job-evaluation.md` for guidance)
- Ask the user if they want to proceed with an application

**Filename and path derivation (used by Steps 2, 3, and 3b):** `<company>`, `<role>`, and `<name>` are derived separately by the **Subfolder and filename naming** rule in `02-Documents/README.md` — the same rule `/apply` Step 2 uses, so this path and `/apply`'s never diverge. `<name>` comes from `01-candidate-profile.md`'s `**Name:**` line; if it is still the unfilled placeholder `[YOUR_NAME]` or sanitizes to empty, stop and ask the user to run `/setup` first — do not create a file with a garbage or placeholder segment. Compose `<CV_FILENAME>` = `<company>_<role>_<name>_CV` and `<COVER_FILENAME>` = `<company>_<role>_<name>_cover`. Resolve `<CV_EXT>`/`<COVER_EXT>` the same way `/apply` Step 2 does: if `05-cv-templates.md`/`06-cover-letter-templates.md` opens with an `ACTIVE-TEMPLATE` managed block (inserted by `/add-template`), use its declared source extension; otherwise use the stock `.tex` default. Derive these once per invocation and reuse across whichever steps run.

### Step 2: Tailor CV
- Derive `<CV_FILENAME>` and `<CV_EXT>` as described above (skip if already derived this invocation).
- Read the most relevant existing CV variant from `02-Documents/applications/*/*/CV/*_CV*<CV_EXT>` as a starting point
- Follow the guidelines in `05-cv-templates.md`
- Create `02-Documents/applications/<company>/<role>/CV/<CV_FILENAME><CV_EXT>` with tailored content
- Adjust: profile statement, skills section, experience bullet emphasis, section order

### Step 3: Write Cover Letter
- Derive `<COVER_FILENAME>` and `<COVER_EXT>` as described above (skip if already derived this invocation).
- Follow the writing style rules in `03-writing-style.md` (critical: no em-dashes, no cliches)
- Follow the template structure in `06-cover-letter-templates.md`
- Create `02-Documents/applications/<company>/<role>/cover_letter/<COVER_FILENAME><COVER_EXT>`
- Ensure the letter connects specific experience to the role requirements

### Step 3b: Record the Application
- Run this once both documents exist. A CV or cover letter drafted alone is not yet an application.
- Follow **`/apply` Step 6b** (`01-FRAMEWORK/.claude/commands/apply.md`) exactly: same header, same match-then-update rule, same `drafted` row, same posting archive, same prohibition on touching `job_scraper/seen_jobs.json`. It is stated there once so the two paths cannot drift. Four of its values are named in `/apply`'s own terms: `cv_file`/`cover_letter_file` are the paths written in Steps 2 and 3 here, `source` is the posting URL from Step 1, `deadline` is the application deadline from the posting text Step 1 keeps verbatim (empty when the posting states none - never guess one), and the posting text item 7 archives is the one Step 1 read.
- This step exists here because `/scrape` Step 5 routes straight into this skill. Without it, that path writes two documents and records nothing.

### Step 4: Interview Preparation
- Follow the framework in `07-interview-prep.md`
- Prepare STAR-format answers for likely questions
- Identify role-specific talking points
- Draft questions the candidate should ask the interviewer

---

## Reference Files

| File | Purpose |
|------|---------|
| `01-candidate-profile.md` | Education, experience, skills, publications, awards |
| `02-behavioral-profile.md` | Behavioral assessment, strengths, ideal environments |
| `03-writing-style.md` | Tone, structure, do's and don'ts |
| `04-job-evaluation.md` | Scoring framework for job fit |
| `05-cv-templates.md` | LaTeX CV structure and tailoring rules |
| `06-cover-letter-templates.md` | LaTeX cover letter structure and tailoring rules |
| `07-interview-prep.md` | STAR examples, tough questions, roleplay guidelines |
| `08-application-forms.md` | Portal free-text fields: self-introduction, project entries, character-limited pitches |
| `09-web-research.md` | Fetching postings and company pages: trust boundary, the WebFetch 403 fallback, escalation order, claim verification |

---

## Quick Commands

The user may also ask for individual steps without the full workflow:
- "Evaluate this job posting" - Step 1 only
- "Write a CV for [company]" - Step 2 only
- "Write a cover letter for [role] at [company]" - Step 3 only
- "Help me prepare for an interview at [company]" - Step 4 only
- "What jobs should I look for?" - Career strategy discussion using profile + evaluation framework
