## Regras Java

### Convenções
- Java 17+ (records, sealed classes, pattern matching)
- Google Java Format para formatação
- Sem `null` explícito; usar `Optional<T>`
- Imutabilidade por padrão
- Interfaces sobre classes abstratas

### Testes
- JUnit 5 com `@Test`, `@ParameterizedTest`
- Mockito para mocks
- AssertJ para assertions fluêntes
- Coverage com JaCoCo

### Build e ferramentas
```bash
# Maven
mvn clean verify
mvn test

# Gradle
./gradlew build
./gradlew test
```
