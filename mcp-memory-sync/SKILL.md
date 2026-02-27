---
name: mcp-memory-sync
description: Synchronizes MCP Memory with the current project state by performing autonomous insert, update, and delete operations on three fixed memory entries — project overview, technical context, and code conventions. Use when the user wants to sync project memory, initialize memory for a new project, update memory after code or architecture changes, or keep agent memory aligned with the real state of the project.
---

# MCP Memory Sync

Manages three fixed project memory entries using a **dual-write strategy**: writes to both the MCP Memory knowledge graph and the Windsurf native Memories (Settings → Memories UI).

> **All memory content (observations) must be written in Brazilian Portuguese (pt-BR).**

---

## Modules

This skill is organized as modules. Read each file when its phase is reached.

| Module | File | When to load |
|---|---|---|
| `tools-reference` | [modules/tools-reference.md](modules/tools-reference.md) | Load once at the start — lists all available tools |
| `prerequisites` | [modules/prerequisites.md](modules/prerequisites.md) | Phase -1 and Phase 0 — MCP check + state detection |
| `initialization` | [modules/initialization.md](modules/initialization.md) | Phase 1A — no memories exist, create from scratch |
| `synchronization` | [modules/synchronization.md](modules/synchronization.md) | Phase 1B — memories exist, sync changes |
| `windsurf-refresh` | [modules/windsurf-refresh.md](modules/windsurf-refresh.md) | Phase 2 — always runs at the end of every execution |
| `quality-rules` | [modules/quality-rules.md](modules/quality-rules.md) | Always — governs all operations and final report |

---

## Execution Flow

```
START
  │
  ├─ load tools-reference.md
  ├─ load quality-rules.md
  │
  ├─ load prerequisites.md
  │     ├─ Phase -1: check MCP Memory availability
  │     │     └─ if unavailable → STOP
  │     └─ Phase 0: detect state (read_graph)
  │           ├─ no entities found → load initialization.md (Phase 1A)
  │           └─ entities found   → load synchronization.md (Phase 1B)
  │
  └─ load windsurf-refresh.md (Phase 2 — always)
        └─ delete all native Memories → read graph → recreate
              └─ present Final Report (quality-rules.md)
END
```
