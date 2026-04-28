## Testes Python

### Padrões
- Nomenclatura: `test_<comportamento>_<condição>_<resultado_esperado>`
- Uma assertion lógica por teste
- `parametrize` para múltiplos casos
- Fixtures para setup/teardown

### Cobertura
- Alvo mínimo: 80%
- `pytest --cov=src --cov-report=xml:coverage.xml`

### Async
- `pytest-asyncio` para testes assíncronos
- `asyncio_mode = "auto"` no `pyproject.toml`
