---
name: knowledge-session
description: Protocol for knowledge-base sessions. Load when working in the knowledge-base vault. All file operations go through MCP (mcpvault) — never direct write/edit/bash.
---

# Knowledge Session Protocol

## Before the session

```bash
git pull
```

## Session start

Fetch vault state via MCP:

```
knowledge_get_vault_stats()
knowledge_list_directory(path="inbox")
knowledge_list_directory(path="concepts")
knowledge_list_directory(path="connections")
knowledge_list_directory(path="ideas/backlog")
```

Then:
1. Report file count per directory
2. Show last 3 files in inbox/ (names only — do not load content)
3. Ask: "What do you want to research or add today?"

Do not load file contents on start. Read full content only when needed for a specific task.

## All file operations go through MCP

| Operation | MCP tool |
|-----------|----------|
| Read | `knowledge_read_note` |
| Search | `knowledge_search_notes` |
| Create | `knowledge_write_note` |
| Edit fragment | `knowledge_patch_note` |
| Update frontmatter only | `knowledge_update_frontmatter` |
| Change tags | `knowledge_manage_tags` |
| List directory | `knowledge_list_directory` |
| Vault stats | `knowledge_get_vault_stats` |

**Never use write, edit, or bash tools on vault files.**

## Wikilinks in connections/ (required)

Use `[[concept-name]]` in the body of connections/ files — not just in frontmatter.
Obsidian builds its graph from wikilinks in content, not from the `concepts:` field.

```markdown
Ryanair applies [[ryanair-rate-limits]] which forces
aggressive [[cache-warming]] on the client side.
```

## Pipeline

```
inbox/  →  @knowledge-collector (temp 0.2)  →  concepts/
concepts/  →  @knowledge-connector (temp 0.7)  →  connections/
connections/  →  @idea-generator (temp 1.1)  →  ideas/
```

Each stage is a separate step — do not skip from inbox/ directly to ideas/.

## End of session

1. List new/modified files from this session
2. Run @knowledge-connector on new concepts/ files
3. Run @idea-generator if new connections/ files were created
4. Commit and push:

```bash
git add -A && git commit -m "knowledge: [short description]" && git push
```

5. Confirm: "Pushed. Obsidian on Windows auto-pulls in 10 minutes."
