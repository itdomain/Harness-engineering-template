# The Harness Concept

## Definition

The harness is all components surrounding the base model that enable it to perform reliably in complex environments.

**Formula:** `Agent = Model + Harness`

The harness is not an enhancement — it is the enabling layer that transforms raw model capability into dependable engineering output.

## Role

The harness anticipates errors and enables automatic correction. It is engineered, not accidental: built deliberately to compensate for the model's known failure modes and to amplify its strengths.

## Key Principles

1. **Adaptability depends on environmental traits.** A harness that works well with a strongly-typed language with clear module boundaries will differ from one for a dynamically-typed legacy codebase. New projects are inherently more flexible than legacy systems.

2. **Ashby's Law of Requisite Variety.** The harness must match the complexity of the system it regulates. A simple harness cannot control a complex system.

3. **Layered architecture.** The harness forms concentric layers around the model:
   - Inner layer: immediate context (what the model needs to know)
   - Middle layer: constraints and guides (what the model must not do)
   - Outer layer: sensors and feedback (how the model learns from outcomes)
   - Boundary layer: human collaboration (when the model asks for help)

## Implications for This Project

The current CLAUDE.md + .claude/ structure is a single-layer harness. It works because the project is small, but it lacks the layered defense that Fowler describes. The refactor moves from a single context file to concentric context layers, where each layer has a distinct role.

## References

- Birgitta Böckeler, "Harness engineering for coding agent users" (2026-04-02) - https://martinfowler.com/articles/harness-engineering.html
- William Ross Ashby, "An Introduction to Cybernetics" (1956) — Law of Requisite Variety - https://en.wikipedia.org/wiki/An_Introduction_to_Cybernetics#Law_of_requisite_variety
