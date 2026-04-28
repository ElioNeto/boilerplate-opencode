## Regras Node.js / TypeScript

### Convenções
- TypeScript obrigatório em projetos novos
- `strict: true` no `tsconfig.json`
- ESM (`"type": "module"`) em projetos novos
- Sem `any` explícito sem comentário justificando
- `async/await` em vez de callbacks ou `.then()` encadeado

### Estrutura
- `src/` para código fonte
- `tests/` ou `src/**/*.test.ts`
- `dist/` para build (no ignore)

### Testes
- Jest ou Vitest
- Coverage com `--coverage`
- Mocks com `jest.mock()` ou `vi.mock()`

### Build e ferramentas
```bash
npm install
npm test
npm run build
npm audit --audit-level=high
```

### CI jobs locais
- `node-sdk-test`: npm install + npm test + npm audit
