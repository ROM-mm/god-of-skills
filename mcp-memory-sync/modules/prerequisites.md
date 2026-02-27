# Module: prerequisites

## Phase -1 — Prerequisites Check (REQUIRED)

Before any operation, verify that the MCP Memory server is available.

Attempt to call `read_graph`. Evaluate the result:

- **Success** → MCP Memory is available. Proceed to Phase -1B.
- **Error / tool not found / server unavailable** → **Stop immediately** and display the error below.

```
❌ MCP Memory is not available.

This skill requires the MCP Memory Server to be configured and active.

To resolve:
1. Check if the MCP Memory server is installed and configured in your environment
2. Confirm that the Memory MCP tools (read_graph, create_entities, etc.) are accessible
3. Verify MEMORY_FILE_PATH points to a .json file, not a directory
4. Restart the MCP server and try again

No operations were performed.
```

**Do not proceed with any other phase if MCP Memory is unavailable.**

> Windsurf native Memories (`create_memory`) do not require a separate check — they are always available.

## Phase -1B — Project Name Detection (REQUIRED)

Determine the current project name from the active workspace URI.

**Rule:** Take the **last path segment** of the workspace root URI, lowercase it, and replace spaces with hyphens.

Examples:
- Workspace `/Users/romerito/projects/my-api` → project name: `my-api`
- Workspace `/Users/morais/work/Data Pipeline` → project name: `data-pipeline`
- Workspace `/Users/pipoca/mcps` → project name: `mcps`

Store this value as `<project-name>`. All entity names in subsequent phases use this prefix.

> If the workspace URI cannot be determined, ask the user to confirm the project name before proceeding.

---

## Phase 0 — State Detection

Read the full graph via `read_graph`.

Check whether entities with these exact prefixed names exist:
- `<project-name>/project-overview`
- `<project-name>/project-technical`
- `<project-name>/project-conventions`

**If none exist → load [initialization.md](initialization.md)**
**If they already exist → load [synchronization.md](synchronization.md)**

> Entities from **other projects** (different prefix) must be completely ignored — do not read, modify, or delete them.
