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

## Instalação manual

```bash
bash scripts/install.sh ElioNeto/meu-repo
```

## Instalação via GitHub Actions

1. Acesse **Actions** neste repositório
2. Execute o workflow **Install OpenCode Boilerplate**
3. Informe o repositório alvo (ex: `ElioNeto/meu-repo`)
4. Uma PR será aberta no repositório alvo com as configurações

## Stacks detectadas automaticamente

| Arquivo detectado | Stack aplicada |
|---|---|
| `go.mod` | Go |
| `pyproject.toml` / `requirements.txt` / `setup.py` | Python |
| `package.json` | Node.js |
| `Cargo.toml` | Rust |
| `pom.xml` / `build.gradle` | Java |

## Dependências dos scripts

```bash
bash scripts/install-deps.sh
```

Requer: Node.js ≥ 20, Docker (para execução local do workflow).
