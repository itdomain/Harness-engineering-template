# Harness Engineering Reference — Index

> **Purpose:** Design rationale, conceptual framework, and engineering principles behind this harness. Read to understand *why* the harness is structured the way it is. For operational use (constraints, guides, sensors), see `.claude/`.

## Files

| File | What It Contains |
|------|-----------------|
| `the_harness_concept.md` | The 4-layer concentric architecture (Inner/Middle/Outer/Boundary), Ashby's Law, agent = model + harness |
| `context_and_constraints.md` | Progressive context loading, constraints as feedforward controls, current vs. target state |
| `guides.md` | Guide framework: feedforward controls, 4 regulation dimensions, SDD Decision Guide, glossary as guides |
| `verification_loops.md` | Two control mechanisms (guides + sensors), computational vs. inferential, shift-left timing |
| `sdd_in_harness_concept.md` | SDD as hybrid control gate: where SDD fits in the harness, problem size/clarity thresholds, decision tree, mutation scoring |
| `human_in_the_loop.md` | Boundary layer: escalation matrix, confirmation patterns, human-in-the-loop triggers |
| `tooling_and_interfaces.md` | Tool inventory, MCP permissions, CLI interfaces |
| `sensors/INDEX.md` | Sensor registry: mutation testing, domain linter, health check, drift detection |
| `sensors/mutation_testing.md` | Mutation testing via Stryker: how it works, thresholds, remediation |
| `sensors/domain_linter.md` | Domain linter: ubiquitous language validation, how to run, limitations |

## Layer Mapping

| Harness Layer | Directory | This Reference |
|--------------|-----------|---------------|
| Inner (knowledge) | `.claude/context_inner/` | `context_and_constraints.md` |
| Middle (guides) | `.claude/constraints_middle/` | `guides.md` |
| Outer (sensors) | `.claude/controls_outer/` | `verification_loops.md`, `sensors/` |
| Boundary (human) | `.claude/handoff_boundary/` | `human_in_the_loop.md` |

## References

- Birgitta Böckeler, "Harness engineering for coding agent users" (2026-04-02) — https://martinfowler.com/articles/harness-engineering.html
- W. Ross Ashby, "An Introduction to Cybernetics" (1956) — Law of Requisite Variety
