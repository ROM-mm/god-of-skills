# Module: quality-rules

## Quality Rules
- **Always prefix entity names with `<project-name>/`** — never create bare `project-overview`, `project-technical`, or `project-conventions` entities without the project prefix
- **Never touch entities from other projects** — only operate on entities whose name starts with the current `<project-name>/` prefix
- **Never create entities beyond the three fixed ones per project** — all project information lives in `<project-name>/project-overview`, `<project-name>/project-technical`, and `<project-name>/project-conventions`
- **Observations must be factual and specific** — avoid generalizations
- **Verify the real project** — do not assume; read files and structure before deciding
- **Be conservative with deletion** — when in doubt, update instead of delete
- **One observation per fact** — do not group unrelated information into a single observation
- **All observation content must be written in pt-BR**

---

## Relevance Criteria

### project-overview — capture only:
- Project purpose and business goal
- Target audience
- Current scope (what is in, what is explicitly out)
- Active use cases

### project-technical — capture only:
- Languages, frameworks, and versions in use
- External dependencies and integrations
- Directory structure (top-level only)
- Infrastructure and deploy setup
- File naming conventions

### project-conventions — capture only:
- Code style rules actively enforced
- Error handling patterns
- Test conventions
- Explicit prohibitions (what must never be done)

### Never capture in any entity:
- Aspirational or planned features not yet implemented
- Temporary workarounds unless they affect architecture
- Personal preferences without behavioral impact
- Duplicate information already in another entity

---

## Windsurf Refresh Pre-condition

Before running Phase 2 (windsurf-refresh), check the Final Report:
- If total operations (inserted + updated + deleted) across all entities = 0 → **skip refresh entirely**
- If total operations > 0 → proceed with delete → read → recreate

Reason: Windsurf Memories are a mirror of MCP. If MCP didn't change, the mirror doesn't need to be redrawn.

---

## Final Report

Present at the end of every execution:

```
## MCP Memory Sync — Report

### Mode
[Initialization / Synchronization]

### overview
- Inseridas: X observações
- Atualizadas: Y observações
- Deletadas: Z observações

### technical
- Inseridas: X observações
- Atualizadas: Y observações
- Deletadas: Z observações

### conventions
- Inseridas: X observações
- Atualizadas: Y observações
- Deletadas: Z observações

### Windsurf Memories Refresh
- Status: Skipped (no changes detected)
  OR
- Deletadas: 3 entradas antigas
- Recreadas: 3 novas entradas

### Summary of changes
[List of main changes made]
```