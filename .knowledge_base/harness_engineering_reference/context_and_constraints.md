# Context and Constraints

## Context Provision

The harness must provide the model with exactly the information it needs for the task at hand — no more, no less. Context is the foundation layer that determines what the model can possibly do.

### Progressive Context Loading

Not all context is equally relevant to every task. Effective harnesses load context progressively:
- **Always available:** project conventions, security rules, coding standards
- **Task-specific:** architecture details, section maps, component IDs
- **Phase-specific:** SDD templates, design patterns, test structures

### Current State vs. Target

**Current:** CLAUDE.md is loaded entirely for every interaction. This includes architecture details, palette, typography, section maps, security rules, and commit guidelines — all mixed together.

**Target:** Context is organized by concern and loaded progressively. The model gets architecture details when editing relevant source files, security rules when touching sensitive code, accessibility standards when modifying UI — without unnecessary noise.

## Constraints as Feedforward Controls

Constraints are the "guides" in Harnest Engineering terminology — anticipatory controls that prevent errors before they happen.

### Three Regulation Dimensions

1. **Internal code quality.** Naming conventions, formatting, code organization. Prevents technical debt accumulation.
2. **Architectural constraints.** Patterns to preserve, patterns to avoid, structural invariants. Prevents architectural drift.
3. **Functional correctness.** Does the code do what it should? Security, accessibility, performance. Prevents user-facing failures.

### Current State vs. Target

**Current (achieved):** Constraints are explicit, categorized, and organized as feedforward controls in `.claude/constraints_middle/`:
- `security-constraints.md` — security rules
- `llm-security-constraints.md` — LLM-specific security rules
- `architectural-constraints.md` — patterns to preserve/avoid
- `preflight.md` — checklist of constraints to verify before any change
- `DOMAIN_GLOSSARY.md` + `ubiquitous_language/` — ubiquitous language constraints

## Key Takeaway

Context tells the model what it can do. Constraints tell it what it must not do. Both are essential for reliable engineering output. The harness fails when constraints are implicit or when context is overloaded.

## Cross-References

- Guides (feedforward): `.knowledge_base/harness_engineering_reference/guides.md`
- Sensors (feedback): `.knowledge_base/harness_engineering_reference/sensors/INDEX.md`
- SDD position: `.knowledge_base/harness_engineering_reference/sdd_in_harness_concept.md`

## References

- Birgitta Böckeler, "Harness engineering for coding agent users" (2026-04-02) - https://martinfowler.com/articles/harness-engineering.html
- Melvin E. Conway - "Conway's Law" — organizations design systems mirror their own communication structure. System constraints reflect organizational constraints. (1967) https://www.melconway.com/Home/Conways_Law.html
