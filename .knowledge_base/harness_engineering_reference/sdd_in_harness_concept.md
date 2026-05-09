# SDD in the Harness Architecture

## The Hybrid Nature of SDD

Spec-Driven Development (SDD) is **neither a pure sensor nor a pure guide**. It is a hybrid control mechanism that incorporates both feedforward and feedback elements across its lifecycle.

### SDD Phase Classification

| SDD Phase | Control Type | Mechanism | What It Does |
|-----------|-------------|-----------|--------------|
| `/start-sdd` **Pre-Check** (gate) | **Gate** (feedforward) | Reads decision framework, produces HALT / PROCEED / ESCALATE verdict | Halts trivial tasks; escalates business-impact changes to humans |
| `/start-sdd` (requirements) | **Guide** (feedforward) | Forces structured thinking before code | Makes scope explicit, prevents "sledgehammer for a nut" |
| `/design-sdd` (design) | **Guide** (feedforward) | Architecture decision log | Prevents architectural drift before implementation |
| `/plan-sdd` (task planning) | **Guide** (feedforward) | Atomic task breakdown with checkpoints | Structures work before execution |
| `/execute-sdd` (TDD RED-GREEN) | **Sensor** (feedback) | {{TEST_FRAMEWORK}} test execution | Detects behavioral deviations in real-time |
| `/mutate` (mutation hardening) | **Sensor** (feedback) | {{MUTATION_TOOL}} mutant detection | Detects test suite gaps (optional per-feature) |
| `/validate-sdd` (validation) | **Sensor** (feedback) | Requirements audit + QA checklist | Verifies output matches intent |

## SDD Limitations

### When SDD Works Well

| Condition | Why SDD Fits |
|-----------|-------------|
| Medium-sized features (2-5 files) | Scope is explicit enough for specs, small enough for one pipeline run |
| Well-specified requirements | Edge cases known, success metrics clear |
| Greenfield or isolated changes | No existing architecture to reconcile |
| JavaScript features | TDD RED-GREEN cycle has established infrastructure |

### When SDD Does Not Fit

| Condition | Problem | Better Approach |
|-----------|---------|----------------|
| **Small bugs** (< 2 files, < 30 lines) | "Sledgehammer for a nut" — spec overhead > implementation effort | Ad-hoc direct fix |
| **Highly ambiguous initiatives** | Current AI templates cannot support specialist analysis or stakeholder coordination | Exploratory research first |
| **Brownfield with architecture conflicts** | New specs may conflict with existing patterns | Architecture review + partial SDD |
| **Large features with low clarity** | Rigid specs + AI non-determinism = misalignment | Iterative research + phased implementation |
| **"Make it better" without specifics** | No requirements to spec | Human-led discovery |

### The Over-Specification Problem

The SDD workflow tends to generate "verbose, repetitive markdown", causing developers to "rather review code than all these markdown files". This is a signal that:

1. **Spec overhead grows with feature size** — for small features, the spec is 80% of the artifact
2. **AI sometimes ignores or over-follows specs** — non-determinism means specs are not guarantees
3. **Functional vs. technical separation is inconsistent** — requirements.md mixes business and technical concerns

### The Under-Specification Problem

Conversely, SDD fails when:
- Requirements are vague ("add a contact form") — specs force false precision
- Multiple valid approaches exist — specs pick one arbitrarily
- Stakeholder coordination is needed — AI cannot replace human alignment

## Problem Size Thresholds

| Size Category | File Count | Line Changes | Time Estimate | SDD Verdict |
|---------------|-----------|-------------|---------------|-------------|
| **Trivial** | 1 file | < 10 lines | < 5 minutes | **Never SDD** — direct fix |
| **Small** | 1-2 files | < 30 lines | < 30 minutes | **Never SDD** — ad-hoc |
| **Medium** | 2-5 files | < 100 lines | < 2 hours | **SDD full pipeline** |
| **Large** | 5-15 files | < 300 lines | < 1 day | **Phased SDD** — split into sub-features |
| **Massive** | > 15 files | > 300 lines | Multi-day | **Exploratory + partial SDD** |

## Clarity Thresholds

| Clarity Level | Indicators | SDD Verdict |
|---------------|-----------|-------------|
| **High** | Edge cases documented, success metrics defined, no ambiguity | **SDD full pipeline** |
| **Medium** | Core requirements clear, some edge cases unknown | **SDD with exploratory design** — add research step before `/plan-sdd` |
| **Low** | Goals stated but not specified, multiple valid approaches | **Exploratory research** — no SDD until clarity improves |
| **None** | "Improve X" without specifics | **Human-led discovery** — SDD not applicable |

## Decision Tree

```
Problem received
├── Is it < 2 files AND < 30 lines?
│   └── YES → Ad-hoc direct fix (no SDD)
│   └── NO → Continue
├── Are requirements well-specified?
│   └── NO → Exploratory research first
│   └── YES → Continue
├── Does it conflict with existing architecture?
│   └── YES → Add architecture review step
│   └── NO → Continue
├── Does it affect {{STAKEHOLDER_DOMAIN_EXAMPLES}}?
│   └── YES → Escalate to human for business impact
│   └── NO → Continue
└── Proceed with SDD full pipeline
    ├── /start-sdd (+ mutate opt-in) → /design-sdd → /plan-sdd → /execute-sdd → /mutate (optional) → /validate-sdd
```

## Where SDD Fits in the Harness

```
┌──────────────────────────────────────────────────────────┐
│                     Harness Architecture                 │
├──────────────────────────────────────────────────────────┤
│                                                          │
│  ┌───────────────┐    ┌──────────────┐    ┌───────────┐  │
│  │   Guides      │    │   SDD as     │    │  Sensors  │  │
│  │  (Feed-       │    │  Hybrid      │    │ (Feedback │  │
│  │   forward)    │    │  Gate        │    │  controls)│  │
│  ├────────────── ┤    ├──────────────┤    ├───────────┤  │
│  │ • Pre-flight  │    │ • Small →    │    │ • Tests   │  │
│  │ • Glossary    │    │   ad-hoc     │    │ • Mutation│  │
│  │ • Constraints │    │ • Clear →    │    │ • Domain  │  │
│  │ • SDD Guide ─ ┼──→ │   SDD        │    │   linter  │  │
│  │ • Architecture│    │ • Ambiguous  │    │ • Drift   │  │
│  │               │    │   → Research │    │   detect  │  │
│  └───────────────┘    │ • Brownfield │    │ • Health  │  │
│                       │   → Review   │    │   check   │  │
│                       └──────┬───────┘    └───────────┘  │
│                              │ ESCALATE                  │
│                              ▼                           │
│                     ┌────────┬─────────┐                 │
│                     │    Boundary      │                 │
│                     │  (Human-in-Loop) │                 │
│                     ├──────────────────┤                 │
│                     │ • Business       │                 │
│                     │   impact         │                 │
│                     │ • Ambiguous      │                 │
│                     │   requirements   │                 │
│                     └──────────────────┘                 │
│                                                          │
│   ┌──────────────────────────────────────────────────┐   │
│   │              Model + Code (Center)               │   │
│   └──────────────────────────────────────────────────┘   │
└──────────────────────────────────────────────────────────┘
```

**Key insight:** Guides and sensors are the outer layers. SDD sits *between* them as a decision gate — it is controlled by guides (when to use) and produces outputs that are controlled by sensors (validation).

## Mutation Testing as a Sensor for Test Suite Health

Mutation testing (`/mutate`) is the **most sensitive sensor** in the harness:

| Sensor | Sensitivity | Speed | What It Detects |
|--------|------------|-------|-----------------|
| {{TEST_FRAMEWORK}} tests | Medium | Fast | Failing assertions |
| Mutation testing ({{MUTATION_TOOL}}) | **High** | Slow | Test suite gaps (tests that pass but don't guard behavior) |
| Domain linter | Low | Fast | Terminology drift |
| Health check | Medium | Fast | Structural violations |
| Drift detection | Low | Slow | Gradual architectural decay |

**Mutation score interpretation:**

| Score | Meaning | Action |
|-------|---------|--------|
| >= 80% | **PASS** — test suite is sensitive | Proceed to validation |
| 60-79% | **WARN** — moderate gaps | Fix top survived mutants |
| < 60% | **FAIL** — major gaps | Block validation, write remediation tests |
| < 40% | **HARD FAIL** | Stop — do not proceed |

## Key Takeaway

SDD is most effective when:
1. **Guided** by a feedforward decision gate (is SDD appropriate?)
2. **Bounded** by problem size and clarity thresholds
3. **Validated** by feedback from sensors (tests, mutation, domain linter)

Without the feedforward and feedback gates, SDD is a blunt instrument. With them, SDD is a scalpel.

## References

- Birgitta Böckeler, "Harness engineering for coding agent users" (2026-04-02) - https://martinfowler.com/articles/harness-engineering.html
- Birgitta Böckeler, "Understanding Spec-Driven-Development" (2026-10-15) - https://martinfowler.com/articles/exploring-gen-ai/sdd-3-tools.html
- Kent Beck - "Test Driven Development: By Example" (TDD) (2002)
- Larry Smith, "Shift-Left Testing" (2001)
- DeMillo, R. A., Lipton, R. J., & Sayward, F. G. (1978-04-29). "Hints on Test Data Selection: Help for the Practicing Programmer". Page(s): 34 - 41. , Coupling Effect - https://doi.org/10.1109/C-M.1978.218136
