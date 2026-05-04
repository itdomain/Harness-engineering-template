# Agent Registry

Compact registry of all agents. Each agent file defines role, workflow, rules, inputs, and outputs.

All agents must consult `.claude/constraints_middle/DOMAIN_GLOSSARY.md` for ubiquitous language compliance.

## SDD Agents

| Agent | Phase | File |
|-------|-------|------|
| Architect | Design | `.claude/agents/architect.md` |
| Planner | Task Planning | `.claude/agents/planner.md` |
| TDD Builder | Test Generation | `.claude/agents/tdd-builder.md` |
| Builder | Execution | `.claude/agents/builder.md` |
| Mutator | Mutation Hardening (optional) | `.claude/agents/mutator.md` |
| Auditor | Validation | `.claude/agents/auditor.md` |

## Operational Agents

| Agent | Command | File |
|-------|---------|------|
| Local Setup | `/setup-local` | `.claude/agents/configurations/local-setup.md` |
| Dependency Updater | `/dependencies-update` | `.claude/agents/configurations/dependency-updater.md` |
| Deploy | `/deploy` | `.claude/agents/configurations/deploy.md` |

## Discipline Agents (Expansion Slots)

Multi-discipline agent subdirectories for future specialization. Place discipline-specific agents inside the matching subfolder.

| Subdirectory | Discipline | Purpose |
|-------------|-----------|---------|
| `.claude/agents/ba/` | Business Analysis | Requirements elicitation, stakeholder alignment, acceptance criteria |
| `.claude/agents/frontend/` | Frontend | UI/UX implementation, accessibility, CSS/JS patterns |
| `.claude/agents/backend/` | Backend | Backend agent definitions, server config, API contracts |
| `.claude/agents/qa/` | Quality Assurance | Test strategy, regression checks, acceptance testing |
| `.claude/agents/data/` | Data | Analytics, structured data, Data Access Layer |
