---
framework_version: 1.0.0
---

# Agent Guidelines: GetJobIQ

This workspace is structured to manage job search activities, scraper tools, CVs, cover letters, and interview preparation.

## Thin-Pointer Design (Single Source of Truth)

To prevent duplication and configuration drift across different AI agent frameworks (Claude Code, Google Antigravity, Codex, Cursor, Gemini CLI, etc.), this workspace uses a unified thin-pointer design. All agent runtimes should load the canonical specifications and candidate profiles from the files and directories below:

1. **Personal Candidate Profile:**
   - The candidate profile, contact details, education, and target preferences are defined in [01-FRAMEWORK/CLAUDE.md](01-FRAMEWORK/CLAUDE.md) and the individual profile methodology files under [01-FRAMEWORK/.claude/skills/job-application-assistant/](01-FRAMEWORK/.claude/skills/job-application-assistant/) (specifically `01-*.md` etc.).
2. **Canonical Workflow Specifications:**
   - The step-by-step instructions and triggers for tasks (setup, scrape, rank, apply, upskill, interview) are defined in the [01-FRAMEWORK/.claude/](01-FRAMEWORK/.claude/) directory (specifically under `01-FRAMEWORK/.claude/skills/` and `01-FRAMEWORK/.claude/commands/`).
   - Do not duplicate these rules or specifications. Treat `01-FRAMEWORK/.claude/` files as the single source of truth.
3. **Portal Search Skills:**
   - Job-portal search CLIs live under [01-FRAMEWORK/.agents/skills/](01-FRAMEWORK/.agents/skills/) in the portable Agent Skills format (with a `SKILL.md` per portal). Codex and Antigravity discover these automatically; the `/scrape` workflow in [01-FRAMEWORK/.claude/skills/job-scraper/](01-FRAMEWORK/.claude/skills/job-scraper/) orchestrates them.
