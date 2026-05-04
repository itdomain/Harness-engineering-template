# Mutation Testing State: [FEATURE NAME]

> **Per-feature opt-in.** Created at `/start-sdd` time. Controls whether `/mutate` runs for this feature.
> See `.knowledge_base/harness_engineering_reference/sensors/mutation_testing.md` for decision guide.

## Configuration

```yaml
enabled: false
feature: [feature-name]
category: [category]
reason: [why enabled or skipped]
```

## Thresholds (if enabled)

| Level | Score | Action |
|-------|-------|--------|
| PASS | >= 80% | Proceed to `/validate-sdd` |
| WARN | 60–79% | Fix top survived mutants, then proceed |
| FAIL | < 60% | Block validation — write targeted tests |
| HARD FAIL | < 40% | Stop — do not proceed |

## Decision Guide

**Enable** (`enabled: true`) when:
- Feature touches core business logic
- Public-facing JavaScript ({{MODULE_EXAMPLES}})
- Security-sensitive code (CSRF, input validation)
- Critical user journeys

**Skip** (`enabled: false`) when:
- Minor UI tweaks or CSS-only changes
- Documentation updates
- Trivial bugfixes (< 2 files, < 30 lines)
- Infrastructure or config changes only
