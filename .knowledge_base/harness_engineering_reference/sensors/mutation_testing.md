# Mutation Testing

## Definition

Mutation testing evaluates test effectiveness by applying minor, systematic alterations to source code. Each mutated version is called a **mutant**. Tests that detect the mutation and cause a failure are said to have **killed** the mutant. Tests that do not detect the change result in a **surviving** mutant. It is a **sensor** (feedback control) in the Harness Engineering framework.

The **mutation score** is calculated as:

```
mutation score = killed / (killed + survived) × 100
```

A high mutation score indicates that the test suite is sensitive to real changes in behavior. A low score reveals weak or missing tests.

## Opt-in Configuration (Feedforward Guide for a Feedback Sensor)

Mutation testing is a **feedback sensor** — it evaluates outcomes after code changes. Because mutation testing is computationally expensive, it is gated by a **feedforward guide** that decides whether the sensor should run for a given feature.

**Mechanism**: A state file created at SDD start (`/start-sdd`) records the opt-in decision. The sensor checks this file before executing; if disabled, it logs a skip and proceeds.

**Conceptual distinction**:
- **Opt-in decision** = Guide (feedforward) — prevents wasting resources on low-value sensor runs
- **Running the mutation test** = Sensor (feedback) — detects test suite gaps after code changes

**When to enable**:
- Enable for features touching core business logic, public-facing JS, security-sensitive code, or critical user journeys
- Skip for minor UI tweaks, CSS-only changes, documentation updates, trivial bugfixes

**Rationale**: Ashby's Law of Requisite Variety — a harness that runs every sensor on every change is wasteful and slows feedback. Selective sensor activation matches harness complexity to system complexity.

## Core Concepts

### Mutants

A mutant is a semantically modified version of the original source code. The modification is small and syntactically correct — it should not cause compilation errors. The purpose is to simulate common programming mistakes.

### Killed vs Survived

- **Killed**: At least one test fails when running against the mutant. The test suite detected the artificial bug.
- **Survived**: All tests pass against the mutant. The test suite failed to detect the change, indicating a gap in test coverage.
- **Timeout**: The mutation engine could not complete within the time limit. Usually indicates an infinite loop in the mutant.
- **No Coverage**: The mutant is in code not executed by any test.
- **Equivalent**: The mutant is semantically identical to the original code. It cannot be killed without changing the program's behavior.

### Mutation Score Interpretation

| Score | Interpretation |
|-------|---------------|
| 90%+ | Excellent test suite |
| 80-89% | Good — minor gaps |
| 60-79% | Moderate — significant gaps |
| < 60% | Weak — major gaps in test coverage |

## Mutation Types

### Arithmetic Operators

Replace arithmetic operators with alternatives:

| Original | Mutated |
|----------|---------|
| `+` | `-` |
| `-` | `+` |
| `*` | `/` |
| `/` | `*` |
| `%` | `*` |

### Logical Operators

Swap logical operators:

| Original | Mutated |
|----------|---------|
| `&&` | `\|\|` |
| `\|\|` | `&&` |
| `<` | `<=` |
| `<=` | `<` |
| `>` | `>=` |
| `>=` | `>` |
| `==` | `!=` |
| `!=` | `==` |

### Assignment Operators

Replace assignment operators:

| Original | Mutated |
|----------|---------|
| `=` | `+=`, `-=`, `*=`, `/=` |
| `+=` | `-=` |
| `-=` | `+=` |
| `*=` | `/=` |
| `/=` | `*=` |

### Boolean Literals

Flip boolean values:

| Original | Mutated |
|----------|---------|
| `true` | `false` |
| `false` | `true` |

### String Literals

Replace string literals with empty strings or other values.

### Array Literals

Replace array elements with default values.

### Function Return Values

Replace function return statements with default values (null, undefined, 0, empty string).

## Tool Comparison

| Tool | Language | Test Runner Integration | Coverage Analysis | HTML Report |
|------|----------|----------------------|-------------------|-------------|
| **Stryker** | JS/TS, PHP, C#, Scala | Vitest, Jest, Mocha, PHPUnit | perTest | Yes |
| **Pitest** | Java, Kotlin | JUnit, TestNG | line/branch | Yes |
| **Mutmut** | Python | pytest | yes | Yes |
| **Infection** | PHP | PHPUnit, Pest | line/branch | Yes |

**Stryker** is the chosen tool for this project due to native Vitest integration, ESM support, and comprehensive reporting.

## Integration with TDD

Mutation testing is a **post-TDD** verification step:

1. Write failing test (RED)
2. Implement minimal code to pass (GREEN)
3. Run mutation test to verify test effectiveness
4. If mutants survive, write additional tests targeting the survived mutants
5. Re-run mutation test until threshold is met

This ensures tests are not just passing, but actively guarding against behavioral changes.

## Best Practices

- Focus mutation testing on core business logic, not trivial utilities
- Use coverage analysis (`perTest`) to identify which tests cover which mutants
- Set meaningful thresholds: 80% high, 60% low, 40% break
- Treat survived mutants as actionable items — write targeted tests
- Never weaken or remove existing tests to improve mutation score
- Run mutation tests selectively on changed files during development
- Use HTML reports to visualize mutation score by file

## The Coupling Effect

The theoretical foundation for mutation testing is the **Coupling Effect**, established by DeMillo, Lipton, and Sayward (1978). The hypothesis states that test suites capable of detecting small (simple) faults will also detect larger, more complex faults. This means that if a test suite can catch a simple operator change (e.g., `<` vs `<=`), it is statistically likely to also catch more complex bugs like incorrect algorithm implementations or logic errors.

## References

- DeMillo, R. A., Lipton, R. J., & Sayward, F. G. (1978-04-29). "Hints on Test Data Selection: Help for the Practicing Programmer". Page(s): 34 - 41. https://doi.org/10.1109/C-M.1978.218136
