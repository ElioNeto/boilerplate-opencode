# boilerplate-opencode

Boilerplate genérico de configurações [OpenCode](https://opencode.ai) com suporte a múltiplas stacks, fluxo de entrega orientado a issues e estratégia Caveman para redução de consumo de tokens.

## Estrutura

```
base/                        ← copiado em todos os repos
  opencode.json              ← config principal (compaction, MCP GitHub, permissões)
  AGENTS.md                  ← contexto do projeto para os agentes
  .opencode/
    agents/
      orchestrator.md        ← seleciona issue, resolve dependências, coordena
      planner.md             ← decompõe issue em TODOs atômicos
      implementer.md         ← implementa e faz commit
      validator.md           ← valida critérios sem editar código
      reviewer.md            ← revisão de qualidade, testes e segurança
      debugger.md            ← diagnóstico de falhas de CI
    commands/
      shipit.md              ← /shipit: valida TODOs e roda pipeline
      sync-backlog.md        ← /sync-backlog: sincroniza issues → backlog.json
      next-issue.md          ← /next-issue: mostra próxima issue desbloqueada
      update-repos.md        ← /update-repos: propaga boilerplate via PRs
    state/
      backlog.json           ← cache local de issues (gerado pelo /sync-backlog)
      backlog.schema.json    ← schema do backlog
      target-repos.json      ← lista de repos alvo para /update-repos
    ignore                   ← arquivos ignorados pelo agente
stacks/                      ← copiado apenas se a stack for detectada
  go/ | python/ | node/ | rust/ | java/
scripts/                     ← executor local de CI e verificador de TODOs
.github/
  workflows/
    install-to-repo.yml      ← workflow de instalação
ISSUE_TEMPLATE.md            ← template padrão para issues
```

## Fluxo de entrega

```
/sync-backlog          → sincroniza issues abertas → .opencode/state/backlog.json
/next-issue            → mostra próxima issue desbloqueada
orchestrator           → escolhe issue → planner → implementer → validator → loop
/shipit                → valida TODOs + pipeline antes do commit
/update-repos [repos]  → propaga boilerplate para outros repos via PR
```

## Estratégia Caveman (redução de tokens)

Os agentes seguem quatro princípios para minimizar consumo de contexto:

1. **Estado local primeiro** — o orchestrator lê `.opencode/state/backlog.json` antes de qualquer chamada ao MCP GitHub. Só busca o corpo completo da issue escolhida e dependências diretas.
2. **Saída em schema curto** — todos os agentes respondem em formato fixo (NEXT/ACT, PLAN/DONE/TEST, STATUS/CHECK). Sem prosa.
3. **Compaction automática** — `opencode.json` ativa `compaction.auto` e `compaction.prune` para podar contexto antigo.
4. **Snapshot desligado** — `snapshot: false` elimina indexação pesada em repos grandes.

## Template de issue

Use `ISSUE_TEMPLATE.md` ao criar issues nos seus repositórios:

```md
## Priority
high

## Depends on
- #N

## Blocks
- #N

## Acceptance criteria
- [ ] critério verificável
- [ ] testes passando
- [ ] lint sem erros
```

O orchestrator usa `Priority` para ordenação e `Depends on`/`Blocks` para o grafo de dependências. O validator usa `Acceptance criteria` para aprovar ou reprovar.

## Configurações obrigatórias

### Secret: `BOILERPLATE_TOKEN`

O workflow **Install OpenCode Boilerplate** requer um **Personal Access Token (PAT)** configurado como secret no repositório.

**Como criar o token:**

1. Acesse **GitHub → Settings → Developer settings → Personal access tokens → Fine-grained tokens**
2. Clique em **Generate new token**
3. Defina nome e data de expiração
4. Em **Repository access**, selecione os repos alvo
5. Conceda as seguintes permissões:

| Permissão | Nível | Motivo |
|---|---|---|
| `Contents` | Read & Write | Checkout e push nos repos alvo |
| `Pull requests` | Read & Write | Abrir PRs automaticamente |
| `Issues` | Read | Ler issues para o fluxo de entrega |
| `Metadata` | Read | Obrigatório para todos os tokens |

6. Copie o token

**Como adicionar o secret:**

1. Acesse **Settings → Secrets and variables → Actions**
2. Novo secret: `BOILERPLATE_TOKEN` = token gerado

### Variável de ambiente local

```bash
export GH_TOKEN=ghp_...
```

O `opencode.json` usa `{env:GH_TOKEN}` para passar o token ao MCP GitHub.

## Instalação manual

```bash
bash scripts/install.sh ElioNeto/meu-repo
```

## Instalação via GitHub Actions

1. Acesse **Actions** neste repositório
2. Execute **Install OpenCode Boilerplate**
3. Informe os parâmetros:

| Parâmetro | Obrigatório | Padrão | Descrição |
|---|---|---|---|
| `target_repo` | ✅ | — | `owner/repo` (ex: `ElioNeto/meu-repo`) |
| `workflow_file` | ❌ | `ci.yml` | Workflow para execução local |
| `overwrite` | ❌ | `true` | Sobrescrever arquivos existentes |

## Stacks detectadas automaticamente

| Arquivo detectado | Stack |
|---|---|
| `go.mod` | Go |
| `pyproject.toml` / `requirements.txt` / `setup.py` | Python |
| `package.json` | Node.js |
| `Cargo.toml` | Rust |
| `pom.xml` / `build.gradle*` | Java |

## Dependências dos scripts

```bash
bash scripts/install-deps.sh
```

Requer: Node.js ≥ 20, Docker.

## Próximos passos após instalação

1. Ajustar `AGENTS.md` com descrição, stack e comandos do projeto
2. Rodar `bash scripts/install-deps.sh`
3. Exportar `GH_TOKEN` no shell
4. Executar `/sync-backlog` para carregar as issues
5. Executar `/next-issue` para ver a próxima issue desbloqueada
6. Iniciar o loop com o orchestrator
