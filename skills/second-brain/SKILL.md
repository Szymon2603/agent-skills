---
name: second-brain
description: Protocol for managing a PARA-based second brain vault. Load when working with a knowledge base, personal wiki, or Obsidian/Anytype vault. Handles capture, distillation, and organization using Projects/Areas/Resources/Archives structure with append-and-review workflow.
---

# Second Brain Protocol

A vault-agnostic protocol for managing a personal knowledge base structured with **PARA** (Projects, Areas, Resources, Archives) and **append-and-review** workflow inspired by Karpathy's note-taking method.

## Vault structure

```
vault/
  0-inbox/        ← raw capture, no structure required
  1-projects/     ← active endeavours with a clear goal and deadline
  2-areas/        ← ongoing responsibilities without end date
  3-resources/    ← topics of interest, reference material
  4-archive/      ← inactive items from any PARA category
```

Folders are numbered to enforce **actionability hierarchy**: most actionable (inbox, projects) first, least (archive) last.

### When to move things between folders

| Move | When |
|------|------|
| `0-inbox → 1-projects` | A captured idea becomes a commitment with a goal |
| `0-inbox → 2-areas` | A recurring responsibility is identified |
| `0-inbox → 3-resources` | Interesting reference material, no immediate action |
| `1-projects → 2-areas` | Project ends, responsibility continues (e.g. "Launch blog" → "Blogging") |
| `1-projects → 4-archive` | Project completed or cancelled |
| `2-areas → 4-archive` | Area no longer relevant |
| `3-resources → 4-archive` | Interest faded |
| `4-archive → 1-projects` | Archived project revived |
| `4-archive → 3-resources` | Old reference becomes relevant again |

## Before the session

```
git pull
```

## Session start

Scan vault state without loading full content:

1. List directories under `0-inbox/`, `1-projects/`, `2-areas/`, `3-resources/`, `4-archive/`
2. Report note count per top-level folder
3. Show last 3 files in `0-inbox/` (names only)
4. Ask: "What do you want to capture, distill, or work on today?"

Do not load file contents on start. Read content only when needed for a specific task.

## Append-and-review (capture)

**Append** — When capturing, add notes to `0-inbox/` with minimal friction:

- Raw text, quick thoughts, links, fragments — all welcome
- Use functional tags in the note body for quick filtering: `read:`, `watch:`, `listen:`, `todo:`, `idea:`
- No frontmatter or structured metadata required at capture time
- Filename format: `{slug}.md` (human-readable, no timestamps)

**Review** — During each session, scan `0-inbox/` bottom-to-top:

- **Rescue** — copy still-relevant items toward the top if they're sinking
- **Merge** — combine related fragments into a single note
- **Distill** — move processed notes to their PARA home (`1-projects/`, `2-areas/`, `3-resources/`)
- **Delete** — rarely. Notes that don't survive review naturally sink; they're never truly lost

This mirrors Karpathy's gravity model: items that don't survive repeated scrutiny sink, things that matter float back up.

## Distillation (inbox → PARA)

A single-pass distillation process. For each inbox note being processed:

1. **Read** the note
2. **Classify** — which PARA category does it belong to?
   - **Project**: has a clear goal and end date → `1-projects/`
   - **Area**: ongoing responsibility → `2-areas/`
   - **Resource**: topic of interest, reference → `3-resources/`
   - **Unclear?** — it stays in `0-inbox/` until it becomes clearer
3. **Distill** — rewrite into a clean note following the appropriate template:
   - Add YAML frontmatter: `title`, `date`, `tags`, `para`, `summary`
   - If it's a project, add `goal:` and optional `deadline:` to frontmatter
   - Write narrative prose with `[[wikilinks]]` in context — see Linking conventions
   - Strip raw tags (`read:`, `todo:`) and convert to proper frontmatter `tags`
   - If the topic already has a MOC in `3-resources/`, link back to it from the new note
   - If processing a topic with 3+ atomic notes, create or update the MOC for it
4. **Move** the note to the appropriate PARA folder
5. **Remove** the original from `0-inbox/`

### Frontmatter fields

Every distilled note must include these fields:

```yaml
---
title: Note Title
date: 2025-05-09
para: project  # project | area | resource | archive
tags: [tag1, tag2]
summary: One-line description for AI preview and MOC curation.
goal: Clear outcome statement  # projects only
deadline: 2025-06-30           # projects only, optional
---
```

Full note templates for each PARA category are in the Note architecture section below.

## Note architecture

The vault is a wiki — every note must be readable by both humans and AI agents. This requires deliberate structure.

### Atomic notes

One idea per note. A note should express a single concept, decision, or reference. If a note grows beyond what fits on one screen, split it.

Why: atomic notes compose. They can be linked from multiple contexts, remixed into MOCs, and retrieved individually by search. Monolithic notes cannot.

### Two note types

| Type | Purpose | Where |
|------|---------|-------|
| **Atomic note** | Single concept, decision, or reference | Any PARA folder |
| **MOC (Map of Content)** | Curated overview linking to related atomic notes | `3-resources/` |

MOCs are the wiki's backbone. They provide a navigable table of contents for a topic without duplicating content. An MOC doesn't explain — it **maps**.

Example MOC structure:

```markdown
---
title: Distributed Systems
date: 2025-05-09
para: resource
tags: [distributed-systems, moc]
---

# Distributed Systems

## Fundamentals
- [[cap-theorem]] — you can only pick two
- [[consistency-models]] — strong vs eventual vs causal
- [[distributed-consensus]] — Raft, Paxos, Byzantine

## Patterns
- [[cache-warming]] — pre-fill before traffic hits
- [[circuit-breaker]] — fail fast instead of cascading
- [[rate-limiting-strategies]] — throttle at the edge

## Books & Papers
- [[ddia-book-notes]] — Designing Data-Intensive Applications
```

### Dual readability: AI + Human

Notes are read by two audiences. Design for both:

**For AI agents:**
- YAML frontmatter with structured fields — machine-parseable at zero cost
- A `summary` field in frontmatter — lets the agent decide whether to read the body
- Consistent section headers — predictable navigation without guessing
- Wikilinks with context — `[[x]]` surrounded by explanatory text, not bare links

**For humans:**
- Narrative prose in body — not just bullet lists
- Context first — *why* this note exists, not just *what* it says
- Examples and analogies — ground abstract concepts
- Progressive detail — summary at top, details below, external links at bottom

### Note template — atomic note

```yaml
---
title: Rate Limiting Strategies
date: 2025-05-09
para: resource
tags: [distributed-systems, rate-limiting, api]
summary: Overview of rate limiting approaches — fixed window, sliding window, token bucket, leaky bucket — with trade-offs and when to use each.
---
```

```markdown
# Rate Limiting Strategies

Why it matters: unprotected APIs degrade under load and enable abuse.

## Core approaches
(...narrative content with [[wikilinks]]...)

## Trade-offs
(...when to use which...)

## See also
- [[circuit-breaker]]
- [[cache-warming]]
```

### Note template — project note

```yaml
---
title: Launch Blog
date: 2025-05-09
para: project
tags: [blog, launch]
goal: Publish first post and make the blog publicly accessible
deadline: 2025-06-30
summary: Project to set up a personal blog and publish the first article.
---
```

```markdown
# Launch Blog

Goal: Publish first post and make the blog publicly accessible.

## Status
(...current state, blockers...)

## Tasks
- [ ] Choose platform → [[blog-platform-comparison]]
- [ ] Write first post → [[first-post-draft]]
- [ ] Configure domain

## Decisions
- Decided on Hugo → see [[blog-platform-comparison]]

## References
- [[blogging]] (area)
- [[writing-workflow]] (resource)
```

### Note template — area note

```yaml
---
title: Health
date: 2025-05-09
para: area
tags: [health, fitness, nutrition]
summary: Ongoing area covering exercise, nutrition, sleep, and preventive care.
---
```

```markdown
# Health

Ongoing responsibility — no end date. Review quarterly.

## Active projects
- [[marathon-training-plan]] → deadline 2025-09-15
- [[sleep-optimization-experiment]]

## Key metrics
(...what I track for this area...)

## References
- [[nutrition-basics]]
- [[sleep-science]]
```

### Linking conventions

Wikilinks are the graph's connective tissue. Use them with intent:

1. **Link with context** — surround `[[slug]]` with enough prose that both the reader and the AI understand the relationship without following the link:

   ```markdown
   The [[cap-theorem]] forces a choice between consistency and availability
   during network partitions.
   ```

   Not just: "See [[cap-theorem]]."

2. **Typed links in text** — when the relationship matters, make it explicit in prose:

   ```markdown
   [[rate-limiting-strategies]] mitigates [[thundering-herd]]
   ```

   Not: "[[rate-limiting-strategies]] [[thundering-herd]]"

3. **Cross-PARA linking** — project and area notes should link to resource notes. This is how resources become actionable and how knowledge flows from reference to practice:

   ```
   1-projects/launch-blog → [[blog-platform-comparison]] in 3-resources/
   2-areas/health → [[nutrition-basics]] in 3-resources/
   ```

4. **MOCs link to everything** — a MOC's job is to curate and organize, so it links broadly within its topic. Atomic notes link narrowly to directly related concepts.

## File operations

This skill is vault-agnostic. Use whatever tools are available:

| Operation | Tool |
|-----------|------|
| Read | filesystem read or MCP vault tool |
| Search | grep/rg or MCP search tool |
| Create | filesystem write or MCP vault tool |
| Edit | filesystem edit or MCP vault tool |
| Move | filesystem move/rename |

If an MCP vault tool is available, prefer it. Otherwise use standard filesystem tools.

**Never** use shell commands (`echo >>`, `sed`, `cat >`) to modify vault files — always use structured write/edit operations.

## End of session

1. List new/modified files from this session
2. Distill remaining `0-inbox/` items if any are ready
3. Commit and push:

```bash
git add -A && git commit -m "knowledge: [short description]" && git push
```

4. Confirm: "Pushed. Vault synced."