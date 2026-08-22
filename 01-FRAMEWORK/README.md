# GetJobIQ

**An AI-native job application framework that runs entirely on your own machine.**

GetJobIQ turns any AI coding agent (Claude Code, and — via the portable
Agent Skills format — Codex, Cursor, Gemini CLI, Antigravity) into a career
assistant: it evaluates job postings against your real profile, tailors your
CV and cover letter, tracks every application, and preps you for interviews.
No servers, no third-party dashboards, no uploading your resume anywhere —
everything lives in this repo, on your disk, under your control.

## Why GetJobIQ

Most "AI job application" tools either auto-apply to hundreds of jobs
indiscriminately, or lock your data into a SaaS dashboard. GetJobIQ does
neither:

- **You stay in the loop.** Every application is evaluated, drafted, and
  reviewed with you — nothing is auto-submitted.
- **Nothing is fabricated.** Every claim in a generated CV or cover letter
  is grounded in your own verified profile. A Factual Grounding Audit
  strips anything that isn't.
- **Portable by design.** The candidate data and methodology live in plain
  markdown, readable by any AI agent — not locked to one vendor.
- **Honest about gaps.** Requirements you don't meet are acknowledged, not
  hidden or keyword-stuffed.

## What it does

| Command | What it does |
|---|---|
| `/setup` | Builds your candidate profile from your documents or a guided interview |
| `/scrape` | Searches job portals and company career pages for matching roles |
| `/rank` | Scores and ranks discovered postings against your profile |
| `/apply <posting>` | Evaluates fit, drafts a tailored CV + cover letter, records the application |
| `/interview <company>` | Builds a stage-specific interview prep pack |
| `/outcome <company>` | Records the result and keeps your tracker current |
| `/add-portal`, `/add-template` | Extend the framework with new job boards or CV/letter designs |
| `/gmail-sync`, `/notion-sync`, `/html-report` | Optional integrations to keep your search organized |

## Repository structure

```
01-FRAMEWORK/                         # This folder
  .claude/
    skills/job-application-assistant/ # Evaluation, CV/letter rules, interview prep
    skills/job-scraper/               # Search strategy, portal orchestration
    commands/                         # Slash commands (/apply, /rank, /outcome, ...)
  .agents/skills/                     # Portable job-portal search CLIs (multi-agent)
  CLAUDE.md                           # Your candidate profile (populated by /setup)
  AGENTS.md                           # Cross-agent compatibility pointer
02-Documents/                         # Source materials + per-application archive
04-APPLICATIONS/cv/, cover_letters/   # Stock LaTeX templates (tailored output lives under 02-Documents/applications/<company>/<role>/)
```

## Getting started

1. Clone the repo:
   ```bash
   git clone https://github.com/<your-username>/GetJobIQ.git
   cd GetJobIQ
   ```
2. Open it in Claude Code (or your preferred AI coding agent).
3. Run `/setup` and follow the prompts — it builds your profile from your
   existing CV/LinkedIn export, or a short interview if you don't have one.
4. Paste a job posting and run `/apply` to see the full workflow end to end.

### Requirements

- An AI coding agent with markdown-based skill/command support (built and
  tested on Claude Code; portable to others via `AGENTS.md`)
- A LaTeX distribution (for compiling the CV/cover-letter templates) —
  `lualatex` and `xelatex`
- Optional: `pdftotext` (poppler) for ATS-parseability verification

## Privacy

Your candidate data (`CLAUDE.md`, tailored CVs, application history) is
personal and should not be committed publicly as-is. Add a `.gitignore`
before your first `/setup` run if you plan to keep this repo public, or
keep your fork private.

## Status

This framework is under active development. See [CONTRIBUTING.md](CONTRIBUTING.md)
for how to add a job portal, a CV template, or report an issue.

## License

<!-- Add your chosen license here, e.g. MIT -->