# CLAUDE.md — Java Project Guidelines

> **Audience**: Claude (Anthropic) coding assistant.
> **Purpose**: Enforces consistent code quality, style, and testing practices across all contributions.
> **Scope**: General coding guidelines only. See `README.md` for project architecture, module responsibilities, and domain context.

---

## 1. Java Version & Language Features

- **Target**: Java 21 (LTS). All code MUST compile and run on Java 21.
- **Preferred modern features** (use them when they improve clarity):
  - `record` types for immutable data carriers.
  - `sealed` classes/interfaces for restricted type hierarchies.
  - Pattern matching with `switch` expressions (including record patterns).
  - Text blocks (`"""`) for multi-line strings (SQL, JSON, templates).
  - `var` for local variables **only** when the type is obvious from the right-hand side.
- **Avoid**:
  - `var` when the inferred type is ambiguous or the declaration spans method calls that obscure intent.
  - Raw types. Always parameterize generics.
  - `Optional` as a field, method parameter, or collection element. Use it only as a return type.

---

## 2. Code Style — Baseline

Follow the [Google Java Style Guide](https://google.github.io/styleguide/javaguide.html) with the overrides listed below.

### 2.1 Overrides & Clarifications

| Rule | Google Default | Our Override |
|---|---|---|
| Indentation | 2 spaces | **4 spaces** |
| Max line length | 100 columns | **120 columns** |
| Wildcard imports | Discouraged | **Forbidden** — always use explicit imports |
| Static imports | Allowed inline | Group static imports **below** all regular imports, separated by a blank line |
| `@Override` | Recommended | **Required** on every overriding method, no exceptions |
| Javadoc on public API | Optional for "obvious" methods | **Required** on all `public` and `protected` members |

### 2.2 Naming Conventions

| Element | Convention | Example |
|---|---|---|
| Packages | All lowercase, no underscores | `com.project.core.engine` |
| Classes / Interfaces | `UpperCamelCase` | `QueryExecutor`, `Parseable` |
| Methods | `lowerCamelCase`, verb-first | `parseToken()`, `buildIndex()` |
| Constants (`static final`) | `UPPER_SNAKE_CASE` | `MAX_RETRY_COUNT` |
| Local variables / params | `lowerCamelCase` | `retryCount`, `inputStream` |
| Type parameters | Single uppercase letter or short name | `T`, `E`, `K`, `V`, `ID` |
| Test classes | Mirror source class + `Test` suffix | `QueryExecutorTest` |
| Test methods | `should_ExpectedBehavior_When_Condition` | `should_ThrowException_When_InputIsNull` |
| Boolean methods/variables | Prefix with `is`, `has`, `can`, `should` | `isValid()`, `hasChildren` |

### 2.3 Import Ordering

Imports must be ordered in this exact sequence, each group separated by a blank line:

```
1. java.*
2. javax.*
3. jakarta.*
4. org.*
5. com.*
6. (all other third-party)
7. (project internal packages)
   ── blank line ──
8. static imports
```

### 2.4 Formatting Details

- **Braces**: K&R style (opening brace on the same line). Required even for single-line `if`/`else`/`for`/`while` blocks.
- **Blank lines**: One blank line between methods. Two blank lines between class-level sections (fields → constructors → methods). No trailing blank lines at end of file.
- **Trailing whitespace**: None. Configure your editor/formatter to strip it.
- **File encoding**: UTF-8, no BOM.
- **Newline at EOF**: Always include exactly one newline at the end of every file.

---

## 3. Documentation

### 3.1 Javadoc

- **Required** on all `public` and `protected` classes, interfaces, methods, and constructors.
- **Optional** on `private` and package-private members — add if the logic is non-trivial.
- Use `@param`, `@return`, and `@throws` tags. Omit `@return` only if the method returns `void`.
- First sentence must be a concise summary (this becomes the index entry). Use third-person declarative: *"Parses the input token."* — not *"This method parses..."* or *"Parse the input token."*
- Use `{@code ...}` for inline code references and `{@link ...}` for type/method cross-references.

### 3.2 Inline Comments

- Use `//` comments sparingly — code should be self-documenting first.
- Use `// TODO:` for planned work. Include a short description of what needs to be done.
- Use `// FIXME:` for known bugs or fragile code that needs attention.
- Never leave commented-out code in the codebase. Use version control for history.

---

## 4. Design Principles

### 4.1 Core Principles

- **Immutability by default**: Make fields `final`. Return unmodifiable collections. Use records for data carriers.
- **Composition over inheritance**: Favor delegation and interfaces over deep class hierarchies.
- **Program to interfaces**: Declare variables, parameters, and return types as interfaces (`List`, `Map`, `Set`) — not implementations (`ArrayList`, `HashMap`).
- **Single Responsibility**: Each class should have one clear reason to change.
- **Fail fast**: Validate inputs early. Throw exceptions at the point of detection, not deep in the call stack.

### 4.2 Dependency Injection

- Prefer constructor injection over field injection.
- Constructors should accept interfaces, not concrete classes.
- Avoid service locator patterns or static factory methods that hide dependencies.

### 4.3 Null Safety

- **Never return `null`** from a method that returns a collection — return an empty collection instead.
- Use `Optional<T>` as a return type when absence is a valid, expected outcome.
- Use `Objects.requireNonNull()` with a descriptive message in constructors and public methods to validate non-null parameters.
- Annotate with `@Nullable` (from `jakarta.annotation` or `org.jspecify`) where null is explicitly allowed.

### 4.4 Error Handling

- **Unchecked exceptions** (`RuntimeException` subclasses) for programming errors and unrecoverable conditions.
- **Checked exceptions** only when the caller can reasonably recover from the failure.
- Create custom exception classes for domain-specific errors. Name them with an `Exception` suffix.
- Custom exceptions must:
  - Extend the most specific applicable base exception.
  - Include a descriptive message.
  - Preserve the original cause via the `Throwable cause` constructor parameter.
- **Never** catch `Exception` or `Throwable` broadly, unless you are at a top-level boundary (e.g., main loop, request handler) and explicitly logging/re-throwing.
- **Never** swallow exceptions silently (empty `catch` block).

### 4.5 Concurrency

- Prefer `java.util.concurrent` utilities over manual `synchronized` blocks.
- Use `ExecutorService` / `StructuredTaskScope` (Java 21) for concurrent task management.
- Make shared mutable state obvious and minimize it. Document thread-safety contracts in Javadoc.

---

## 5. Testing Guidelines

### 5.1 Framework Stack

| Concern | Library |
|---|---|
| Test framework | JUnit 5 (Jupiter) |
| Assertions | AssertJ |
| Mocking | Mockito (with `mockito-junit-jupiter` extension) |
| Parameterized tests | `@ParameterizedTest` + `@MethodSource` / `@CsvSource` |

### 5.2 Test Structure

- **Location**: Tests live under `src/test/java` mirroring the source package structure.
- **One test class per source class**: `Foo.java` → `FooTest.java` in the same package.
- **AAA Pattern**: Every test method should follow **Arrange → Act → Assert** with a clear visual separation (blank lines between sections):

```java
@Test
void should_ReturnParsedToken_When_InputIsValid() {
    // Arrange
    var parser = new TokenParser(defaultConfig());
    var input = "valid_token_input";

    // Act
    var result = parser.parse(input);

    // Assert
    assertThat(result).isNotNull();
    assertThat(result.type()).isEqualTo(TokenType.IDENTIFIER);
    assertThat(result.value()).isEqualTo("valid_token_input");
}
```

### 5.3 Test Naming

- Method naming: `should_ExpectedBehavior_When_Condition`
- Examples:
  - `should_ThrowIllegalArgumentException_When_InputIsNull`
  - `should_ReturnEmptyList_When_NoMatchesFound`
  - `should_IncrementRetryCount_When_ConnectionFails`
- Use `@DisplayName` for human-readable descriptions when the method name alone is not sufficient.

### 5.4 Assertions (AssertJ)

- **Always prefer AssertJ** over JUnit's built-in assertions.
- Use fluent chains for readability:

```java
// ✅ Good — AssertJ
assertThat(results)
    .hasSize(3)
    .extracting(Result::status)
    .containsExactly(Status.SUCCESS, Status.PENDING, Status.FAILED);

// ❌ Bad — JUnit built-in
assertEquals(3, results.size());
assertEquals(Status.SUCCESS, results.get(0).status());
```

- For exception assertions:

```java
// ✅ Good
assertThatThrownBy(() -> service.process(null))
    .isInstanceOf(IllegalArgumentException.class)
    .hasMessageContaining("must not be null");
```

### 5.5 Mocking (Mockito)

- Use `@ExtendWith(MockitoExtension.class)` at the class level.
- Use `@Mock` for dependencies, `@InjectMocks` for the subject under test.
- Prefer `when(...).thenReturn(...)` over `doReturn(...).when(...)` unless stubbing void methods or spies.
- **Verify interactions sparingly** — assert on outputs and state, not on internal method calls, unless the interaction itself is the behaviour being tested (e.g., "did we send an email?").
- Never mock value objects, records, or data classes. Construct real instances.

### 5.6 Test Quality Rules

- **No logic in tests**: No `if`, `for`, `while`, or `try-catch` in test methods. If you need iteration, use `@ParameterizedTest`.
- **No test interdependence**: Each test must be independent and idempotent. No shared mutable state between tests.
- **Test one behaviour per method**: A test should have a single logical assertion (which can be multiple AssertJ chained assertions on the same subject).
- **Use factory methods**: For complex test data, create private helper methods (e.g., `defaultConfig()`, `sampleUser()`), or use a builder/fixture pattern. Keep test methods focused on the scenario, not on setup boilerplate.
- **Test edge cases**: Null inputs, empty collections, boundary values, error paths — not just the happy path.

### 5.7 Test Coverage Expectations

- **Unit tests are mandatory** for all business logic, utilities, and service classes.
- **Integration tests** for boundary components (repositories, external API clients, message handlers).
- Focus coverage on **behaviour and branching logic**, not on getters/setters or trivial delegation.

---

## 6. Build & Dependency Conventions (Gradle — Groovy DSL)

### 6.1 Dependency Declarations

- Use `implementation` for runtime dependencies.
- Use `testImplementation` for test-only dependencies.
- Use `compileOnly` for compile-time-only annotations (e.g., Lombok, JSpecify).
- Use `api` only in library modules where the dependency is part of the public API.
- **Never** use `compile` or `testCompile` (deprecated).
- Pin dependency versions explicitly or via a version catalog (`libs.versions.toml`). Avoid `+` or `latest.release` version ranges.

### 6.2 Build File Style

- Keep `build.gradle` files lean. Extract shared configuration to a convention plugin or `buildSrc`.
- Group dependency declarations logically (core → testing → tooling).

---

## 7. Logging

- Use SLF4J as the logging facade (`org.slf4j.Logger`).
- Use parameterized logging — **never** string concatenation:

```java
// ✅ Good
logger.info("Processing batch: id={}, size={}", batchId, items.size());

// ❌ Bad
logger.info("Processing batch: id=" + batchId + ", size=" + items.size());
```

- Log levels:
  - `ERROR` — system failures that need immediate attention.
  - `WARN` — recoverable but unexpected conditions.
  - `INFO` — significant lifecycle events (startup, shutdown, batch completion).
  - `DEBUG` — detailed diagnostic data for troubleshooting.
  - `TRACE` — very fine-grained, typically disabled in production.

---

## 8. Code Organization within a Class

Order members within a class in this sequence:

```
1. Static fields (constants first, then mutable statics)
2. Instance fields
3. Constructors
4. Static factory methods
5. Public methods
6. Package-private methods
7. Protected methods
8. Private methods
9. Inner classes / enums / records (static first, then non-static)
10. equals(), hashCode(), toString() — at the very end
```

---

## 9. General Rules for AI Agents

- **Do not generate boilerplate without purpose**. Every class, method, and field must have a reason to exist.
- **Do not add speculative code**. Only implement what is explicitly requested or clearly implied by the task.
- **Preserve existing code style**. When modifying existing files, match the surrounding style even if it deviates from these guidelines — unless explicitly asked to reformat.
- **Explain non-obvious decisions**. If a design trade-off is made, leave a brief comment or mention it in the PR/commit message.
- **Keep changes minimal and focused**. One concern per commit. Do not bundle unrelated refactors with feature work.
- **Always ensure the build passes**. Run `./gradlew build` (or equivalent) mentally/actually before considering work complete.
- **Write tests alongside production code**. Every new public method or behaviour change must include corresponding tests.
- **Do not introduce new dependencies** without explicit approval. If a library would help, propose it — don't just add it.
- **Respect the existing module/package boundaries** defined in the project. Do not create cross-module dependencies without discussion.
