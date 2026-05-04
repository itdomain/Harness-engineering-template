# Validation Report: [FEATURE NAME]

> **Based on**: `specs/features/[category]/[feature-name]/requirements.md` (or `specs/bugs/[bug-id]/requirements.md`) + implemented code
> **Validated**: [DATE]
> **Status**: PASS / FAIL

---

## Requirements Audit

| Requirement | Met? | Notes |
|-------------|------|-------|
| [requirement text] | Yes / Partial / No | [explanation] |

## Success Metrics

| Metric | Target | Actual | Passed? |
|--------|--------|--------|---------|
| [metric] | [target] | [actual] | Yes / No |

## Tests Run

- [ ] Manual QA checklist
- [ ] Browser compatibility (Chrome, Firefox, Safari, Edge)
- [ ] Mobile responsiveness
- [ ] Accessibility audit (WCAG AA minimum)
- [ ] Performance ({{PERFORMANCE_METRICS}} if applicable)
- [ ] Security headers / input validation (if backend changes)

## Findings

_Bug or issue found: ..._

_No issues found._

## Verdict

**PASS** — All requirements met. Archive specs to `specs/archive/features/[category]/[feature-name]-<date>/` (bugs → `specs/archive/bugs/[bug-id]-<date>/`).

**FAIL** — See findings above. Return to `/plan-sdd` or `/execute-sdd` as needed.
