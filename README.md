# boilerplate-opencode

Boilerplate genérico de configurações [OpenCode](https://opencode.ai) com suporte a múltiplas stacks.

## Estrutura

```
base/           ← copiado em todos os repos
stacks/         ← copiado apenas se a stack for detectada
  go/
  python/
  node/
  rust/
  java/
scripts/        ← executor local de CI e verificador de TODOs
.github/
  workflows/
    install-to-repo.yml  ← workflow de instalação
```

## Configurações obrigatórias

### Secret: `BOILERPLATE_TOKEN`

O workflow **Install OpenCode Boilerplate** requer um **Personal Access Token (PAT)** configurado como secret no repositório.

**Como criar o token:**

1. Acesse **GitHub → Settings → Developer settings → Personal access tokens → Fine-grained tokens**
2. Clique em **Generate new token**
3. Defina um nome (ex: `boilerplate-opencode`) e data de expiração
4. Em **Repository access**, selecione **All repositories** (ou apenas os repos alvo)
5. Conceda as seguintes **permissões de repositório**:

| Permissão | Nível | Motivo |
|---|---|---|
| `Contents` | Read & Write | Fazer checkout e push de branches nos repos alvo |
| `Pull requests` | Read & Write | Abrir PRs automaticamente |
| `Metadata` | Read | Obrigatório para todos os tokens |

6. Clique em **Generate token** e copie o valor

**Como adicionar o secret ao repositório:**

1. Acesse este repositório → **Settings → Secrets and variables → Actions**
2. Clique em **New repository secret**
3. Nome: `BOILERPLATE_TOKEN`
4. Valor: o token gerado acima
5. Clique em **Add secret**

> **Atenção:** o workflow valida que o repositório alvo pertence ao owner `ElioNeto`. Tokens de outros owners não funcionarão.

---

## Instalação manual

```bash
bash scripts/install.sh ElioNeto/meu-repo
```

## Instalação via GitHub Actions

1. Acesse **Actions** neste repositório
2. Execute o workflow **Install OpenCode Boilerplate**
3. Informe os parâmetros:

| Parâmetro | Obrigatório | Padrão | Descrição |
|---|---|---|---|
| `target_repo` | ✅ Sim | — | Repositório alvo no formato `owner/repo` (ex: `ElioNeto/meu-repo`) |
| `workflow_file` | ❌ Não | `ci.yml` | Arquivo de workflow para executar localmente |
| `overwrite` | ❌ Não | `true` | Sobrescrever arquivos existentes |

4. Uma PR será aberta no repositório alvo com as configurações

## Stacks detectadas automaticamente

| Arquivo detectado | Stack aplicada |
|---|---|
| `go.mod` | Go |
| `pyproject.toml` / `requirements.txt` / `setup.py` | Python |
| `package.json` | Node.js |
| `Cargo.toml` | Rust |
| `pom.xml` / `build.gradle` / `build.gradle.kts` | Java |

## Dependências dos scripts

```bash
bash scripts/install-deps.sh
```

Requer: Node.js ≥ 20, Docker (para execução local do workflow).

## Próximos passos após instalação

1. Revisar e ajustar o `AGENTS.md` com a descrição do projeto
2. Rodar `bash scripts/install-deps.sh` para instalar dependências locais
3. Usar `/shipit` no OpenCode para acionar o fluxo de entrega
