# CLAUDE.md — Java Project Guidelines

> **Audience**: Claude (Anthropic) coding assistant.
> **Scope**: General coding guidelines only. See `README.md` for project architecture, module responsibilities, and domain context.

---

## 1. Java Version & Language Features

- **Target**: Java 21 (LTS). All code MUST compile and run on Java 21.
- **Use modern features** when they improve clarity:
  - `record` types for immutable data carriers.
  - `sealed` classes/interfaces for restricted type hierarchies.
  - Pattern matching with `switch` expressions (including record patterns).
  - Text blocks (`"""`) for multi-line strings.
  - `var` for local variables **only** when the type is obvious from the right-hand side.
- **Avoid**:
  - `var` when the inferred type is ambiguous.
  - `Optional` as a field, method parameter, or collection element. Use it **only** as a return type.

---

## 2. Code Style

Follow the [Google Java Style Guide](https://google.github.io/styleguide/javaguide.html) with these overrides:

| Rule | Google Default | Our Override |
|---|---|---|
| Indentation | 2 spaces | **4 spaces** |
| Max line length | 100 columns | **120 columns** |
| Wildcard imports | Discouraged | **Forbidden** — always use explicit imports |
| Static imports | Allowed inline | Group **below** all regular imports, separated by a blank line |
| `@Override` | Recommended | **Required** on every overriding method |
| Javadoc on public API | Optional for "obvious" methods | **Required** on all `public` and `protected` members |

---

## 3. Documentation

- Javadoc must include `@param`, `@return` (unless `void`), and `@throws` tags.
- First sentence: concise summary in third-person declarative — *"Parses the input token."*
- Never leave commented-out code. Use version control for history.

---

## 4. Design Principles

### Core

- **Immutability by default**: Fields `final`. Unmodifiable collections. Records for data carriers.
- **Composition over inheritance**.
- **Program to interfaces**: Declare types as `List`, `Map`, `Set` — not `ArrayList`, `HashMap`.
- **Single Responsibility**: One clear reason to change per class.
- **Fail fast**: Validate inputs early. Throw at the point of detection.

### Dependency Injection

- Prefer constructor injection over field injection. Constructors accept interfaces, not concrete classes.

### Null Safety

- Never return `null` from a method that returns a collection — return an empty collection.
- Use `Objects.requireNonNull()` with a descriptive message in constructors and public methods.
- Annotate with `@Nullable` (from `jakarta.annotation` or `org.jspecify`) where null is explicitly allowed.

### Error Handling

- **Unchecked exceptions** for programming errors and unrecoverable conditions.
- **Checked exceptions** only when the caller can reasonably recover.
- Custom exceptions: extend the most specific base, include a descriptive message, and preserve the original cause via the `Throwable cause` constructor.
- Never catch `Exception`/`Throwable` broadly unless at a top-level boundary, and never swallow exceptions silently.

### Concurrency

- Prefer `java.util.concurrent` utilities over manual `synchronized`. Use `StructuredTaskScope` (Java 21) where appropriate.
- Document thread-safety contracts in Javadoc.

---

## 5. Testing

### Framework Stack

| Concern | Library |
|---|---|
| Test framework | JUnit 5 (Jupiter) |
| Assertions | AssertJ |
| Mocking | Mockito (with `mockito-junit-jupiter` extension) |
| Parameterized tests | `@ParameterizedTest` + `@MethodSource` / `@CsvSource` |

### Structure & Naming

- Tests live under `src/test/java`, mirroring the source package. `Foo.java` → `FooTest.java`.
- Follow **Arrange → Act → Assert** with `// Arrange`, `// Act`, `// Assert` comments separated by blank lines.
- Test method naming: `should_ExpectedBehavior_When_Condition`
  - Example: `should_ThrowIllegalArgumentException_When_InputIsNull`

### Assertions & Mocking

- Always use AssertJ fluent chains over JUnit built-in assertions.
- Use `assertThatThrownBy(() -> ...).isInstanceOf(...).hasMessageContaining(...)` for exception assertions.
- Prefer `when(...).thenReturn(...)` over `doReturn(...).when(...)` unless stubbing void methods or spies.
- Verify interactions sparingly — assert on outputs and state, not internal method calls, unless the interaction itself is the tested behaviour.
- Never mock value objects, records, or data classes. Construct real instances.

### Quality Rules

- **No logic in tests**: No `if`, `for`, `while`, or `try-catch`. Use `@ParameterizedTest` for iteration.
- **No test interdependence**: Each test must be independent and idempotent.
- **Test one behaviour per method** (multiple chained AssertJ assertions on the same subject are fine).
- **Use factory methods** for complex test data (e.g., `defaultConfig()`, `sampleUser()`) to keep tests focused.
- **Integration tests** for boundary components (repositories, external API clients, message handlers).

---

## 6. Build Conventions (Gradle — Groovy DSL)

- Never use deprecated `compile` / `testCompile`. Use `implementation`, `testImplementation`, `compileOnly`, `api`.
- Pin dependency versions explicitly or via a version catalog (`libs.versions.toml`). No `+` or `latest.release` ranges.

---

## 7. Logging

- Use SLF4J (`org.slf4j.Logger`). Use parameterized messages — never string concatenation.

---

## 8. Class Member Ordering

Static fields (constants first) → instance fields → constructors → static factory methods → public methods → package-private → protected → private methods → inner classes/enums/records → `equals()`, `hashCode()`, `toString()` at the end.

---

## 9. General Rules for AI Agents

- **No boilerplate without purpose**. Every class, method, and field must have a reason to exist.
- **No speculative code**. Only implement what is explicitly requested or clearly implied.
- **Preserve existing code style**. Match surrounding style even if it deviates — unless explicitly asked to reformat.
- **Explain non-obvious decisions** in a comment or commit message.
- **Minimal, focused changes**. One concern per commit. No bundled refactors with feature work.
- **Ensure the build passes**. Run `./gradlew build` before considering work complete.
- **Write tests alongside production code**. Every new public method or behaviour change must include tests.
- **No new dependencies without approval**. Propose — don't add.
- **Respect module/package boundaries**. No cross-module dependencies without discussion.
