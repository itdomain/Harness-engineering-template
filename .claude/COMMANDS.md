# Command Registry

Compact registry of all slash commands. Each command file orchestrates a workflow by reading agent files and skills.

Harness structure: `.claude/context_inner/` (inner — knowledge), `.claude/constraints_middle/` (middle — guides), `.claude/controls_outer/` (outer — sensors), `.claude/handoff_boundary/` (boundary — human-in-the-loop).

## SDD Commands

| Command | Phase | File |
|---------|-------|------|
| `/start-sdd` | Requirements | `.claude/commands/start-sdd.md` |
| `/design-sdd` | Design | `.claude/commands/design-sdd.md` |
| `/plan-sdd` | Task Planning | `.claude/commands/plan-sdd.md` |
| `/execute-sdd` | Execution | `.claude/commands/execute-sdd.md` |
| `/mutate` | Mutation Hardening (optional) | `.claude/commands/mutate.md` |
| `/validate-sdd` | Validation | `.claude/commands/validate-sdd.md` |

## Operational Commands

| Command | Purpose | File |
|---------|---------|------|
| `/setup-local` | Bootstrap dev environment | `.claude/commands/configurations/setup-local.md` |
| `/dependencies-update` | Check & update deps | `.claude/commands/configurations/dependencies-update.md` |
| `/deploy` | Deploy to production | `.claude/commands/deploy.md` |
| `/git-commit-push` | Stage, commit, push | `.claude/commands/utility/git-commit-push.md` |
| `/init-harness` | Initialize harness scaffold for a new project | `.claude/commands/init-harness.md` |

## Discipline Commands (Expansion Slots)

Multi-discipline command subdirectories for future specialization. Place discipline-specific commands inside the matching subfolder.

| Subdirectory | Discipline | Purpose |
|-------------|-----------|---------|
| `.claude/commands/ba/` | Business Analysis | Requirements commands, stakeholder reporting |
| `.claude/commands/frontend/` | Frontend | UI scaffolding, accessibility checks, style tooling |
| `.claude/commands/backend/` | Backend | Backend command definitions, server config commands |
| `.claude/commands/qa/` | Quality Assurance | Test runners, regression suites, audit commands |
| `.claude/commands/data/` | Data | Analytics queries, structured data, Data Access Layer |
