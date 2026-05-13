# Verification Loops

## Two Mechanisms of Control

There are two mechanisms through which the harness controls model behavior:

1. **Guides (feedforward controls).** Anticipate behavior and prevent errors before they happen. Examples: pre-commit hooks, type checkers, linting rules, pre-flight checklists.

2. **Sensors (feedback controls).** Observe outcomes and detect when something goes wrong. Examples: test suites, CI pipelines, code review, security scanners.

## Execution Categories

Harness engineering concept categorizes verification along two axes:

| Category | Characteristics | Examples |
|----------|----------------|----------|
| **Computational** | Deterministic, rapid, reliable | Unit tests, linting, type checking, mutation testing |
| **Inferential** | Semantic, probabilistic, costly | Code review, security audit, architectural review |

Effective harnesses use computational verification for the common case and inferential verification for the edge cases.

## Timing Strategy

### Shift-Left Verification

Deploy lightweight computational checks before heavier inferential analysis:
1. **Pre-change:** Pre-flight checklist (constraint verification)
2. **During change:** Linting, type checking (if applicable)
3. **Post-change:** Test suite execution, security header verification
4. **Post-integration:** Code review, architectural audit

### Continuous Monitoring

Handle gradual drift outside active development cycles:

**Examples:**
- Periodic dependency vulnerability checks
- CSS custom property audit against documented palette
- Section map verification against actual HTML
- CSP header compliance checks

## Current State vs. Target

**Current:** Verification only runs during SDD phases:
- Tests run during `/execute-sdd` (TDD builder + execution)
- Validation during `/validate-sdd` (requirements audit + QA checklist)
- No verification outside SDD workflow

**Target:** Verification at multiple layers:
- **Pre-flight** (`constraints_middle/preflight.md`): Verify constraints before every change
- **Post-change** (`controls_outer/health-check.md`): Automated verification after changes
- **Continuous** (`controls_outer/drift-detection.md`): Periodic checks for architectural drift
- **SDD-specific**: Existing verification loops preserved and enhanced
- **Mutation hardening** (post-TDD, pre-validation): Mutation testing via Stryker or similar Mutation Testing frameworks to maximize test suite sensitivity

## Key Takeaway

A harness without verification loops is just a configuration file. Verification is what makes the harness engineering — it closes the loop between intent and outcome, ensuring the model's output matches what was specified.

## References

- Birgitta Böckeler, "Harness engineering for coding agent users" (2026-04-02) - https://martinfowler.com/articles/harness-engineering.html
- Larry Smith - "Shift-Left Testing" (2001)
- Kent Beck - "Test Driven Development: By Example" (TDD) (2002)
- Mutation testing: `.knowledge_base/harness_engineering_reference/sensors/mutation_testing.md`
- Domain Linter: `.knowledge_base/harness_engineering_reference/sensors/domain_linter.md`
- SDD Decision Guide: `.knowledge_base/harness_engineering_reference/sdd_in_harness_concept.md`
