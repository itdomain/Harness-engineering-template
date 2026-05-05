# CLAUDE.md

This file provides guidance for Claude Code (claude.ai/code) when working with this repository.

## Setup

For all prerequisites and installation instructions see README.md.


## Communication

{{COMMUNICATION_STYLE}}

> **Init note**: `/init-harness` will ask whether to enable caveman mode. Leave this placeholder until then, or set manually: add `Use caveman mode (full intensity) for every response` to enable.

## SDD Workflow — Spec-Driven Development

Six slash commands plus an optional mutation phase orchestrate development through spec → design → plan → execute → mutate (optional) → validate. See `.claude/COMMANDS.md` for full registry.

**Delegation pattern**: Each command is a lightweight orchestrator that delegates to its corresponding agent(s). Commands read prerequisites and forward to agent files (`.claude/agents/`). Agents contain the full workflow, rules, and guardrails. Commands should never duplicate agent logic.

| Command | Phase | Produces |
|---------|-------|----------|
| `/start-sdd feature [cat] [name]` or `/start-sdd bug [id]` | Requirements | `specs/features/[cat]/[name]/requirements.md` or `specs/bugs/[id]/requirements.md` |
| `/design-sdd` | Design | `design.md` (delegates to Architect agent) |
| `/plan-sdd` | Task Planning | `tasks.md` with test tasks (delegates to Planner agent) |
| `/execute-sdd` | Execution | `{{TEST_FILE_PATTERN}}` + code changes + `history.log` (delegates to TDD Builder + Builder agents) |
| `/mutate` | Mutation Hardening | `mutation_report.html`, mutation score (delegates to Mutator agent, optional per-feature) |
| `/validate-sdd` | Validation | `validation_report.md` → archive (delegates to Auditor agent) |

**Rules:** No code without specs. Test-first for change in behavior (TDD RED-GREEN cycle). Atomic commits per task. Strict context (`specs/features/<cat>/<name>/` or `specs/bugs/<id>/` only). Archive on PASS.

### TDD Verification and Mutation Hardening

Testing uses {{TEST_FRAMEWORK}} with {{TEST_ENVIRONMENT}} environment and globals enabled. Mutation testing uses {{MUTATION_TOOL}} with the {{TEST_FRAMEWORK}} runner.

**Rules:**
- Tests live in `{{TEST_DIRECTORY}}/` mirroring source structure.
- `{{TEST_COMMAND}}` must pass before execution is declared complete.
- Never modify, weaken, or remove test assertions.
- **Mutation hardening** (Phase 4.5, optional): After green TDD, run `npx {{MUTATION_TOOL}} run` only if `mutate.md` has `enabled: true`. Thresholds: 80% high, 60% low, 40% break. Survived mutants below 60% must be fixed by writing new targeted tests. Mutation testing is **optional for JS features** — enabled per-feature at SDD start.
- **Always run `{{TEST_COMMAND}}` after any change to `{{ENTRY_POINT_FILE}}` or `{{MAIN_SOURCE_FILE}}`. Add this to your workflow — never skip tests for source/entry-point changes.**
- Infrastructure: `package.json`, `{{TEST_CONFIG_FILE}}`, `{{MUTATION_CONFIG_FILE}}`.

## Ubiquitous Language

**Hard gate — before naming anything:** always consult `.claude/constraints_middle/DOMAIN_GLOSSARY.md` before naming anything. Use the exact canonical term from the glossary — never invent synonyms or abbreviations.

**Bounded-context glossaries:**
- **Business:** `.claude/constraints_middle/ubiquitous_language/business/GLOSSARY.md` — domain offerings, bundles, products
- **Organizational:** `.claude/constraints_middle/ubiquitous_language/organizational/GLOSSARY.md` — team, roles, personas
- **Development:** `.claude/constraints_middle/ubiquitous_language/development/GLOSSARY.md` — SDD workflow, agents, commands, technical terms

See `.claude/constraints_middle/CONSULT_THE_GLOSSARY.md` for detailed instructions.

## Architecture

{{PROJECT_DESCRIPTION}}

**Full architecture details:** `.claude/context_inner/architecture.md`

### Core Files

> See `.claude/context_inner/architecture.md` for the full architecture documentation, populated during `/init-harness`.

## Governance

**Hard gate — before every code change:** Read `.claude/constraints_middle/preflight.md` and evaluate every checklist item. If any item fails, stop and fix before proceeding. Never skip this step. Also read the Security, LLM Security and Architectural constraints:
- **Pre-flight checklist:** `.claude/constraints_middle/preflight.md` 
- **Security:** `.claude/constraints_middle/security-constraints.md`
- **LLM Security:** `.claude/constraints_middle/llm-security-constraints.md`
- **Architectural:** `.claude/constraints_middle/architectural-constraints.md`

**Hard gate — after every code change:** Read `controls_outer/health-check.md` for automated verification after changes.
**Hard gate — before any harness change** (any file under `.claude/` or `.knowledge_base/`): Consult `.knowledge_base/harness_engineering_reference/INDEX.md` and verify the change aligns with Harness Engineering principles. Complete the "Harness Changes" checklist in `preflight.md` before proceeding.


## Harness Structure

The harness is organized hierarchically following Harness Engineering principles:

| Directory | Purpose |
|-----------|---------|
| `.claude/context_inner/` | Inner layer — what the model needs to know (architecture, governance, conventions) |
| `.claude/constraints_middle/` | Middle layer — feedforward controls (guides) — pre-flight, security, architectural |
| `.claude/controls_outer/` | Outer layer — feedback controls (sensors) — health checks, drift detection |
| `.claude/handoff_boundary/` | Boundary layer — human-in-the-loop patterns — escalation matrix, confirmation patterns |
| `.claude/agents/` | Agent definitions (10 agents across SDD and operational phases) |
| `.claude/commands/` | Slash commands (10 commands) |
| `.claude/skills/` | Skills (5 skills) |
| `.knowledge_base/` | Reference layer — harness engineering concepts and design rationale |

**Layer rationale:** Inner layer loads for every task (what the model knows). Middle layer enforces before every change (feedforward guides). Outer layer verifies after every change (feedback sensors). Boundary layer escalates to humans when AI judgment is insufficient. See `.knowledge_base/harness_engineering_reference/INDEX.md` for design rationale.

## Registries

- **Agents:** `.claude/AGENTS.md` — 10 agents across SDD and operational phases
- **Commands:** `.claude/COMMANDS.md` — 10 slash commands
- **Skills:** `.claude/SKILLS.md` — 3 skills (caveman, git-commit-push, playwright-cli)


### Design System

> After init, populate this section with your design system tokens. See `.claude/context_inner/architecture.md` for full palette documentation.

### Typography

> After init, populate this section with your typography system. See `.claude/context_inner/architecture.md` for full typography documentation.

### SDD Flow Diagram

The SDD workflow is visualized in `specs/assets/sdd-flow.svg`. **Update the SVG whenever the workflow changes** — adding/removing phases, agents, file paths, or feedback loops. The mutation phase (Phase 4.5) is optional per-feature and gated by the opt-in decision at SDD start.

**Maintenance rules:**
- Keep the SVG in sync with CLAUDE.md's SDD Workflow section. Every command, agent, and phase must appear in the diagram.
- Preserve the project palette (`{{COLOR_BG}}`, `{{COLOR_ACCENT}}`, `{{COLOR_HIGHLIGHT}}`, `{{COLOR_ERROR}}` for FAIL paths).
- Each phase row is self-contained (Command → Agent → Output → Details).
- The feedback loop (FAIL → fix_request.md → re-evaluation) must always be visible.
- Footer must list all file paths and agent references.
- viewBox: `0 0 1200 920`. Use `&lt;` for `<` in SVG text.
