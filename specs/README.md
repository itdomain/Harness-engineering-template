# Specs — Spec-Driven Development

This directory contains SDD artifacts for features and bugfixes being built or updated.

## Workflow

Six slash commands plus one optional mutation phase orchestrate development through spec → design → plan → execute → mutate → validate. Each command delegates to a specialized agent.

| Step | Command | Agent | Produces |
|------|---------|-------|----------|
| 1. Requirements | `/start-sdd feature [cat] [name]` or `/start-sdd bug [id]` | — | `specs/features/[cat]/[name]/requirements.md` or `specs/bugs/[id]/requirements.md` |
| 2. Design | `/design-sdd` | Architect | `design.md` with schema, Mermaid data flow, architecture decisions, requirements traceability |
| 3. Planning | `/plan-sdd` | Planner | `tasks.md` with atomic test + implementation tasks (JS features only) |
| 4. Test Generation | (built into `/execute-sdd`) | TDD Builder | `{{TEST_FILE_PATTERN}}` + `test_history.log` |
| 5. Execution | `/execute-sdd` | Builder | Code changes + `history.log` (all tests PASS) |
| 5.5. Mutation | `/mutate` | Mutator | `{{MUTATION_REPORT_FILE}}`, mutation score (PASS ≥80%, WARN 60-79%, FAIL <60%) — optional per-feature |
| 6. Validation | `/validate-sdd` | Auditor | `validation_report.md` → archive on PASS |

**Rules:** No code without specs. Test-first for change in behavior (TDD RED-GREEN cycle). Atomic commits per task. Strict context (`specs/features/<cat>/<name>/` or `specs/bugs/<id>/` only). Archive on PASS.

### TDD Verification and Mutation Hardening

JavaScript testing uses {{TEST_FRAMEWORK}} with {{TEST_ENVIRONMENT}} environment and globals enabled. Mutation testing uses {{MUTATION_TOOL}} with the {{TEST_FRAMEWORK}} runner.

**Rules:**
- Tests live in `{{TEST_DIRECTORY}}/` mirroring source structure.
- `{{TEST_COMMAND}}` must pass before execution is declared complete.
- Never modify, weaken, or remove test assertions.
- **Mutation hardening** (Phase 5.5, optional): After green TDD, run `npx {{MUTATION_TOOL_CMD}} run` only if `mutate.md` has `enabled: true`. Thresholds: 80% high, 60% low, 40% break. Survived mutants below 60% must be fixed by writing new targeted tests. Mutation testing is **optional for JS features** — enabled per-feature at SDD start.
- **Always run `{{TEST_COMMAND}} {{TEST_FILE_PATTERN}}` after any change to `{{ENTRY_POINT_FILE}}` or `{{MAIN_SOURCE_FILE}}`. Add this to your workflow — never skip tests for JS/HTML changes.**
- Infrastructure: `package.json`, `{{TEST_CONFIG_FILE}}`, `{{MUTATION_CONFIG_FILE}}`.


## Agent Registry

| Agent | Phase | Role | File |
|-------|-------|------|------|
| Architect | Design | Translate requirements into design specs with schema, data flow, architecture decisions | `.claude/agents/architect.md` |
| Planner | Planning | Break design into atomic micro-tasks (<1hr each) with dependencies and review checkpoints | `.claude/agents/planner.md` |
| TDD Builder | Test Generation | Generate failing test cases from requirements + design (RED cycle) | `.claude/agents/tdd-builder.md` |
| Builder | Execution | Implement code strictly from tasks.md, following TDD RED-GREEN cycle | `.claude/agents/builder.md` |
| Mutator | Mutation Hardening (optional) | Run {{MUTATION_TOOL}} mutation tests, fix survived mutants (if enabled) | `.claude/agents/mutator.md` |
| Auditor | Validation | Verify implementation against requirements, generate validation report | `.claude/agents/auditor.md` |

## Structure

```
specs/
├── README.md              ← this file
├── .template/             ← blank templates (never edited directly)
│   ├── requirements.md    ← business logic, edge cases, performance, success metrics
│   ├── design.md          ← schema, data flow, architecture decisions, traceability
│   ├── tasks.md           ← test tasks + implementation tasks with DoD and dependencies
│   └── validation_report.md ← requirements audit, success metrics, verdict
├── archive/               ← validated spec archives
│   ├── bugs/              ← validated bugfix specs (bug-id-date/)
│   └── features/          ← validated feature specs (category/name-date/)
├── test/                  ← {{TEST_FRAMEWORK}} tests (mirrors source)
│   └── assets/
│       └── js/
│           └── main.test.js
├── features/              ← active feature specs
│   └── [category]/        ← e.g. ui, api, infra, db, auth
│       └── [feature-name]/ ← slugified feature name
│           ├── requirements.md
│           ├── design.md
│           ├── tasks.md
│           ├── history.log        ← execution log ([YYYY-MM-DD HH:mm] TASK N: [Status])
│           ├── test_history.log   ← TDD spec-to-test mapping
│           └── validation_report.md
└── bugs/                  ← active bugfix specs
    └── [bug-id]/          ← e.g. BUG-42-hotfix-login
        ├── requirements.md
        ├── design.md
        ├── tasks.md
        ├── history.log
        └── validation_report.md
```

## Categories

| Category | Scope |
|----------|-------|
| `ui` | Frontend — layout, components, styles, interactions |
| `api` | Backend / form logic / server-side handlers |
| `infra` | Build tools, CI/CD, server config, deployment |
| `db` | Database schema, stored procedures, persistence layer , DTO |
| `auth` | Authentication, authorization, sessions |
| `other` | Anything that doesn't fit above |

## Bugfix Workflow

Bugfixes follow the same SDD phases as features but live under `specs/bugs/`.

1. **Report**: `/start-sdd bug [bug-id]` — creates `specs/bugs/[bug-id]/requirements.md` with bug description, reproduction steps, expected vs actual behavior.
2. **Design → Plan → Execute → Validate**: Same as feature flow, paths under `specs/bugs/[bug-id]/`. TDD applies if bug touches JavaScript.
3. **Archive on PASS**: Move to `specs/archive/bugs/[bug-id]-<date>/`.

## Rules

1. **No code without specs** — all commands check for required `.md` files before proceeding.
2. **Test-first for JS** — when spec touches JavaScript, tests are generated before implementation (TDD RED-GREEN cycle).
3. **Atomic commits** — each task in `tasks.md` maps to one logical commit.
4. **Strict context** — during execution, agents use only files in `specs/features/<category>/<feature-name>/` (or `specs/bugs/<bug-id>/`).
5. **Archive on pass** — validated features are moved to `specs/archive/features/[category]/[feature-name]-<date>/` (bugs → `specs/archive/bugs/[bug-id]-<date>/`).
6. **Mutation hardening** — JS features with enabled mutation testing must pass mutation testing (score ≥ 80%) before validation.
7. **Ubiquitous language** — all names must use canonical terms from `.claude/constraints_middle/DOMAIN_GLOSSARY.md`.
8. **No test weakening** — never modify, weaken, or remove test assertions to force a pass or improve mutation score.
