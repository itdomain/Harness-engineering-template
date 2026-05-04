# Drift Detection

> **Init note**: This file defines sensor checks for {{PROJECT_NAME}}. After `/init-harness`, populate each section with project-specific checks. Run when: after any significant change, before commits, during code review.

## Design Token Drift

> Add your design system tokens here. Claude verifies these haven't drifted from spec.

| Token | Expected value | Notes |
|-------|---------------|-------|
| `{{DESIGN_TOKEN_1}}` | `{{COLOR_BG}}` | Primary background |
| `{{DESIGN_TOKEN_2}}` | `{{COLOR_ACCENT}}` | Primary accent |

## Section / Module Map Drift

> List your application's key sections or modules. Claude verifies they haven't been accidentally removed or renamed.

| Section/Module | Expected identifier | Notes |
|----------------|--------------------|----- |
| `{{MODULE_1}}` | `{{MODULE_1_ID}}` | — |
| `{{MODULE_2}}` | `{{MODULE_2_ID}}` | — |

## Dependency Vulnerabilities

> Track critical dependency version ranges. Claude checks these before dependency updates.

| Package | Current range | Notes |
|---------|--------------|-------|
| `{{TEST_FRAMEWORK_DEPENDENCY}}` | `{{TEST_FRAMEWORK_VERSION_RANGE}}` | Test framework |
| `{{MUTATION_TOOL_DEPENDENCY}}` | latest | Mutation testing |

## Terminology Drift

Check: all canonical terms from `.claude/constraints_middle/DOMAIN_GLOSSARY.md` are used consistently.
Check: no legacy/synonym terms appear in new code.

## LLM Behavior Drift

| Signal | Threshold | Action |
|--------|-----------|--------|
| Uses non-glossary terms | Any occurrence | Flag and correct |
| Skips pre-flight checklist | Any skip | Halt and re-run |
| Introduces undocumented dependencies | Any | Escalate |
| Modifies `{{ENV_FILE}}` directly | Any | Escalate immediately |

## MCP Permission Drift

- Verify MCP tools listed in `settings.local.json` match currently installed servers
- Verify no new MCP servers added without explicit approval
- Verify deny rules still include `{{ENV_FILE}}`

## When to Run

- After any change to `{{MAIN_SOURCE_FILE}}` or `{{ENTRY_POINT_FILE}}`
- Before SDD validation phase
- After dependency updates
- When LLM behavior seems inconsistent
