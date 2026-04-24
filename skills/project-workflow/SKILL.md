---
name: project-workflow
description: GitHub Issues workflow — branch creation, commit references, ROADMAP updates. Load at the start of any coding session working on a specific issue.
---

# Project Workflow

## Before coding

1. Read the Issue (ask for the number if not provided)
2. Check ROADMAP.md — does this feature have dependencies?
3. Create branch: `feature/issue-{N}-{short-kebab-name}`

## During work

- Every commit references the issue: `feat: add rate limiting (#12)`
- One commit = one logical change
- Do not mix refactoring with new functionality

## After finishing

1. Verify all Acceptance Criteria from the Issue
2. Run tests for changed modules
3. Update ROADMAP.md — set feature status to "done"
4. Call `@reviewer` to check the implementation before committing

## Commit format

Load the `git-workflow` skill for full commit conventions.
Short version: `<type>(<scope>): <imperative description> (#N)`
