## Regras Python

### Convenções
- PEP 8 obrigatório (enforced por `ruff`)
- Type hints em todas as funções públicas
- Docstrings em Google style
- `pathlib.Path` em vez de `os.path`
- f-strings em vez de `.format()` ou `%`

### Estrutura
- `src/` layout ou pacote direto na raiz
- `tests/` separado do código fonte
- `pyproject.toml` como único arquivo de configuração

### Testes
- `pytest` com `pytest-cov`
- Fixtures em `conftest.py`
- Mocks com `pytest-mock` ou `unittest.mock`

### Build e ferramentas
```bash
pip install -e .
ruff check .
pytest tests/ -v --cov --cov-report=term-missing
pip-audit
```

### CI jobs locais
- `python-sdk-test`: pip install + ruff + pytest
- `security-python`: pip-audit
