---
name: /execute-sdd
description: Generate tests (TDD) and execute tasks.md — implement code against spec
---

## /execute-sdd — Test Generation + Execution Phase

Orchestrates TDD test generation and implementation by delegating to the **TDD Builder** and **Builder** agents.

### Prerequisites

- `specs/features/[category]/[feature-name]/tasks.md` (or `specs/bugs/[bug-id]/tasks.md`) must exist
- `specs/features/[category]/[feature-name]/design.md` (or `specs/bugs/[bug-id]/design.md`) must exist
- `specs/features/[category]/[feature-name]/requirements.md` (or `specs/bugs/[bug-id]/requirements.md`) must exist
- `CLAUDE.md` must be read for project standards

### Workflow

1. **Test Generation** (if tasks.md contains `TEST:` prefixed tasks):
   - Read `.claude/agents/tdd-builder.md` for the complete workflow. The agent handles test generation from requirements + design.
   - Verify test files exist in `{{TEST_DIRECTORY}}/`.
   - Log to `specs/test_history.log`.
   - If no JS impact: skip and note "No JS impact. Skipping TDD step."
2. **Implementation**:
   - Read `.claude/agents/builder.md` for the complete workflow. The agent handles execution per tasks.md following the TDD RED-GREEN cycle.
   - Verify against design.md — halt and report if implementation deviates.
   - Check against CLAUDE.md standards.
3. **Final Verification**:
   - All tests must PASS (`{{TEST_COMMAND}}`) before reporting completion.
   - If any test fails, do not report success. Report the failures.
4. **Confirm**: Report success. The user can now run `/validate-sdd`.

### Rules

- **Never** override the TDD Builder or Builder agents' guardrails — see `.claude/agents/tdd-builder.md` and `.claude/agents/builder.md`.
- **No code without specs**: Check that `tasks.md` and `design.md` exist before starting.
- **Strict context**: Use only files in `specs/features/<category>/<feature-name>/` (or `specs/bugs/<bug-id>/`).
- **Halt on deviation**: If code diverges from spec/design, stop and report.
- Respect all CLAUDE.md rules: no `target="_blank"`, CSRF protection, input validation, CSP, accessibility.
