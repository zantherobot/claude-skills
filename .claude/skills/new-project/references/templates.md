# Template Files Reference

These are the standard boilerplate files for new projects. Adapt the contents
based on the project details gathered during setup.

---

## CLAUDE.md

The CLAUDE.md has two parts: **universal standards** (copy verbatim into every
project) and **project-specific sections** (fill in based on the stack discussion).

Lines marked `[FILL IN ...]` must be replaced with project-specific content.
Lines marked `[IF ...]` are conditional — include only when the condition applies.
Everything else is verbatim — do not rephrase or rearrange.

````markdown
# [PROJECT_NAME] — Developer Guide for Claude Code

This file tells Claude how to work on this repository. For what the product does
and the project file structure, see SPEC.md.

## Running the App Locally

[FILL IN — exact shell commands to install deps and run locally.]

## Running Tests

[FILL IN — exact test, lint, and format commands for this stack.]

All checks must pass before committing. If you add or change any logic, add
corresponding tests in `tests/`.

## Development Standards

These apply to every change.

### Testing

- Every change must include or update tests covering the modified behaviour.
- All existing and new tests must pass before committing or deploying.
- [FILL IN — what specifically needs tests in this project, e.g. "Calculation logic (solar production, NEM 3.0 credits, financial math) must have unit tests with known-good expected outputs."]
- Edge cases — [FILL IN — project-specific edge cases, e.g. "zero bill, maximum system size, boundary zip codes, missing inputs"] — must be tested.

### Linting, Formatting & Code Style

[FILL IN — stack-specific rules. Include both linter and formatter. Examples:]

For Python:
- Python code must pass `ruff check` with no errors before committing.
- Run `ruff format` before committing. Do not mix formatting changes with logic changes.
- Follow PEP 8 conventions. Line length limit: 100 characters.

For TypeScript:
- TypeScript code must pass `tsc --noEmit` with no errors before committing.
- Run `npx prettier --write .` before committing. Do not mix formatting changes with logic changes.
- Follow standard TypeScript/React conventions. Use consistent formatting with the existing codebase.

[FILL IN any additional style notes, e.g.:]
- "CSS uses Tailwind utility classes — avoid custom CSS unless truly necessary."

### Spec Stays Current

- If a change modifies user-facing behaviour, inputs, outputs, or calculation logic, update SPEC.md in the same commit.
- Do not let the spec drift from the implementation.

[IF the project has a backend:]

### Input Validation

- All user inputs must be validated server-side, regardless of any client-side checks.
- Invalid inputs return clear, user-friendly error messages — never raw stack traces.

[END IF]

[IF the project uses environment variables:]

### No Secrets in Code

- API keys, credentials, and environment-specific config go in environment variables, never hardcoded in source files.
- The `.env.example` file documents required variables without real values.

[END IF]

### Small, Focused Commits

- Each commit should do one thing. Avoid mixing unrelated changes.
- Commit messages should describe *why*, not just *what*.

### Graceful Error Handling

- Failures (e.g. API down, bad response, unknown input) must display a helpful message to the user and log the error server-side.
- Log errors with enough context to diagnose without reproducing (endpoint, input, status code). Railway captures stdout/stderr.
- The app should never show a raw error page to end users.

### Accessibility & Responsive Design

- Target mobile and desktop viewports.
- Meet WCAG 2.1 AA accessibility guidelines.
- Use semantic HTML elements; add ARIA attributes only when no native element fits.

## Deployment & Branch Workflow

There are two environments, both hosted on Railway:

| Branch    | Environment                    | URL                  |
|-----------|--------------------------------|----------------------|
| `staging` | Staging (Railway staging env)  | Railway staging URL  |
| `main`    | Production (Railway production env) | Railway production URL |

### Default behaviour

- **All work targets `staging` by default.** Commit and push to `staging`; Railway auto-deploys.
- **Never push or merge to `main` without explicit instruction.** Production is only updated when the user asks to promote, or confirms after being asked.

### Promoting staging → production

When asked (or after asking "Ready to promote to production?"):

1. Ensure all checks pass on `staging`.
2. Open and squash-merge a PR from `staging` → `main` — Railway auto-deploys production.

```bash
gh pr create --base main --head staging \
  --title "Promote staging → production" --body ""
gh pr merge --squash
# gh leaves you on staging automatically
```

### Typical feature workflow

```
work on staging branch
       │
       ▼  git push origin staging
   staging  ──► auto-deploys to Railway staging env
       │
       │  (user confirms ready for production)
       ▼  gh pr create + gh pr merge --squash
     main  ──► auto-deploys to Railway production env
```

[OPTIONAL — include only if the project has offline data processing scripts
separate from the runtime application:]

### Data Pipeline

[Describe the scripts, their dependencies, and how to run them.]
````

---

## spec.md

The spec is co-developed with the user in Step 1. Populate it with the answers
gathered during that conversation. It should be a useful starting document — not
exhaustive, but enough that someone reading it understands what the project does,
how it works, and what decisions have been made.

Design decisions and project file structure live HERE, not in CLAUDE.md.

```markdown
# [Project Name] — Product Specification

## Overview

[One or two sentences: what this project does and who it's for.]

## Tech Stack

- **Frontend:** [framework, rendering approach]
- **Backend:** [framework, API style — or "N/A" for static/client-only apps]
- **Data:** [sources, static vs runtime API calls]
- **Testing:** [framework — pytest for Python, Vitest for TS/Node]
- **Formatting:** [tool — ruff format for Python, Prettier for TS/Node]
- **Deployment:** [Railway / other]

[If there's a non-obvious stack choice, add a brief "Why X over Y?" note.]

## Core Features (v1)

[Numbered list of must-have features for the first working version.]

1. ...
2. ...
3. ...

## Data Sources

[For each data source: what it provides, where it comes from, any API keys
or credentials needed. Use a table for multiple sources:]

| Data | Source | Approach in v1 |
|---|---|---|
| ... | ... | ... |

[If the project has no external data sources (e.g. a game), replace this section
with whatever domain-specific reference is needed — e.g. "Dictionary" for a word
game, "Scoring Rules" for a game, etc.]

## Key Calculations & Assumptions

[Document formulas, parameters, and simplifications. Use tables for assumptions:]

| Parameter | Value | Source |
|---|---|---|
| ... | ... | ... |

[If the project doesn't involve calculations, replace this section with the
equivalent domain logic — e.g. "Game Mechanics", "Scoring", etc.]

## Architecture

[Project structure with one-line descriptions. This is the single source of truth
for the file layout — CLAUDE.md points here.]

```
file_or_dir       # Description
tests/            # Test suite
SPEC.md           # Product specification (this file)
CLAUDE.md         # Developer guide for Claude Code
```

## UI / UX

[Describe the layout, key screens, charts, user interactions. Be specific enough
that someone could build the UI from this description.]

## Scope Boundaries

### In Scope (v1)
- ...

### Out of Scope (Future)
- ...

## Design Decisions

[Record each non-obvious decision and its rationale. Add to this section as the
project evolves. Examples:]
- "Marginal emissions, not average — solar displaces the marginal generator"
- "No incentives — shows raw economics without policy distortion"
- "Static data — committed JSON for consistency; no runtime API calls"

## Environment Variables

[Include if the project uses any env vars. Remove this section if not applicable.]

| Variable | Required | Description |
|---|---|---|
| ... | ... | ... |

## Open Questions

[Things still to be decided. Remove items as they're resolved — move the
resolution to Design Decisions.]

## Stretch Goals

[Future ideas beyond v1.]
```

---

## README.md

```markdown
# [Project Name]

[One-sentence description]

[![Deploy Status](RAILWAY_BADGE_URL)](RAILWAY_DASHBOARD_URL)

## Getting started

### Prerequisites
[List prerequisites based on stack]

### Local development
[Stack-appropriate instructions, e.g.:]
```bash
# Clone the repo
git clone https://github.com/zantherobot/[project-name].git
cd [project-name]

# Install dependencies
[stack-appropriate command]

# Run locally
[stack-appropriate command]
```

## Deployment
Deployed on [Railway](https://railway.app) — pushes to `staging` trigger
auto-deploy to staging; PRs to `main` deploy to production.

## License
All Rights Reserved. See [LICENSE](LICENSE) for details.
```

Replace `RAILWAY_BADGE_URL` and `RAILWAY_DASHBOARD_URL` with the actual Railway
deploy status badge for the project once the Railway environment is created. If
Railway is not set up yet, leave the badge line as a TODO comment.

---

## LICENSE

```
Copyright (c) [CURRENT_YEAR] Henry White

All rights reserved.

No part of this software, including source code, documentation, or associated
files, may be reproduced, distributed, or transmitted in any form or by any
means without the prior written permission of the copyright holder.

For licensing inquiries, contact the copyright holder.
```

---

## .gitignore templates

### Python
```
__pycache__/
*.py[cod]
*$py.class
*.so
.Python
env/
venv/
.venv/
*.egg-info/
dist/
build/
.eggs/
*.egg
.pytest_cache/
.ruff_cache/
.mypy_cache/
.env
.env.local
*.db
*.sqlite3
```

### Node.js
```
node_modules/
dist/
build/
.env
.env.local
*.log
npm-debug.log*
.DS_Store
coverage/
```

### General (append to any stack)
```
.DS_Store
.vscode/
.idea/
*.swp
*.swo
*~
```

---

## Railway deployment files

### Python + Gunicorn (Procfile)
```
web: gunicorn app:app --bind 0.0.0.0:$PORT
```

### Python + Uvicorn / FastAPI (Procfile)
```
web: uvicorn app:app --host 0.0.0.0 --port $PORT
```

### Python + Streamlit (Procfile)
```
web: streamlit run app.py --server.port $PORT --server.address 0.0.0.0
```

### Node.js (Procfile)
```
web: node index.js
```

### railway.toml (optional — use if custom build needed)
```toml
[build]
builder = "nixpacks"

[deploy]
startCommand = "[same as Procfile web command]"
restartPolicyType = "ON_FAILURE"
restartPolicyMaxRetries = 10
```

### Static site (nixpacks.toml)
```toml
[phases.setup]
nixPkgs = ["python311"]

[start]
cmd = "python -m http.server $PORT"
```

---

## Stack-specific dependency files

### Python — requirements.txt
Include test and formatting tools by default:
```
pytest
ruff
```

### Node.js — package.json
Include test and formatting tools by default:
```json
{
  "name": "[project-name]",
  "version": "0.1.0",
  "private": true,
  "scripts": {
    "start": "[start command]",
    "dev": "[dev command]",
    "test": "vitest run",
    "test:watch": "vitest",
    "lint": "tsc --noEmit",
    "format": "prettier --write ."
  },
  "dependencies": {},
  "devDependencies": {
    "vitest": "^3.0.0",
    "prettier": "^3.0.0"
  }
}
```

### .env.example
Create if the project uses any API keys or secrets:
```
# [SERVICE_NAME] credentials
SERVICE_API_KEY=
SERVICE_SECRET=
```
