---
name: /mutate
description: Run mutation testing to harden the test suite — optional per-feature
---

## /mutate — Mutation Hardening Phase (Phase 4.5)

Orchestrates mutation testing by delegating to the **Mutator** agent.

### Prerequisites

- `/execute-sdd` must have completed successfully
- All tests must be passing (`npm test`)
- Feature must touch JavaScript files (skipped for PHP-only features)
- `mutate.md` must exist with `enabled: true` (set at SDD start)

### Workflow

1. **Delegating to Mutator agent**: Read `.claude/agents/mutator.md` for the complete workflow. The agent handles:
   - Opt-in check (skip if `mutate.md` absent or `enabled: false`)
   - Scope identification (which JS files changed)
   - Mutation test execution (`npx stryker run`)
   - Result parsing and threshold evaluation
   - Survived mutant remediation (when score < 60%)
   - Logging and gate enforcement

2. **Report**: Forward the agent's result to the user. If the agent skipped mutation testing, report the skip. If it passed, allow progression to `/validate-sdd`. If it failed, halt and do not allow `/validate-sdd` until the mutation score improves.

### Rules

- **Optional per-feature**: `/mutate` only runs if `mutate.md` has `enabled: true` (decided at SDD start).
- **Never** override the Mutator agent's guardrails — see `.claude/agents/mutator.md`.
- Respect all CLAUDE.md rules: no `target="_blank"`, CSRF protection, input validation, CSP, accessibility.
