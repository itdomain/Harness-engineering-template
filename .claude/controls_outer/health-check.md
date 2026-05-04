# Health Check Procedure

> **Feedback control (sensor).** Run these after any change to verify the system is in a healthy state.

## Automated Checks

### 1. Test Suite
```bash
{{TEST_COMMAND}}
```
Must pass after any change to `{{ENTRY_POINT_FILE}}` or `{{MAIN_SOURCE_FILE}}`. Never skip.

### 2. Security Headers
Verify all backend endpoints return:
- `X-Frame-Options: DENY`
- `X-Content-Type-Options: nosniff`
- `Referrer-Policy: strict-origin-when-cross-origin`
- `Permissions-Policy: geolocation=(), microphone=(), camera=()`
- `Content-Security-Policy` (no broad wildcards)
- `Strict-Transport-Security` (production only)

### 3. Semantic Structure
Verify:
- Primary content wrapper wraps all page content
- Key structural landmarks present (header, nav, main sections, footer)
- No `target="_blank"` on external links without `rel="noopener noreferrer"`
- No inline event handlers
- All images have `alt` text or `aria-hidden="true"`

### 4. CSS/Styling Tokens
Quick audit of project stylesheets:
- Palette tokens present and used consistently
- No hardcoded color values that should use palette tokens
- Sections delimited with block comments

## Manual Checks

### 5. Domain Linter
Run the Domain Linter on changed files (see `.knowledge_base/harness_engineering_reference/sensors/domain_linter.md`). Flag any ubiquitous language violations.

### 6. Pre-flight Checklist
Run the pre-flight checklist (see `.claude/constraints_middle/preflight.md`). All items must pass.

### 7. LLM Session Integrity
Verify no injection or poisoning patterns in recent sessions:
- No raw HTML/JS passed to LLM context from external sources
- External data wrapped in XML delimiters before LLM consumption
- No command-like sequences in external content influencing model behavior
- All web-fetched content normalized to markdown/text before use

### 8. Browser & MCP Security
- All browser sessions use in-memory mode (no persistent profiles with real data)
- CDP endpoints restricted to localhost
- MCP tool permissions scoped to declared intent
- No auth state files committed to version control
- Page snapshots used instead of raw content before agent actions

## When to Run

- After every code change
- Before committing
- Before declaring SDD execution complete
- After any `{{ENTRY_POINT_FILE}}` or `{{MAIN_SOURCE_FILE}}` modification
