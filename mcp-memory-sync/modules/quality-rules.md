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
- Deletadas: 3 entradas antigas
- Recreadas: 3 novas entradas

### Summary of changes
[List of main changes made]
```
