# Module: synchronization

## Phase 1B — Synchronization Mode (memories already exist)

Triggered when the three prefixed entities for the current project already exist in the MCP graph.
Fully analyze the project, compare with recorded observations, and operate only on what changed.

> **All operations use `<project-name>` as prefix (detected in Phase -1B).**
> Entities from other projects (different prefix) must be completely ignored.

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

## Routing Rule — Which entity to operate on?

**Operate on `<project-name>/project-overview` when:**
- Change in project purpose or premise
- Change in scope (addition or removal of business features)
- New use cases identified
- Change in high-level documentation (README with new description, new product docs)
- Change in motivation or target audience

**Operate on `<project-name>/project-technical` when:**
- Addition, removal, or update of dependencies/libraries
- Addition or removal of files (Dockerfile, CI/CD, configs, scripts)
- Framework or language change
- Directory structure change
- New architectural pattern adopted
- New external service referenced (database, queue, API)
- Change in build, test, or linting tools
- Change in the quantity or types of files in the project

**Operate on `<project-name>/project-conventions` when:**
- New naming pattern identified or an existing one changed
- New mandatory code structure rule adopted
- Linting or formatting rule added, removed, or modified
- New forbidden pattern identified in the codebase
- Testing convention changed
- Commit message convention adopted or changed
- New annotation, decorator, or mandatory comment pattern enforced

---

## Execution Order

Always: **delete → update → insert**

### Deleting obsolete observations

Use `delete_observations` to remove observations that are no longer true.

```
When to delete:
- File, module, or technology was removed from the project
- Convention or pattern was abandoned
- Information is duplicated or contradicts a more recent observation
```

### Updating observations

No direct update exists — use `delete_observations` to remove the stale value, then `add_observations` to insert the corrected one.

```
When to update:
- Technology version changed
- Description no longer matches the current code state
- New information complements an existing observation
- A convention evolved or was refined
```

### Inserting new observations

Use `add_observations` to add new facts not yet recorded.

```
When to insert:
- New technology, tool, or file was added to the project
- New pattern or architectural decision was identified
- New use case or business scope was discovered
- New code convention or forbidden pattern was introduced
```

---

After all MCP Memory operations are complete → load [windsurf-refresh.md](windsurf-refresh.md)
