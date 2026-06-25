# Java Project Guidelines

> **Audience**: Claude (Anthropic) coding assistant. General coding guidelines only.
> **Architecture, modules, domain context**: see `README.md`.

## 1. Language (Java 21 LTS)

All code must compile and run on Java 21. Use modern features where they improve clarity: `record` for immutable data carriers, `sealed` hierarchies for restricted types, pattern matching in `switch` (including record patterns), and text blocks for multi-line strings. Use `var` only when the type is obvious from the right-hand side.

`Optional` is a **return type only** — never a field, method parameter, or collection element.

## 2. Code Style

Follow the [Google Java Style Guide](https://google.github.io/styleguide/javaguide.html), with these overrides:

| Rule | Google default | Our override |
|---|---|---|
| Static imports | inline | group **below** regular imports, separated by a blank line |
| `@Override` | recommended | **required** on every overriding method |
| Javadoc | optional on "obvious" methods | **required** on all `public` / `protected` members |

**Formatting** — Wire a formatter into `./gradlew build` that fails CI on violations. With the indentation and line-length overrides dropped, stock google-java-format now fits the whitespace rules — except it orders static imports first, so add an import-order check (via Spotless or Checkstyle) to keep them below. `@Override` and Javadoc presence aren't formatter concerns; enforce those with Checkstyle.

**Javadoc** must include `@param`, `@return` (unless `void`), and `@throws`. First sentence is a concise third-person summary — *"Parses the input token."* Never leave commented-out code; use version control for history.

## 3. Design

**Core** — Immutability by default: `final` fields, unmodifiable collections, records for data carriers. Composition over inheritance. Program to interfaces (`List`, `Map`, `Set` — not `ArrayList`, `HashMap`). One reason to change per class. Fail fast: validate inputs at the point of detection.

**Dependency injection** — Constructor injection over field injection; constructors accept interfaces, not concrete types.

**Null safety** — Never return `null` from a method that returns a collection; return an empty one. Use `Objects.requireNonNull(x, "message")` in constructors and public methods. Mark explicitly-nullable points `@Nullable` (`jakarta.annotation` or `org.jspecify`).

**Errors** — Unchecked exceptions for programming errors and unrecoverable conditions; checked exceptions only when the caller can reasonably recover. Custom exceptions extend the most specific base, carry a descriptive message, and preserve the cause via the `Throwable cause` constructor. Never catch `Exception` / `Throwable` except at a top-level boundary, and never swallow silently.

**Concurrency** — Prefer `java.util.concurrent` utilities over manual `synchronized`; use `StructuredTaskScope` (Java 21) where it fits. Document thread-safety contracts in Javadoc.

## 4. Testing

| Concern | Library |
|---|---|
| Framework | JUnit 5 (Jupiter) |
| Assertions | AssertJ |
| Mocking | Mockito (`mockito-junit-jupiter`) |
| Parameterized | `@ParameterizedTest` + `@MethodSource` / `@CsvSource` |

**Layout & naming** — Tests under `src/test/java`, mirroring the source package; `Foo` → `FooTest`. Arrange → Act → Assert, with `// Arrange` / `// Act` / `// Assert` comments separated by blank lines. Method names: `should_ExpectedBehavior_When_Condition` (e.g. `should_ThrowIllegalArgumentException_When_InputIsNull`).

**Assertions & mocking** — AssertJ fluent chains over JUnit built-ins; use `assertThatThrownBy(() -> ...).isInstanceOf(...).hasMessageContaining(...)` for exceptions. Prefer `when(...).thenReturn(...)` over `doReturn(...).when(...)` except for void methods and spies. Verify interactions sparingly — assert on state and output, not internal calls, unless the interaction itself is the behavior under test. Never mock value objects, records, or data classes — construct real instances.

**Quality** — No logic in tests (`if` / `for` / `while` / `try-catch`); use `@ParameterizedTest` to iterate. Each test is independent and idempotent. One behavior per method (chained AssertJ assertions on the same subject are fine). Use factory methods for complex fixtures (`defaultConfig()`, `sampleUser()`). Write integration tests for boundary components (repositories, external API clients, message handlers).

## 5. Build (Gradle, Groovy DSL)

Use `implementation` / `testImplementation` / `compileOnly` / `api` — never the deprecated `compile` / `testCompile`. Pin dependency versions explicitly or via a version catalog (`libs.versions.toml`); no `+` or `latest.release` ranges. Propose new dependencies — don't add them without approval.

## 6. Logging

SLF4J (`org.slf4j.Logger`) with parameterized messages — never string concatenation.

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
