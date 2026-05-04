---
name: /validate-sdd
description: Validate implemented feature against requirements — produce validation report
---

## /validate-sdd — Validation Phase

Orchestrates validation by delegating to the **Auditor** agent.

### Prerequisites

- `specs/features/[category]/[feature-name]/requirements.md` (or `specs/bugs/[bug-id]/requirements.md`) must exist
- `specs/features/[category]/[feature-name]/design.md` (or `specs/bugs/[bug-id]/design.md`) must exist
- `specs/features/[category]/[feature-name]/tasks.md` (or `specs/bugs/[bug-id]/tasks.md`) must exist
- `CLAUDE.md` must be read for project standards

### Workflow

1. **Delegating to Auditor agent**: Read `.claude/agents/auditor.md` for the complete workflow. The agent handles:
   - Requirements audit (Met / Partial / Not Met per requirement)
   - Success metrics verification
   - QA checklist: manual QA, browser compatibility, mobile responsiveness, accessibility, performance, security, code quality
   - Generating `validation_report.md`
   - Archiving on PASS or generating `fix_request.md` on FAIL
2. **Report**: Forward the agent's verdict and next steps.

### Rules

- **Never** override the Auditor agent's guardrails — see `.claude/agents/auditor.md`.
- Be thorough. A failed validation is better than a shipped bug.
- Do not mark PASS unless all "Met" requirements are fully implemented.
