---
name: new-project
description: >
  Scaffold a new GitHub repo with standard boilerplate for web-based Claude Code
  projects. Use when the user says "new project", "create a repo", "start a new
  app", "scaffold a project", or wants to set up a fresh GitHub repository.
  Do NOT use for existing repos or non-project-creation tasks.
disable-model-invocation: true
allowed-tools: Bash(gh *) Bash(git *) Bash(mkdir *) Bash(cp *)
argument-hint: "[project-name]"
---

# New Project Scaffolder

Create a new GitHub repo with Henry's standard boilerplate, ready for Claude Code
development and Railway auto-deployment.

## Workflow

### Step 1: Gather project details and co-develop the spec

If `$ARGUMENTS` provides a project name, use it. Otherwise ask.

Then walk through these topics **one at a time** — don't dump all questions at once.
Ask, discuss, move on.

**1. Goal** — "What does this project do, in one sentence? Who is it for?"

**2. Tech stack** — Ask what language/framework fits. Suggest options based on the
   project description. Common stacks:
   - Python + Flask/Gunicorn (API / data apps)
   - Python + Streamlit (dashboards / data viz)
   - Node.js + Express (web APIs)
   - React / Vite (frontend SPAs)
   - Static HTML/JS (simple tools)
   Do NOT assume a stack. The user decides.

**3. Data sources** — "Where does the data come from? APIs, static files, user
   input only?" For each source, get: what it provides, URL if known, any API
   keys needed.

**4. Core features** — "What are the must-have features for v1?" Get a short list.

**5. Key design decisions** — Prompt for any non-obvious choices:
   - "Any important tradeoffs or simplifications to document?"
   - "Anything deliberately out of scope for v1?"
   - "Does this project need user accounts or authentication?"

**6. Visibility** — "Public or private repo?" Default is public. Note: the
   LICENSE defaults to All Rights Reserved, so confirm this pairing makes sense
   for the project.

**7. Railway deployment?** (yes/no — default yes)

Use the answers to populate both `spec.md` and the project-specific sections of
`CLAUDE.md`. The spec should be a useful starting document, not just a placeholder —
but it doesn't need to be exhaustive at this stage. It will evolve.

### Step 2: Create the repo and set up branches

```bash
gh repo create $PROJECT_NAME --public --clone
cd $PROJECT_NAME
```

If the user chose private, use `--private` instead.

### Step 3: Generate boilerplate files

Create these files in the repo. Read `references/templates.md` for the exact
contents of each file.

**CLAUDE.md has two layers:**
- **Universal sections** (Spec Stays Current, Validation, No Secrets in Code,
  Small Focused Commits, Graceful Error Handling, Logging, Accessibility, and the
  entire Deployment & Branch Workflow) — copy verbatim from the template. Do not
  rephrase.
- **Project-specific sections** (Run/Test commands, Testing specifics, Linting &
  Code Style, and optional Data Pipeline) — fill in based on the stack discussion
  from Step 1.

**Test framework defaults** (do not ask — just wire these in):
- Python → pytest (add to `requirements.txt`, test command: `python -m pytest tests/ -v`)
- TypeScript/Node → Vitest (add to `package.json` devDependencies, test command: `npm test`)

**Auto-formatter defaults** (do not ask — just wire these in):
- Python → `ruff format` (add `ruff` to `requirements.txt`)
- TypeScript/Node → Prettier (add to `package.json` devDependencies)

**Always included:**
- `CLAUDE.md` — dev standards and workflow (see above)
- `spec.md` — project specification (populated from Step 1 discussion)
- `README.md` — project overview
- `LICENSE` — All Rights Reserved (default)
- `.gitignore` — tailored to the chosen stack

**If Railway deployment is enabled:**
- Stack-appropriate deploy config (Procfile, railway.toml, nixpacks.toml, etc.)
- `requirements.txt` / `package.json` / equivalent dependency file
- `.env.example` if the project uses API keys or secrets

### Step 4: Initial commit, create staging branch, push

```bash
git add -A
git commit -m "Initial scaffold via /new-project"
git push -u origin main

# Create and switch to staging branch — all work happens here
git checkout -b staging
git push -u origin staging
```

The working branch is now `staging`. All subsequent work targets `staging`.

### Step 5: Confirm

Tell the user:
- Repo URL
- What files were created
- Current branch: `staging`
- Suggested next step: "Open this repo in Claude Code on the web and start building. The spec is your starting point — it'll evolve as you go."
