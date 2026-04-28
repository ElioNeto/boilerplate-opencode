## Testes Node.js

### Padrões
- `describe` + `it` para organização
- Uma assertion principal por `it`
- `beforeEach`/`afterEach` para setup/teardown
- Evitar `setTimeout` em testes; usar fake timers

### Cobertura
- Alvo mínimo: 80%
- `jest --coverage` ou `vitest run --coverage`
