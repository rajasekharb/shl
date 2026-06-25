# Java Project Guidelines

> **Audience**: AI Coding Assistants and Human Engineers.
> **Architecture, modules, domain context**: see `README.md`.

## 1. Language (Java 21 LTS)
All code must compile and run on Java 21. Use modern features to express intent clearly: `record` for immutable data carriers, `sealed` hierarchies for restricted types (especially error modeling), pattern matching in `switch` (including record patterns), and text blocks for multi-line strings. Use `var` only when the type is obvious from the right-hand side.

Leverage Java 21 **Sequenced Collections** (`SequencedList`, `SequencedSet`, `SequencedMap`) when order matters, utilizing `.getFirst()`, `.getLast()`, and `.reversed()` instead of manual index manipulation.

`Optional` is a **return type only** — never a field, method parameter, or collection element.

Always parameterize generics — no raw types. Avoid `var` when the inferred type is ambiguous or the declaration spans method calls that obscure intent.

## 2. Code Style
Follow the [Google Java Style Guide](https://google.github.io/styleguide/javaguide.html), with these overrides:

| Rule           | Google default                | Our override                                                                                   |
|----------------|-------------------------------|------------------------------------------------------------------------------------------------|
| Static imports | inline                        | group **below** regular imports, separated by a blank line                                     |
| `@Override`    | recommended                   | **required** on every overriding method                                                        |
| Javadoc        | optional on "obvious" methods | **required** on public/protected APIs, except simple records, getters, and setters             |

**Formatting** — Wire a formatter into `./gradlew build` that fails CI on violations. With the indentation and line-length overrides dropped, stock google-java-format now fits the whitespace rules — except it orders static imports first, so add an import-order check (via Spotless or Checkstyle) to keep them below. `@Override` and Javadoc presence aren't formatter concerns; enforce those with Checkstyle.

**Formatting details** — K&R braces (opening brace on the same line); braces are required even on single-line `if`/`else`/`for`/`while` blocks. One blank line between methods; two between class-level sections (fields → constructors → methods). No trailing blank lines at end of file. File encoding: UTF-8, no BOM. Exactly one newline at EOF.

**Naming** — Follow Google Java Style for packages (`all.lowercase`), classes (`UpperCamelCase`), methods (`lowerCamelCase`, verb-first), constants (`UPPER_SNAKE_CASE`), and type parameters (`T`, `E`, `K`, `V`). Boolean-returning methods and boolean variables must use an `is`, `has`, `can`, or `should` prefix (e.g. `isValid()`, `hasChildren`).

**Import ordering** — Imports must appear in this sequence, each group separated by a blank line: `java.*` → `javax.*` → `jakarta.*` → `org.*` → `com.*` → all other third-party → project-internal packages → (blank line) → static imports.

**Javadoc** must include `@param`, `@return` (unless `void`), and `@throws`. First sentence is a concise third-person summary — *"Parses the input token."* Use `{@code ...}` for inline code references and `{@link ...}` for type/method cross-references. Never leave commented-out code; use version control for history.

**Inline comments** — Use `//` comments sparingly; code should be self-documenting first. Use `// TODO:` with a short description for planned work. Use `// FIXME:` for known bugs or fragile code that needs attention.

## 3. Architecture & Design
**Domain-Centric Boundaries** — The core domain must not depend on infrastructure or frameworks (Dependency Rule). Do not leak infrastructure annotations (like `@Entity` or `@JsonProperty`) into domain models. Dependencies must point inward.

**Core** — Immutability by default: `final` fields, unmodifiable collections, records for data carriers. Composition over inheritance. Program to interfaces (`List`, `Map`, `Set` — not `ArrayList`, `HashMap`). One reason to change per class. Fail fast: validate inputs at the point of detection.

**Dependency injection** — Constructor injection over field injection; constructors accept interfaces, not concrete types. Avoid service locator patterns or static factory methods that hide dependencies.

**Null safety** — Never return `null` from a method that returns a collection; return an empty one. Use `Optional<T>` as a return type when absence is a valid, expected outcome. Use `Objects.requireNonNull(x, "message")` in constructors and public methods. Annotate with `@Nullable` (`jakarta.annotation` or `org.jspecify`) where null is explicitly allowed; ensure the chosen annotation library is declared as a `compileOnly` dependency.

**Errors** — Unchecked exceptions for programming errors and unrecoverable conditions; checked exceptions only when the caller can reasonably recover. Custom exceptions extend the most specific base, carry a descriptive message, and preserve the cause via the `Throwable cause` constructor. Never catch `Exception` / `Throwable` except at a top-level boundary, and never swallow silently.

**Concurrency** — Prefer `java.util.concurrent` utilities over manual `synchronized`. **Virtual Threads** are the default for blocking I/O; never use platform thread pools (like `Executors.newFixedThreadPool()`) for I/O bound tasks. Use `StructuredTaskScope` (Java 21) for coordinating concurrent subtasks. Document thread-safety contracts in Javadoc. Achieve thread-safety by passing immutable objects.

## 4. Testing
| Concern       | Library                                                |
|---------------|--------------------------------------------------------|
| Framework     | JUnit 5 (Jupiter)                                      |
| Assertions    | AssertJ                                                |
| Mocking       | Mockito (`mockito-junit-jupiter`) + Hand-written Fakes |
| Parameterized | `@ParameterizedTest` + `@MethodSource` / `@CsvSource`  |
| Integration   | Testcontainers (JUnit Jupiter integration)             |

**Strategy: Fakes > Mocks** — For infrastructure boundaries (e.g., Database Repositories, HTTP Clients), prefer writing in-memory `Fake` implementations over complex Mockito setups. Fakes provide higher fidelity and less test fragility. Use Mockito sparingly to verify specific, hard-to-simulate edge-case interactions (e.g., network timeouts). Never mock records or data classes.

**Layout & naming** — Tests under `src/test/java`, mirroring the source package; `Foo` → `FooTest`. Arrange → Act → Assert, with `// Arrange` / `// Act` / `// Assert` comments separated by blank lines. Method names: `should_ExpectedBehavior_When_Condition` (e.g. `should_ThrowIllegalArgumentException_When_InputIsNull`).

**Assertions & mocking** — AssertJ fluent chains over JUnit built-ins; use `assertThatThrownBy(() -> ...).isInstanceOf(...).hasMessageContaining(...)` for exceptions. Prefer `when(...).thenReturn(...)` over `doReturn(...).when(...)` except for void methods and spies. Verify interactions sparingly — assert on state and output, not internal calls, unless the interaction itself is the behavior under test. Never mock value objects, records, or data classes — construct real instances.

**Quality** — No logic in tests (`if` / `for` / `while` / `try-catch`); use `@ParameterizedTest` to iterate. Each test is independent and idempotent. One behavior per method (chained AssertJ assertions on the same subject are fine). Use factory methods for complex fixtures (`defaultConfig()`, `sampleUser()`). Write integration tests for boundary components (repositories, external API clients, message handlers) using **Testcontainers** to verify against real infrastructure. Do not use Fakes to verify SQL/HTTP syntax. 100% of domain logic must be tested hermetically.

## 5. Build (Gradle, Kotlin DSL)
Use `implementation` / `testImplementation` / `compileOnly` / `api` — never the deprecated `compile` / `testCompile`. Pin dependency versions explicitly or via a version catalog (`libs.versions.toml`); no `+` or `latest.release` ranges. Use `.gradle.kts` files for strict typing and IDE autocompletion. Propose new dependencies — don't add them without approval.

## 6. Logging (Log4j2 Production Standard)
**Implementation**: Bind SLF4J to Log4j2. Use `log4j2.xml` to output asynchronous JSON logs in production, and standard pattern text in development.
**Garbage-Free Logging**: Ensure Log4j2 is configured to run in "garbage-free" mode (`-Dlog4j2.enableThreadlocals=true`) to avoid object allocations in high-throughput critical paths.
**Structured Logging**: Use the SLF4J 2.x Fluent API (`logger.atInfo().addKeyValue("k","v").log()`) for event-specific structured data. Use MDC only for cross-cutting request state (e.g., Trace IDs) to ensure JSON payloads are queryable.

## 7. Class Member Ordering
Static fields (constants first) → instance fields → constructors → static factories → public → package-private → protected → private methods → nested types → `equals()` / `hashCode()` / `toString()` last.

## 8. Working Rules
- Implement only what is requested or clearly implied — no speculative code or purposeless boilerplate.
- Keep changes minimal and focused: one concern per commit; don't bundle refactors with feature work.
- Follow §2 for any code you write or change. Don't reformat unrelated surrounding code — but where its existing style conflicts with these guidelines, follow the guidelines and flag the file rather than matching the deviation.
- Explain non-obvious decisions in a comment or commit message.
- Every new public method or behavior change ships with tests (§4).
- Respect module/package boundaries; no cross-module dependencies without discussion.
- Run `./gradlew build` before considering work complete.
