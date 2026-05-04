# Architectural Constraints

> **Init note**: This file is populated after `/init-harness`. It encodes what architectural patterns Claude must preserve and what anti-patterns to avoid for **{{PROJECT_NAME}}**. These are feedforward controls — Claude reads this before every code change.

## Preserve These Patterns

> List architectural patterns that are intentional and must not be changed without explicit instruction.

### {{PATTERN_CATEGORY_1}}
- {{PRESERVE_RULE_1}} — reason: {{WHY_PRESERVE_1}}
- {{PRESERVE_RULE_2}} — reason: {{WHY_PRESERVE_2}}

### {{PATTERN_CATEGORY_2}}
- {{PRESERVE_RULE_3}} — reason: {{WHY_PRESERVE_3}}

### SDD Architecture (always preserve)
- Specs live in `specs/features/<cat>/<name>/` or `specs/bugs/<id>/`
- No code without a spec for behavior changes
- Tests live in `{{TEST_DIRECTORY}}/` mirroring source structure
- History log format: `[YYYY-MM-DD HH:mm] TASK N: [Status] — [description]`

## Anti-Patterns (never introduce)

> List things that would break architecture, security, or consistency.

- {{ANTI_PATTERN_1}} — reason: {{WHY_AVOID_1}}
- {{ANTI_PATTERN_2}} — reason: {{WHY_AVOID_2}}
- Never commit `{{ENV_FILE}}` or secrets — reason: security
- Never introduce build tools without explicit approval — reason: {{WHY_NO_BUILD_TOOLS}}
- Never skip the pre-flight checklist — reason: quality gate

## Stack-Specific Rules

> Add rules specific to your tech stack here after init.

```
{{STACK_SPECIFIC_ARCHITECTURAL_RULES}}
```

> Example (Node.js): "Never use `require()` — this project uses ES modules"
> Example (React): "Never use class components — hooks only"
> Example (Go): "Always handle errors explicitly — never use `_` for error returns"
