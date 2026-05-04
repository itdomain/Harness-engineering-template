---
name: /plan-sdd
description: Generate atomic task plan from requirements.md + design.md
---

## /plan-sdd — Task Planning Phase

Orchestrates the task plan by delegating to the **Planner** agent.

### Prerequisites

- `specs/features/[category]/[feature-name]/requirements.md` (or `specs/bugs/[bug-id]/requirements.md`) must exist
- `specs/features/[category]/[feature-name]/design.md` (or `specs/bugs/[bug-id]/design.md`) must exist
- `CLAUDE.md` must be read for project standards

### Workflow

1. **Delegating to Planner agent**: Read `.claude/agents/planner.md` for the complete workflow. The agent handles:
   - Breaking the design into atomic tasks (< 1 hour each) with Definition of Done
   - Mapping dependencies between tasks
   - Inserting human review checkpoints at logical milestones
   - Generating test tasks (prefixed `TEST:`) for JS features before implementation tasks
2. **Report**: Forward the task breakdown. The user can now run `/execute-sdd`.

### Rules

- **Never** override the Planner agent's guardrails — see `.claude/agents/planner.md`.
- No code written here — planning only.
- Each task must reference specific file paths and line ranges where applicable.
- Tasks must follow project conventions (CLAUDE.md standards).
