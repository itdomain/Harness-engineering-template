---
name: /design-sdd
description: Generate design.md from requirements.md — schema, data flow, architecture
---

## /design-sdd — Design Phase

Orchestrates the design specification by delegating to the **Architect** agent.

### Prerequisites

- `specs/features/[category]/[feature-name]/requirements.md` (or `specs/bugs/[bug-id]/requirements.md`) must exist
- `CLAUDE.md` must be read for project standards

### Workflow

1. **Delegating to Architect agent**: Read `.claude/agents/architect.md` for the complete workflow. The agent handles:
   - Reading requirements and cross-referencing against the proposed design
   - Producing `design.md` with schema/API changes, data flow (Mermaid), component architecture, Architecture Decision Log, and requirements traceability
   - Flagging gaps, ambiguities, or conflicts
2. **Report**: Forward the agent's result. The user can now run `/plan-sdd`.

### Rules

- **Never** override the Architect agent's guardrails — see `.claude/agents/architect.md`.
- Design must respect existing patterns (defer JS, CSS custom properties, no target="_blank", accessibility standards).
- No implementation code — design only.
