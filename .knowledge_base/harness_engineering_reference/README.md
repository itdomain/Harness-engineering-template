# Harness Engineering Reference

Conceptual documentation for the Harness Engineering approach used in this Claude Code setup. Read this to understand *why* the harness is structured the way it is — the `.claude/` directory is where you *operate* it.

## What Is Harness Engineering?

A harness wraps an AI coding agent with structured context, constraints, and controls so the agent behaves reliably across a codebase over time. The term comes from Birgitta Böckeler's framing: a harness is what separates a capable model from a dependable agent.

The core insight: **agent = model + harness**. The model provides intelligence. The harness provides memory, judgment boundaries, and consistency. Without the harness, each session starts from scratch and drifts. With it, the agent accumulates reliable patterns.

## The Four-Layer Architecture

```
┌─────────────────────────────────────────────┐
│  BOUNDARY  .claude/handoff_boundary/        │  ← Human-in-the-loop
│  ┌─────────────────────────────────────┐    │
│  │  OUTER  .claude/controls_outer/     │    │  ← Feedback sensors
│  │  ┌───────────────────────────────┐  │    │
│  │  │ MIDDLE .claude/constraints_   │  │    │  ← Feedforward guides
│  │  │        middle/                │  │    │
│  │  │  ┌─────────────────────────┐  │  │    │
│  │  │  │ INNER .claude/context_  │  │  │    │  ← What the model knows
│  │  │  │       inner/            │  │  │    │
│  │  │  └─────────────────────────┘  │  │    │
│  │  └───────────────────────────────┘  │    │
│  └─────────────────────────────────────┘    │
└─────────────────────────────────────────────┘
```

| Layer | Directory | Role | Timing |
|-------|-----------|------|--------|
| **Inner** | `.claude/context_inner/` | Architecture, conventions, governance — what the model needs to know | Loaded every session |
| **Middle** | `.claude/constraints_middle/` | Pre-flight checklist, security, glossary, architectural rules — guides before action | Feedforward (before changes) |
| **Outer** | `.claude/controls_outer/` | Health check, drift detection — sensors after action | Feedback (after changes) |
| **Boundary** | `.claude/handoff_boundary/` | Escalation matrix, confirmation patterns — when to stop and ask a human | Escalation triggers |

## How This Reference Is Organized

```
.knowledge_base/harness_engineering_reference/
├── README.md                    ← You are here
├── INDEX.md                     ← File registry with one-line descriptions
├── the_harness_concept.md       ← Core definition, Ashby's Law, 4-layer model
├── context_and_constraints.md   ← Progressive context loading, feedforward controls
├── guides.md                    ← Guide framework: 4 regulation dimensions, SDD Decision Guide
├── verification_loops.md        ← Guides vs. sensors, shift-left timing strategy
├── sdd_in_harness_concept.md    ← Where SDD fits, problem thresholds, mutation scoring
├── human_in_the_loop.md         ← Boundary layer: escalation, confirmation patterns
├── tooling_and_interfaces.md    ← Tool inventory, MCP integration, permission model
└── sensors/
    ├── INDEX.md                 ← Sensor registry
    ├── mutation_testing.md      ← Mutation testing: thresholds, remediation, tool comparison
    └── domain_linter.md         ← Domain linter: ubiquitous language validation
```

## Reading Order

**To understand the concept from scratch:**
1. `the_harness_concept.md` — foundation
2. `context_and_constraints.md` — how context and constraints work together
3. `guides.md` — feedforward controls in depth
4. `verification_loops.md` — feedback controls and timing
5. `sdd_in_harness_concept.md` — where SDD fits in the harness

**To understand a specific layer:**
- Inner layer → `context_and_constraints.md`
- Middle layer → `guides.md`
- Outer layer → `verification_loops.md` + `sensors/`
- Boundary layer → `human_in_the_loop.md`

**To understand the sensors:**
- `sensors/INDEX.md` first, then individual sensor files

## Relationship to `.claude/`

This reference documents the *design rationale*. The `.claude/` directory contains the *operational implementation*.

| This reference | `.claude/` equivalent |
|----------------|-----------------------|
| `the_harness_concept.md` | `CLAUDE.md` (entry point) |
| `context_and_constraints.md` | `context_inner/architecture.md`, `context_inner/conventions.md` |
| `guides.md` | `constraints_middle/preflight.md`, `constraints_middle/architectural-constraints.md` |
| `verification_loops.md` | `controls_outer/health-check.md`, `controls_outer/drift-detection.md` |
| `human_in_the_loop.md` | `handoff_boundary/escalation-matrix.md`, `handoff_boundary/confirmation-patterns.md` |
| `sensors/mutation_testing.md` | `agents/mutator.md`, `commands/mutate.md` |
| `sensors/domain_linter.md` | `constraints_middle/DOMAIN_GLOSSARY.md`, `constraints_middle/CONSULT_THE_GLOSSARY.md` |

## Key Principles

**Ashby's Law of Requisite Variety**: A controller must have at least as much variety (possible responses) as the system it controls. For an AI agent, this means the harness must anticipate and constrain the full range of behaviors the model might exhibit — not just the happy path.

**Feedforward before feedback**: The middle layer (guides) acts *before* changes happen. The outer layer (sensors) acts *after*. Both are necessary; neither alone is sufficient.

**SDD as a hybrid control gate**: Spec-Driven Development is not just a workflow — it is a control mechanism. Requirements → design → plan → execute → validate is a structured loop that limits the blast radius of any single AI decision.

**Human-in-the-loop is not a failure mode**: The boundary layer exists because some decisions require human judgment regardless of AI capability. Escalation is the correct response to ambiguity, irreversibility, and stakeholder impact — not a workaround.

## References

- Birgitta Böckeler, *Harness engineering for coding agent users* (2026-04-02) — https://martinfowler.com/articles/harness-engineering.html
- W. Ross Ashby, *An Introduction to Cybernetics* (1956) — Law of Requisite Variety
