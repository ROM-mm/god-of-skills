# SKILL mcp-memory-sync

## Visão Geral

A **MCP Memory Sync** é uma skill Windsurf especializada em sincronizar automaticamente o estado atual de um projeto com duas fontes de memória: o **MCP Memory Server** (grafo de conhecimento) e as **Memories nativas do Windsurf** (UI). Ela opera de forma autônoma para manter o contexto do projeto sempre atualizado.

### Propósito Principal

Manter três entradas de memória fixas por projeto através de uma **estratégia de dupla escrita**: escreve tanto no grafo de conhecimento MCP quanto nas memórias nativas do Windsurf, garantindo que o agente IA sempre tenha acesso ao contexto mais recente do projeto.

---

## Estrutura de Arquivos

### SKILL.md
Arquivo principal que define a skill e seu fluxo de execução.

**Funções:**
- **Metadados**: Nome (`mcp-memory-sync`) e descrição detalhada
- **Descrição de Funcionamento**: Explica a estratégia de dupla escrita
- **Tabela de Módulos**: Mapeia cada fase do processo para seu arquivo correspondente
- **Fluxo de Execução**: Diagrama visual do processo completo com fases e decisões

**Conteúdo Chave:**
- Lista de 6 módulos organizados por fase de execução
- Regra obrigatória: todo conteúdo deve ser em pt-BR
- Fluxo START → Phase -1 → Phase 0 → Phase 1A/1B → Phase 2 → END

---

## Módulos Detalhados

### 1. tools-reference.md
**Função:** Catálogo de referência de todas as ferramentas utilizadas pela skill.

**Conteúdo:**
- **Ferramentas MCP Memory**: 8 ferramentas para operações no grafo de conhecimento
  - `read_graph`, `open_nodes`, `search_nodes` (leitura)
  - `create_entities`, `create_relations` (criação)
  - `add_observations`, `delete_observations`, `delete_entities`, `delete_relations` (modificação)
- **Ferramentas Windsurf**: 3 operações do `create_memory`
  - `Action: create/update/delete` para memórias nativas
- **Padrão de Nomenclatura**: `<project-name>/entity-type`
  - Exemplo: `my-api/project-overview`, `my-api/project-technical`, `my-api/project-conventions`
- **Relações Fixas**: 3 relações predefinidas entre as entidades

---

### 2. prerequisites.md
**Função:** Verificações pré-execução e detecção do estado atual.

**Fases:**

**Phase -1 (Prerequisites Check):**
- Verifica disponibilidade do MCP Memory Server através de `read_graph`
- Se indisponível → exibe mensagem de erro e para execução
- Se disponível → continua para Phase -1B

**Phase -1B (Project Name Detection):**
- Extrai nome do projeto da URI do workspace
- Regra: último segmento do caminho, lowercase, hífens para espaços
- Exemplo: `/Users/romerito/projects/my-api` → `my-api`

**Phase 0 (State Detection):**
- Lê grafo completo via `read_graph`
- Verifica existência das 3 entidades com prefixo do projeto
- **Se nenhuma existe** → carrega `initialization.md` (Phase 1A)
- **Se já existem** → carrega `synchronization.md` (Phase 1B)

---

### 3. initialization.md
**Função:** Criação inicial das 3 entidades quando o projeto ainda não tem memória.

**Phase 1A (Initialization Mode):**
Acionado quando nenhuma das 3 entidades existe no grafo MCP.

**Coleta de Dados por Entidade:**

**`<project-name>/project-overview`:**
- Propósito do projeto
- Premissa e motivação
- Escopo (dentro/fora)
- Casos de uso principais
- Público-alvo

**`<project-name>/project-technical`:**
- Linguagens e versões
- Frameworks e bibliotecas
- Ferramentas de build/test/lint
- Infraestrutura (Docker, CI/CD)
- Estrutura de diretórios
- Serviços externos (DBs, APIs)
- Padrões arquiteturais

**`<project-name>/project-conventions`:**
- Convenções de nomenclatura
- Estrutura de código obrigatória
- Padrões de tratamento de erros
- Convenções de testes
- Padrões de commit
- Padrões proibidos
- Ferramentas de estilo

**Regras Estritas de Ignorar:**
Lista extensa de arquivos e diretórios a serem ignorados durante a análise:
- Arquivos VCS, IDE, secrets, lock files, compilados, logs, binários
- Diretórios como `.git/`, `node_modules/`, `target/`, `__pycache__/`, etc.

---

### 4. synchronization.md
**Função:** Sincronização incremental quando as entidades já existem.

**Phase 1B (Synchronization Mode):**
Acionado quando as 3 entidades já existem no grafo MCP.

**Regra de Roteamento (Qual entidade operar?):**

**`project-overview` quando:**
- Mudança no propósito ou premissa
- Alteração de escopo
- Novos casos de uso
- Mudança na documentação de alto nível

**`project-technical` quando:**
- Adição/remoção de dependências
- Mudança em arquivos de configuração
- Alteração na estrutura de diretórios
- Novo padrão arquitetural
- Mudança em ferramentas de build

**`project-conventions` quando:**
- Novo padrão de nomenclatura
- Alteração em regras de linting
- Mudança em convenções de teste
- Novo padrão proibido

**Ordem de Execução: delete → update → insert**

**Delete:** Remove observações obsoletas
**Update:** Delete + Add (não há update direto)
**Insert:** Adiciona novos fatos não registrados

---

### 5. windsurf-refresh.md
**Função:** Atualização das memórias nativas do Windsurf após cada execução.

**Phase 2 (Windsurf Memories Refresh):**
Executa após TODAS as execuções da skill.

**Estratégia: delete → read → recreate**

**Step 1 - Delete:**
- Remove entradas existentes do projeto atual
- Busca por títulos `[overview] <project-name>`, `[technical] <project-name>`, `[conventions] <project-name>`
- Ou por tags `mcp-memory-sync` + `<project-name>`

**Step 2 - Read:**
- Lê estado atual do grafo para as 3 entidades
- Coleta todas as observações atuais

**Step 3 - Recreate:**
- Cria 3 memórias nativas com Markdown estruturado
- Cada memória tem título, tags e conteúdo organizado em seções

**Formato das Memórias:**
- **Overview**: Propósito, Premissa, Escopo, Casos de Uso, Público-alvo
- **Technical**: Stack, Infraestrutura, Arquitetura, Estrutura, Serviços, Configuração
- **Conventions**: Nomenclatura, Estrutura, Erros, Testes, Commits, Proibições, Ferramentas

---

### 6. quality-rules.md
**Função:** Regras de qualidade e relatório final.

**Regras de Qualidade:**
- Prefixo obrigatório `<project-name>/` em todas as entidades
- Nunca operar em entidades de outros projetos
- Apenas 3 entidades fixas por projeto
- Observações devem ser factuais e específicas
- Verificar o projeto real, não assumir
- Uma observação por fato
- Todo conteúdo em pt-BR

**Critérios de Relevância:**
Define o que cada entidade deve capturar e o que nunca deve ser capturado.

**Condição para Windsurf Refresh:**
- Se total de operações = 0 → skip refresh
- Se total de operações > 0 → proceder com refresh

**Relatório Final:**
Apresentado ao final de cada execução com:
- Modo (Initialization/Synchronization)
- Operações por entidade (inseridas/atualizadas/deletadas)
- Status do Windsurf Refresh
- Resumo das mudanças

---

## Fluxo de Execução Completo

```
START
  │
  ├─ load tools-reference.md
  ├─ load quality-rules.md
  │
  ├─ load prerequisites.md
  │     ├─ Phase -1: check MCP Memory
  │     │     └─ if unavailable → STOP
  │     └─ Phase 0: detect state
  │           ├─ no entities → load initialization.md (Phase 1A)
  │           └─ entities found → load synchronization.md (Phase 1B)
  │
  └─ load windsurf-refresh.md (Phase 2 — always)
        └─ delete → read → recreate
              └─ present Final Report
END
```

---

## Características Técnicas

### Isolamento por Projeto
- Cada projeto tem suas 3 entidades isoladas com prefixo
- Operações nunca afetam outros projetos
- Memórias Windsurf também isoladas por tags

### Idioma
- Todo conteúdo das observações deve ser em pt-BR
- Regra estrita e não negociável

### Estratégia de Dupla Escrita
- MCP Memory: fonte primária, grafo de conhecimento
- Windsurf Memories: espelho para UI, formato Markdown estruturado

### Operações Autônomas
- Análise completa do projeto
- Detecção automática de mudanças
- Sincronização incremental quando possível

### Qualidade e Consistência
- Regras estritas de relevância
- Verificação factual obrigatória
- Relatório detalhado de operações

---

## Casos de Uso Recomendados

1. **Inicialização de Projeto**: Primeira execução para criar memória base
2. **Pós-Mudanças Arquiteturais**: Após refatorações significativas
3. **Sincronização Periódica**: Manter contexto atualizado
4. **Onboarding de Novo Desenvolvedor**: Garantir contexto completo disponível

A skill é projetada para ser executada sempre que o usuário desejar sincronizar a memória do projeto, inicializar memória para um novo projeto, atualizar memória após mudanças, ou manter a memória do agente alinhada com o estado real do projeto.
