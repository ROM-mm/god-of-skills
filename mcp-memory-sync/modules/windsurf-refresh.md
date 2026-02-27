# Module: windsurf-refresh

## Phase 2 — Windsurf Memories Refresh (runs after EVERY execution)

Ensures the Windsurf native Memories (Settings → Memories) always reflect the current MCP graph state **for the active project only**.
Strategy: **delete current project entries → read graph → recreate from scratch.**

> All operations are scoped to `<project-name>` (detected in Phase -1B). Never touch memories from other projects.

---

## Step 1 — Delete existing Windsurf native Memories for this project

Find and delete entries tagged with both `mcp-memory-sync` **and** `<project-name>`:

```
create_memory (Action: delete, Id: <id of [overview] <project-name> entry>)
create_memory (Action: delete, Id: <id of [technical] <project-name> entry>)
create_memory (Action: delete, Id: <id of [conventions] <project-name> entry>)
```

To find IDs: look for Windsurf memories with titles matching:
- `[overview] <project-name>`
- `[technical] <project-name>`
- `[conventions] <project-name>`

Or look for entries tagged with both `mcp-memory-sync` and `<project-name>`.

If none exist (first run for this project) → skip to Step 2.

---

## Step 2 — Read current graph state for this project

```
open_nodes: ["<project-name>/project-overview", "<project-name>/project-technical", "<project-name>/project-conventions"]
```

Collect all current observations from the three entities.

---

## Step 3 — Recreate Windsurf native Memories for this project

Write **structured Markdown** for each entity. Do not write a plain paragraph — use headers, bold bullet points, and code blocks where applicable.

### Memory: overview

```
create_memory (Action: create):
  Title: "Projeto: <project-name> — Visão Geral"
  Tags: ["overview", "<project-name>", "mcp-memory-sync"]
  Content:
    ## Propósito
    <one or two sentences describing what the project does>

    ## Premissa
    <problem it solves / motivation>

    ## Escopo
    - **Dentro:** <what is in scope>
    - **Fora:** <what is out of scope>

    ## Casos de Uso
    - **<use case 1>:** <short description>
    - **<use case 2>:** <short description>

    ## Público-alvo
    <who uses the project>
```

### Memory: technical

```
create_memory (Action: create):
  Title: "Projeto: <project-name> — Contexto Técnico"
  Tags: ["technical", "<project-name>", "mcp-memory-sync"]
  Content:
    ## Stack Tecnológico
    - **<language/framework>** — <version and role>
    - **<library>** — <purpose>

    ## Infraestrutura
    - **<tool>:** <present/absent and role>

    ## Arquitetura
    <architectural pattern identified>

    ## Estrutura de Diretórios
    ```
    <key directories and their purpose>
    ```

    ## Serviços Externos
    - **<service>:** <role>

    ## Configuração
    - **<env var or config file>:** <purpose>
```

### Memory: conventions

```
create_memory (Action: create):
  Title: "Projeto: <project-name> — Convenções"
  Tags: ["conventions", "<project-name>", "mcp-memory-sync"]
  Content:
    ## Nomenclatura
    - **<element>:** <naming rule>

    ## Estrutura de Código
    - **<rule>:** <description>

    ## Tratamento de Erros
    - **<pattern>:** <description>

    ## Testes
    - **<convention>:** <description>

    ## Commits
    - **Padrão:** <commit message format>

    ## Proibições
    - **<forbidden pattern>:** <reason>

    ## Ferramentas de Estilo
    - **<tool>:** <config file or rule>
```

**Result:** Each project has its own isolated, structured Windsurf native Memories. Running the skill on a different project never affects another project's entries.

---

After completing this phase → load [quality-rules.md](quality-rules.md) and present the Final Report.
