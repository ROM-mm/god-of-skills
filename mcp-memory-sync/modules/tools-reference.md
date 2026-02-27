# Module: tools-reference

Reference table for all tools used by this skill.

## MCP Memory Server tools (knowledge graph)

| Tool | Purpose |
|---|---|
| `read_graph` | Read the entire knowledge graph |
| `open_nodes` | Open specific nodes by name |
| `search_nodes` | Search nodes by query |
| `create_entities` | Create new entities with observations |
| `create_relations` | Create relations between entities |
| `add_observations` | Add observations to existing entities |
| `delete_entities` | Delete entities and their relations |
| `delete_observations` | Delete specific observations from entities |
| `delete_relations` | Delete relations between entities |

## Windsurf native Memories tools (UI-visible)

| Tool | Purpose |
|---|---|
| `create_memory (Action: create)` | Create a new memory entry visible in Windsurf UI |
| `create_memory (Action: update)` | Update an existing Windsurf memory by ID |
| `create_memory (Action: delete)` | Delete a Windsurf memory by ID |

## Fixed entity names

The three entities managed by this skill use the project name as a prefix to ensure isolation across multiple projects in the same graph.

Pattern: `<project-name>/project-overview`, `<project-name>/project-technical`, `<project-name>/project-conventions`

Where `<project-name>` is the **last segment of the workspace URI**, lowercased and with spaces replaced by hyphens.

Examples for a project at `/Users/romerito/projects/my-api`:
- `my-api/project-overview`
- `my-api/project-technical`
- `my-api/project-conventions`

| Entity (template) | Type |
|---|---|
| `<project-name>/project-overview` | ProjectMemory |
| `<project-name>/project-technical` | ProjectMemory |
| `<project-name>/project-conventions` | ProjectMemory |

## Fixed relations

Relations also use the prefixed names:

| From | Relation | To |
|---|---|---|
| `<project-name>/project-technical` | `belongs_to` | `<project-name>/project-overview` |
| `<project-name>/project-conventions` | `belongs_to` | `<project-name>/project-overview` |
| `<project-name>/project-conventions` | `applies_to` | `<project-name>/project-technical` |
