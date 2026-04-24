# agent-skills

Public skills repository. Installable via `npx skills`.

## What this repo is

A collection of reusable skills for AI coding tools (OpenCode, Claude Code, Cursor, Copilot).
Skills are conventions, workflows, and best practices — not project-specific logic.

This repo is intentionally generic and public. No private paths, credentials, or personal config.

## Structure

```
skills/
  git-workflow/        ← conventional commits, branch naming, PR descriptions
  knowledge-session/   ← knowledge-base vault protocol (MCP-only)
  project-workflow/    ← GitHub Issues workflow, ROADMAP updates
  (add more here)
README.md
AGENTS.md              ← this file (context for AI working on this repo)
```

## Rules when editing this repo

- Skill directories: `kebab-case`
- Each skill must have a `SKILL.md` at its root (uppercase, exact filename)
- `SKILL.md` frontmatter requires `name:` and `description:`
- `name:` must match the directory name exactly
- Keep `SKILL.md` under ~5000 words — put detail in `references/` subdirectory
- Skills must be generic — no hardcoded usernames, paths, or project names
- Supporting files go in `scripts/`, `references/`, or `assets/` subdirectories

## Adding a new skill

```bash
npx skills init my-skill-name
# edit skills/my-skill-name/SKILL.md
```

Or manually:
```bash
mkdir skills/my-skill-name
cat > skills/my-skill-name/SKILL.md << 'EOF'
---
name: my-skill-name
description: What this skill does. When to load it.
---

# My Skill Name
(content)
EOF
```

## Commit conventions

Load the `git-workflow` skill when committing.

```
feat(skills): add new-skill-name skill
fix(skills): fix git-workflow PR template
docs: update README install instructions
```
