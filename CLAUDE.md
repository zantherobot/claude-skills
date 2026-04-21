# claude-skills — Developer Guide for Claude Code

This repo holds personal Claude Code skills used across projects.

## How to use

Open this repo in Claude Code on the web. Skills in `.claude/skills/` are
available automatically. Type `/` to see available skills.

## Adding a new skill

1. Create a directory under `.claude/skills/<skill-name>/`
2. Add a `SKILL.md` with YAML frontmatter and instructions
3. Add supporting files in `references/` if needed
4. Commit and push

## Current skills

| Skill | Purpose |
|---|---|
| `/new-project` | Scaffold a new GitHub repo with standard boilerplate |
