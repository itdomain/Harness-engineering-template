# Pre-flight Checklist

> **Feedforward control.** Verify these constraints before making any change. This is the first gate — if any item fails, stop and fix before proceeding.

## Checklist

### Naming
- [ ] Consult `.claude/constraints_middle/DOMAIN_GLOSSARY.md` before naming anything
- [ ] Use canonical glossary terms — never invent synonyms or abbreviations
- [ ] Function/variable/class names prefer glossary terms over generic names

### Security
- [ ] No `target="_blank"` on external links
- [ ] No inline event handlers (`onclick`, `onload`, etc.)
- [ ] CSRF protection present on all POST endpoints
- [ ] Security headers intact (X-Frame-Options, CSP, Referrer-Policy, Permissions-Policy)
- [ ] No `X-XSS-Protection: 0` added
- [ ] No `.innerHTML` passthrough for user data (use `.textContent` or {{SHARED_UTIL_FUNCTION}})

### Accessibility
- [ ] All interactive elements have visible focus indicators (minimum 3px outline, sufficient contrast)
- [ ] All images have appropriate `alt` text or `aria-hidden="true"`
- [ ] Form fields have associated labels
- [ ] ARIA attributes used correctly (`aria-expanded`, `aria-live`, `role`)

### Code Quality
- [ ] CSS uses custom properties (`--bg`, `--accent`, etc.) from palette
- [ ] JavaScript loaded with `defer`
- [ ] No render-blocking JavaScript
- [ ] External font links include SRI and `crossorigin="anonymous"`

### LLM Security
- [ ] External data ingestion uses XML delimiter wrapping (`<external_source_content>`)
- [ ] No raw HTML/JavaScript passed directly into LLM context
- [ ] Instruction isolation directive prepended to prompts with external content
- [ ] Source verification performed for multi-source data updates
- [ ] No sub-agent prompts contain unescaped user input
- [ ] Browser sessions use in-memory mode (`--headless`, no `--persistent`)
- [ ] CDP endpoints restricted to `localhost` only
- [ ] MCP tool permissions scoped to narrow intent (least privilege)

### Testing
- [ ] `{{TEST_COMMAND}}` passes (for HTML/JS changes)
- [ ] Test assertions never modified or weakened

### SDD Decision Guide

Before invoking `/start-sdd`, evaluate:

- [ ] **Problem size:** Is this small enough for ad-hoc? (< 2 files, < 30 lines of change → use ad-hoc, no SDD)
- [ ] **Problem clarity:** Are requirements well-specified? (If ambiguous → exploratory research first)
- [ ] **Brownfield impact:** Does this conflict with existing architecture? (If yes → add architecture review step)
- [ ] **Business impact:** Does this affect pricing, CTAs, or content? (If yes → escalate to human)

**Decision matrix:**
- Small + any clarity → **Ad-hoc** (no SDD)
- Medium + high clarity → **SDD full pipeline**
- Medium + medium clarity → **SDD with exploratory design**
- Large + low clarity → **Exploratory research** (no SDD)
- See `.knowledge_base/harness_engineering_reference/sdd_in_harness_concept.md` for full decision tree

### Mutation Score Gate (Optional — Feedback Sensor)

Mutation testing is **enabled per-feature** at SDD start (`/start-sdd`). This gate only applies if `mutate.md` exists with `enabled: true`.

**Decision guide** — when to enable mutation testing:
- **Enable** (`enabled: true`): Features touching core business logic, public-facing JS, security-sensitive code, or critical user journeys
- **Skip** (`enabled: false`): Minor UI tweaks, CSS-only changes, documentation updates, trivial bugfixes (< 2 files)

- [ ] (If `mutate.md` has `enabled: true`) Mutation score >= 60% (or >= 80% for core business logic)
- [ ] (If `mutate.md` has `enabled: true`) Survived mutants documented with remediation plan
- [ ] (If `mutate.md` has `enabled: true`) No test assertions weakened to improve score
- See `.knowledge_base/harness_engineering_reference/sensors/mutation_testing.md`

### Content
- [ ] CTA text matches glossary canonical names
- [ ] Section labels match glossary terms
- [ ] No typos in domain terms
