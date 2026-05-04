# Harness Engineering Template

A portable Claude Code harness scaffold implementing the Harness Engineering concept: a structured set of context (inner), constraints (middle), controls (outer), and human-in-the-loop boundaries that guide Claude across any codebase.

## What This Is

This template provides:
- **SDD workflow** (Spec-Driven Development): 6 slash commands for spec → design → plan → execute → validate
- **Ubiquitous language gates**: domain glossaries and naming enforcement
- **Feedforward controls**: pre-flight checklist, security constraints, architectural constraints
- **Feedback sensors**: health checks, drift detection, domain linter
- **Human-in-the-loop patterns**: escalation matrix, confirmation protocols
- **Knowledge base**: Harness Engineering concept documentation

## Prerequisites

- [Claude Code](https://claude.ai/code) installed and authenticated
- Git repository initialized in your project

## Installation

### Step 1 — Copy the harness into your project

```bash
# From your project root:
git clone https://github.com/YOUR_ORG/harness-template .harness-tmp
cp -r .harness-tmp/.claude .
cp -r .harness-tmp/.knowledge_base .
cp -r .harness-tmp/specs .
cp .harness-tmp/CLAUDE.md .
cp .harness-tmp/.claudeignore .
rm -rf .harness-tmp
```

> If your project already has a `CLAUDE.md` or `specs/` directory, merge manually. Do not overwrite blindly.

### Step 2 — Open project in Claude Code

```bash
claude .
```

### Step 3 — Run the init command

```
/init-harness
```

Claude will scan your codebase, ask 7 targeted questions, and customize all harness files for your project. The entire process takes 2–5 minutes.

## After Init

Files the init command cannot fill automatically (requires your domain knowledge):
- `.claude/context_inner/architecture.md` — Describe your system architecture
- `.claude/constraints_middle/DOMAIN_GLOSSARY.md` — Add your domain terms
- `.claude/constraints_middle/ubiquitous_language/` — Business, organizational, development glossaries
- `.claude/controls_outer/drift-detection.md` — Add project-specific drift checks
- `.claude/constraints_middle/architectural-constraints.md` — Add your preserve/avoid patterns

## SDD Workflow

Once initialized, use these slash commands:

| Command | Phase | Produces |
|---------|-------|---------|
| `/start-sdd feature [cat] [name]` | Requirements | `specs/features/[cat]/[name]/requirements.md` |
| `/design-sdd` | Design | `design.md` |
| `/plan-sdd` | Task Planning | `tasks.md` |
| `/execute-sdd` | Execution | tests + code + `history.log` |
| `/mutate` | Mutation Hardening (optional) | mutation report |
| `/validate-sdd` | Validation | `validation_report.md` → archive |

## Harness Layers

| Layer | Directory | Purpose |
|-------|-----------|---------|
| Inner | `.claude/context_inner/` | What the model knows (architecture, conventions) |
| Middle | `.claude/constraints_middle/` | Feedforward controls (pre-flight, security, glossary) |
| Outer | `.claude/controls_outer/` | Feedback sensors (health check, drift detection) |
| Boundary | `.claude/handoff_boundary/` | Human-in-the-loop (escalation, confirmation) |

## Expansion Slots

Empty directories ready for discipline-specific agents and commands:
- `.claude/agents/ba/` + `.claude/commands/ba/` — Business Analysis
- `.claude/agents/frontend/` + `.claude/commands/frontend/` — Frontend
- `.claude/agents/backend/` + `.claude/commands/backend/` — Backend
- `.claude/agents/qa/` + `.claude/commands/qa/` — QA
- `.claude/agents/data/` + `.claude/commands/data/` — Data

## See Also

- `INIT_GUIDE.md` — Detailed init walkthrough and troubleshooting
- `.knowledge_base/harness_engineering_reference/` — Harness Engineering concept documentation
- `.claude/COMMANDS.md` — Full command registry
- `.claude/AGENTS.md` — Full agent registry
