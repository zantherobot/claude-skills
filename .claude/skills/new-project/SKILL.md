---
name: new-project
description: >
  Co-develop a project spec for a new app. Use when the user says "new project",
  "start a new app", "plan a project", or wants to define what to build before
  creating a repo. Produces a reviewed spec.md — does NOT create repos or write code.
disable-model-invocation: true
allowed-tools: Write
argument-hint: "[project-name]"
---

# New Project Spec Builder

Walk through a structured conversation to co-develop `spec.md` for a new project.
Present the spec for review, then write it to the current repo. Stop there —
implementation happens in the project repo, not here.

## Workflow

### Step 1: Gather project details

If `$ARGUMENTS` provides a project name, use it. Otherwise ask.

Then walk through these topics **one at a time** — ask, discuss, move on.
Do not dump all questions at once.

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

**6. Visibility** — "Public or private repo?" (for documentation purposes only)

**7. Railway deployment?** (yes/no — default yes, for documentation purposes only)

### Step 2: Draft spec.md

Using the answers from Step 1 and the spec template in `references/templates.md`,
draft the full contents of `spec.md`. Present it to the user as a fenced markdown
block for review.

Ask: "Does this look right, or would you like to adjust anything?"

Iterate until the user confirms the spec is ready.

### Step 3: Write the file

Write `spec.md` to the current repo using the Write tool.

### Step 4: Confirm and hand off

Tell the user:
- `spec.md` has been written to this repo
- Next step: create a new GitHub repo, open it in Claude Code on the web, and
  say: "Implement this project — copy the spec from this repo's spec.md as the
  starting point"
- The spec will evolve as you build — keep it current with the implementation
