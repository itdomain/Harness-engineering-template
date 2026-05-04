# Sensors (Feedback Controls) — Index

## Definition

Sensors are **feedback controls** — mechanisms that observe outcomes and detect when something goes wrong. Unlike guides (which prevent errors before they happen), sensors catch failures after the fact and trigger correction.


## Sensor Registry

| Sensor | File | Control Type | What It Detects |
|--------|------|-------------|-----------------|
| **Mutation Testing** | `mutation_testing.md` | Computational | Test suite gaps — mutants that survive indicate weak tests (optional per-feature) |
| **Domain Linter** | `domain_linter.md` | Inferential | Terminology drift — non-canonical terms in changed files |
| **Health Check** | `.claude/controls_outer/health-check.md` | Computational + Inferential | Structural violations after code changes |
| **Drift Detection** | `.claude/controls_outer/drift-detection.md` | Computational + Inferential | Gradual architectural decay over time |

## When Sensors Run

| Timing | Sensors Active |
|--------|---------------|
| **Pre-change** | None (guides only) |
| **During change** | Health check (automated) |
| **Post-change** | Mutation testing (if enabled), domain linter, health check |
| **Post-integration** | Drift detection, full audit |
| **Continuous** | Dependency vulnerability scans, palette drift |

## Computational vs. Inferential

| Category | Characteristics | Sensors |
|----------|----------------|---------|
| **Computational** | Deterministic, rapid, reliable | Mutation testing, health check |
| **Inferential** | Semantic, probabilistic, costly | Domain linter, drift detection |

Effective harnesses use computational verification for the common case and inferential verification for edge cases.

## References

- See `../verification_loops.md` for execution categories and timing strategy
- See `../guides.md` for the complementary feedforward controls
