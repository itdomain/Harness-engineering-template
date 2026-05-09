# Guides (Feedforward Controls)

## Definition

Guides are **feedforward controls** — anticipatory mechanisms that prevent errors before they happen. Unlike sensors (which detect failures after the fact), guides shape behavior proactively by constraining the solution space the agent considers.

**Formula:** `Guides = Constraints + Conventions + Decision Logic`

A guide answers: "Before you do X, check Y first." A sensor answers: "After you did X, verify Y passed."

## Role

Guides shift verification left — they catch problems at the point of decision, not after code is written. Effective guides are:

1. **Specific** — not "write good code" but "use `{{SECURE_COMPARE_FUNCTION}}()` for CSRF validation"
2. **Actionable** — the agent can follow the guide without external judgment
3. **Testable** — you can verify the guide was consulted (e.g., via a preflight checklist)

## Three Regulation Dimensions

Guides operate across three dimensions, each preventing a class of errors:

### 1. Internal Code Quality

Naming conventions, formatting, code organization. Prevents technical debt accumulation.

**Examples in this project:**
- CSS custom property usage (`--bg`, `--accent`, etc.) instead of hardcoded colors
- JS loaded with `defer` attribute
- No `target="_blank"` on external links
- No inline event handlers
- IIFE pattern for module encapsulation
- Event delegation for DOM event handling

### 2. Architectural Constraints

Patterns to preserve, patterns to avoid, structural invariants. Prevents architectural drift.


### 3. Functional Correctness

Does the code do what it should? Security, accessibility, performance. Prevents user-facing failures.


## Resource Management (Opt-in Gates)

Decisions about which sensors to enable per-feature. Prevents wasting computational resources on low-value feedback loops.

**Concept**: A feedforward guide (opt-in decision) gates a feedback sensor (mutation testing). The decision is made before the sensor runs, at the point of feature initialization.

**Mechanism**: A state file created at SDD start records whether the sensor is enabled. The sensor checks this file before executing; if disabled, it logs a skip and proceeds.

**When to enable**: Core business logic, public-facing JS, security-sensitive code, critical user journeys. **When to skip**: Minor UI tweaks, CSS-only changes, documentation updates, trivial bugfixes.

**Implementation recommendation**: Prefer a **computational** (deterministic and fast) opt-in check over a semantic check with an LLM. This avoids non-determinism and keeps the gate reliable.

## Pre-flight Checklist as a Guide

The pre-flight checklist (`.claude/constraints_middle/preflight.md`) is the primary operational guide. It runs before every change:

```
## Pre-flight Checklist

- [ ] Naming: Consult glossary, use canonical terms
- [ ] Security: No `target="_blank"`, no inline handlers, CSRF present
- [ ] Security: Security headers intact, no `X-XSS-Protection: 0`
- [ ] Security: No `.innerHTML` passthrough, no hardcoded secrets
- [ ] Accessibility: Focus indicators, alt text, form labels, ARIA
- [ ] Code Quality: CSS custom properties, JS defer, no render-blocking
- [ ] Testing: `{{TEST_COMMAND}} {{TEST_FILE_PATTERN}}` passes
- [ ] Content: CTA text matches glossary, section labels match glossary
```

Each checkbox is a guide — it forces the agent to verify a constraint before proceeding.

## Glossary Terms as Guides

Every term in the bounded-context glossaries is a **Guide violation detector**. When an agent uses a non-canonical term, it signals:

1. The glossary was not consulted (guide not followed)
2. The term may cause inconsistency across the codebase
3. A domain linter sensor should flag it post-change

**How glossaries act as guides:**
- `.claude/constraints_middle/DOMAIN_GLOSSARY.md` — global terms (company name, taglines, CTAs)
- `.claude/constraints_middle/ubiquitous_language/business/GLOSSARY.md` — service terminology
- `.claude/constraints_middle/ubiquitous_language/organizational/GLOSSARY.md` — team/role terminology
- `.claude/constraints_middle/ubiquitous_language/development/GLOSSARY.md` — SDD workflow, agent roles, technical terms

**Example guide violations:**
| Incorrect | Correct | Source |
|-----------|---------|--------|
| `{{DOMAIN_TERM_WRONG_EXAMPLE}}` | `{{DOMAIN_TERM_EXAMPLE}}` | business glossary |
| Non-canonical synonym | Canonical term | domain glossary |
| Lowercase persona | Capitalized persona | organizational glossary |
| "Specifications Driven Development" | "SDD" | development glossary |

## SDD Decision Guide

Before invoking `/start-sdd`, evaluate the problem against these criteria:

### Problem Size

| Size | Criteria | Action |
|------|----------|--------|
| **Small** | < 2 files, < 30 lines of change | Ad-hoc (no SDD). Direct fix. |
| **Medium** | 2-5 files, clear scope, < 1 hour | SDD full pipeline. |
| **Large** | > 5 files, multiple sections | SDD with phased execution. |
| **Massive** | Architecture-level redesign | Iterative research + partial SDD. |

### Problem Clarity

| Clarity | Criteria | Action |
|---------|----------|--------|
| **High** | Requirements well-specified, edge cases known | SDD full pipeline. |
| **Medium** | Requirements mostly clear, some ambiguity | SDD with exploratory design phase. |
| **Low** | Vague goals, multiple valid approaches | Exploratory research first. No SDD. |
| **None** | "Make it better" without specifics | Human-led discovery. |

### Brownfield Impact

| Impact | Criteria | Action |
|--------|----------|--------|
| **None** | Greenfield or isolated change | Standard SDD. |
| **Low** | Affects one section | Standard SDD. |
| **Medium** | Conflicts with existing patterns | Add architecture review step. |
| **High** | Multiple independent sections affected | Escalate to human for coordination. |

### Decision Matrix

| Size | Clarity | Action |
|------|---------|--------|
| Small | Any | **Ad-hoc** — direct implementation |
| Medium | High | **SDD full pipeline** — /start-sdd → /validate-sdd |
| Medium | Medium | **SDD with exploratory design** — add research step |
| Large | High | **Phased SDD** — split into sub-features |
| Large | Medium | **Phased SDD + architecture review** |
| Any | Low/None | **Exploratory research** — no SDD |

## Architectural Constraints as Guides

The architectural constraints document (`.claude/constraints_middle/architectural-constraints.md`) is a guide catalog:

### Preserve These Patterns (Positive Guides)

| Pattern | Why | How to verify |
|---------|-----|---------------|
| Single-page, no build step | Project identity | No `package.json` build scripts |
| CSS custom properties | Palette consistency | Grep for `--bg`, `--accent` usage |
| `defer` on JS | Performance | HTML `<script defer>` attribute |
| IIFE + event delegation | Module safety | No global namespace pollution |
| {{PUBLIC_HANDLER_FILE}} thin stub | Separation of concerns | {{PUBLIC_HANDLER_FILE}} delegates to handler |

### Avoid These Patterns (Negative Guides)

| Anti-pattern | Why | How to detect |
|-------------|-----|---------------|
| `target="_blank"` | Accessibility + security | HTML grep |
| Inline event handlers | CSP violation | HTML grep |
| `.innerHTML` for user data | DOM XSS | JS grep |
| Hardcoded secrets | Credential leakage | Git grep, env audit |
| Broad CSP wildcards | Security degradation | Header audit |
| Weakened test assertions | False positives | Test file review |

## Key Takeaway

Guides prevent errors at the point of decision. They are most effective when:
- **Specific** (not "be secure" but "use `{{SECURE_COMPARE_FUNCTION}}()`")
- **Actionable** (the agent can follow without judgment)
- **Testable** (you can verify compliance via sensors)

A harness without guides is reactive — it waits for failures. A harness with guides is proactive — it shapes behavior before mistakes happen.

## References

- Birgitta Böckeler, "Harness engineering for coding agent users" (2026-04-02) - https://martinfowler.com/articles/harness-engineering.html
- Melvin E. Conway - "Conway's Law" — organizations design systems mirror their own communication structure. System constraints reflect organizational constraints. (1967) https://www.melconway.com/Home/Conways_Law.html
- Eric Evans, "Domain-Driven Design: Tackling Complexity in the Heart of Software" (2003-04-15), Chapter 2: Communication and the Use of Language
