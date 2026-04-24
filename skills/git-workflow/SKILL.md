---
name: git-workflow
description: Git conventions — branch naming, conventional commits, PR descriptions, changelog. Load when committing or opening a PR.
---

# Git Workflow

## Branch naming

```
<type>/<issue-N>-<short-kebab-description>

feat/12-ryanair-adapter
fix/18-scheduler-timezone
chore/5-setup-docker-compose
```

Type must match the primary commit type on the branch.
Include issue number when one exists.

## Commit messages

```
<type>(<scope>): <imperative description> (#N)
```

Types: `feat` | `fix` | `refactor` | `test` | `docs` | `chore` | `perf` | `ci`

Rules:
- Imperative mood: "add" not "added" or "adds"
- Scope = module or directory
- Reference issue when it exists
- One commit = one logical change — never mix feat with refactor
- **Never squash** — history is valuable
- Rebase WIP commits before opening a PR: `git rebase -i`

## Before every commit

1. `git diff --staged` — review exactly what you're committing
2. Run tests for changed modules
3. Check staged files — no `.env`, `*.log`, `.idea/`, `node_modules/`
4. Check for hardcoded credentials or secrets

## PR description template

```markdown
## What
[1-3 sentences describing the change]

## Why
[context, problem being solved]

## How
[key technical decisions — omit if obvious]

## Checklist
- [ ] Tests pass
- [ ] No hardcoded credentials
- [ ] CHANGELOG updated (if feat/fix)
```

## CHANGELOG

Update on every `feat` and `fix`. Format (Keep a Changelog):

```markdown
## [Unreleased]
### Added
- Description from user perspective, not implementation (#12)
### Fixed
- Description (#18)
```
