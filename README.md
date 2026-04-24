# agent-skills

Reusable skills for AI coding tools.

## Install all skills

```bash
npx skills add <your-username>/agent-skills
```

## Install specific tools only

```bash
npx skills add <your-username>/agent-skills -a opencode -a claude-code
```

## Preview without installing

```bash
npx skills add <your-username>/agent-skills --list
```

## Update installed skills

```bash
npx skills update
```

## Available skills

| Skill | Description |
|-------|-------------|
| `git-workflow` | Conventional commits, branch naming, PR descriptions, changelog |
| `knowledge-session` | Knowledge-base vault protocol — MCP-only file operations, pipeline |
| `project-workflow` | GitHub Issues workflow — branch creation, commit refs, ROADMAP |

## Add to your project agent

Reference skills by name in your project agent's system prompt:

```markdown
When committing changes, load the `git-workflow` skill.
When working in knowledge-base, load the `knowledge-session` skill.
When starting a coding session on an issue, load the `project-workflow` skill.
```
