# Human-in-the-Loop Patterns

## Core Principle

Developers "steer" the system by iteratively refining controls whenever recurring issues emerge. Human evaluation serves as a supplementary sensor — not the primary verification mechanism, but essential for high-value decisions where probabilistic verification is insufficient.

The objective is not to remove the human, but to "direct it to where our input is most important" — preserving human oversight for decisions that genuinely require judgment.

## Hand-off Patterns

### Auto-Proceed (No Human Intervention)

The harness can handle these autonomously:
- Style changes (CSS updates, color swaps, typography tweaks)
- Bug fixes that match a documented spec exactly
- Test updates (fixing flaky tests, updating expectations)
- Documentation updates (typos, clarifications, translations)
- Dependency version bumps with no breaking changes

### Human Review (Confirmation Required)

The model should pause and confirm for:
- Architectural changes (new files, new directories, structural modifications)
- New dependencies (npm install, composer require)
- Security-related changes (headers, CSRF, validation rules)
- External link modifications (URL changes, link targets)
- API changes ({{PUBLIC_HANDLER_FILE}}, {{PRIVATE_HANDLER_FILE}}, {{ENV_FILE}})

### Escalation (Human Leads)

The model should defer to the human for:
- Production deployment
- Secret/key management changes
- Breaking API changes
- Database schema changes
- Changes that affect multiple independent sections
- Decisions with business impact ({{STAKEHOLDER_DOMAIN_EXAMPLES}})

## Escalation Criteria

Use this matrix to decide when to escalate:

| Signal | Action |
|--------|--------|
| Change matches documented spec exactly | Auto-proceed |
| Change deviates from spec | Pause and explain |
| Change affects security surface | Human review |
| Change introduces new dependency | Human review |
| Change is ambiguous or has multiple valid approaches | Escalate |
| Change has business impact | Escalate |
| 3+ recurring issues in same area | Refine harness controls |

## Key Takeaway

The human is not a bottleneck — the human is the steering wheel. The harness handles the routine so the human can focus on judgment calls. Good hand-off patterns keep the human informed without bogging them down in verification of things the harness can handle.

## References

- Birgitta Böckeler, "Harness engineering for coding agent users" (2026-04-02) - https://martinfowler.com/articles/harness-engineering.html
- Donald Arthur Norman, "The Design of Everyday Things" - human as a "steering mechanism" (Revised Edition, 2013 . Chapter 2: The Psychology of Everyday Actions -> The Seven Stages Of Action) https://en.wikipedia.org/wiki/The_Design_of_Everyday_Things#Seven_stages_of_action
