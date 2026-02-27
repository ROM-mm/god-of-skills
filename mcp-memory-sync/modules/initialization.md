# Module: initialization

## Phase 1A — Initialization Mode (first run)

Triggered when none of the three fixed entities exist in the MCP graph.
Create all three from scratch by fully analyzing the project.

---

> **All entity names must use the `<project-name>` prefix detected in Phase -1B.**
> Example: if `<project-name>` is `mcps`, use `mcps/project-overview`, `mcps/project-technical`, `mcps/project-conventions`.

---

## ❌ Files and directories to NEVER read or analyze

The following must be completely ignored during all data collection phases:

**Ignore files:**
- VCS: `.gitignore`, `.gitattributes`, `.gitmodules`
- IDE/editor: `.codeiumignore`, `.windsurfignore`, `.cursorignore`, `.aiderignore`, `.editorconfig`
- Secrets/env: `.env`, `.env.*`, `*.env`
- Lock files: `package-lock.json`, `yarn.lock`, `pnpm-lock.yaml`, `poetry.lock`, `Pipfile.lock`, `Gemfile.lock`, `Cargo.lock`, `go.sum`, `packages.lock.json`, `composer.lock`
- Compiled/generated: `*.class`, `*.jar`, `*.war`, `*.ear`, `*.pyc`, `*.pyo`, `*.o`, `*.obj`, `*.exe`, `*.dll`, `*.so`, `*.dylib`
- Logs/temp: `*.log`, `*.tmp`, `*.cache`, `*.pid`
- Binary/media: `*.png`, `*.jpg`, `*.jpeg`, `*.gif`, `*.ico`, `*.svg`, `*.woff`, `*.woff2`, `*.ttf`, `*.eot`, `*.pdf`, `*.zip`, `*.tar`, `*.gz`, `*.rar`

**Ignore directories:**
- VCS: `.git/`
- IDE: `.idea/`, `.vscode/`, `.eclipse/`, `.settings/`
- OS: `.DS_Store`, `Thumbs.db`
- **Python:** `__pycache__/`, `.pytest_cache/`, `.mypy_cache/`, `.ruff_cache/`, `.venv/`, `venv/`, `env/`, `.tox/`
- **Node.js/JS/TS:** `node_modules/`, `.next/`, `.nuxt/`, `.svelte-kit/`, `.turbo/`
- **Java/Kotlin/Scala:** `target/`, `.gradle/`, `build/` (when Java project)
- **Go:** `vendor/` (when Go project)
- **Ruby:** `.bundle/`, `vendor/bundle/`
- **Rust:** `target/` (when Rust project)
- **C#/.NET:** `bin/`, `obj/`, `.vs/`
- **PHP:** `vendor/` (when PHP project)
- **General build/output:** `dist/`, `out/`, `release/`, `.output/`
- **Coverage/reports:** `coverage/`, `.coverage`, `htmlcov/`, `lcov-report/`

> Only read source code files, configuration files (e.g., `pyproject.toml`, `pom.xml`, `build.gradle`, `package.json`, `go.mod`, `Cargo.toml`, `Gemfile`, `composer.json`, `Dockerfile`, CI/CD YAMLs), and documentation files (e.g., `README.md`).

---

### Data collection for `<project-name>/project-overview`

Analyze the project and extract:
- Name and purpose — what it does, what problem it solves
- Business premise — context and motivation
- Target audience or main use cases
- Project objectives and scope
- Any high-level documentation found (README, docs, wikis)

```
create_entities:
  name: "<project-name>/project-overview"
  entityType: "ProjectMemory"
  observations: [write all in pt-BR]
    - "Propósito: [descrição do que o projeto faz]"
    - "Premissa: [problema que resolve / motivação]"
    - "Escopo: [o que está dentro e fora do projeto]"
    - "Casos de uso: [principais usos identificados]"
    - "Público-alvo: [quem usa o projeto]"
```

---

### Data collection for `<project-name>/project-technical`

Analyze the project and extract:
- Programming languages and their versions
- Main frameworks and libraries (with versions when available)
- Build, test, linting, and formatting tools
- Presence of Dockerfile, docker-compose, CI/CD files
- Dependency files found (requirements.txt, package.json, pom.xml, pyproject.toml, etc.)
- Directory structure and project organization
- Total number of files and file types present
- Databases, queues, and external services referenced
- Architectural patterns identified (MVC, Clean Architecture, Event-Driven, etc.)
- Environment variables and configuration files

```
create_entities:
  name: "<project-name>/project-technical"
  entityType: "ProjectMemory"
  observations: [write all in pt-BR]
    - "Linguagens: [linguagens e versões]"
    - "Frameworks: [frameworks principais]"
    - "Dependências: [arquivo de deps + bibliotecas chave]"
    - "Infraestrutura: [Dockerfile? docker-compose? CI/CD?]"
    - "Estrutura: [organização de diretórios]"
    - "Arquivos: [quantidade total e tipos presentes]"
    - "Arquitetura: [padrão arquitetural identificado]"
    - "Serviços externos: [DBs, filas, APIs referenciadas]"
    - "Configuração: [variáveis de ambiente e configs]"
```

---

### Data collection for `<project-name>/project-conventions`

Analyze source code, config files, linting configs, and documentation to extract:
- Naming conventions — variables, functions, classes, files, directories
- Mandatory code structure patterns
- Import and dependency ordering rules
- Error handling patterns enforced across the codebase
- Testing conventions — file naming, folder structure, test patterns
- Commit message conventions
- Mandatory annotations, decorators, or comments
- Forbidden patterns (e.g., no bare except, no print, no wildcard imports)
- Style guides referenced (.editorconfig, ESLint, Ruff, Checkstyle, etc.)

```
create_entities:
  name: "<project-name>/project-conventions"
  entityType: "ProjectMemory"
  observations: [write all in pt-BR]
    - "Nomenclatura: [padrões de nome usados no projeto]"
    - "Estrutura de código: [como módulos/classes/funções devem ser organizados]"
    - "Tratamento de erros: [padrão de error handling adotado]"
    - "Testes: [convenções de teste, nomenclatura, estrutura]"
    - "Commits: [padrão de mensagem de commit]"
    - "Proibições: [padrões explicitamente evitados]"
    - "Ferramentas de estilo: [linters, formatters e suas configs]"
```

---

### Create relations between entities

```
create_relations:
  - from: "<project-name>/project-technical"    relationType: "belongs_to"  to: "<project-name>/project-overview"
  - from: "<project-name>/project-conventions"  relationType: "belongs_to"  to: "<project-name>/project-overview"
  - from: "<project-name>/project-conventions"  relationType: "applies_to"  to: "<project-name>/project-technical"
```

---

After all entities and relations are created → load [windsurf-refresh.md](windsurf-refresh.md)
