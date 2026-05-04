# Domain Linter — Ubiquitous Language Validation Task

> **Purpose:** A verification loop task that checks code diffs against the Domain Glossary to flag terms that deviate from the Ubiquitous Language.

## What It Does

The Domain Linter scans changed files for terminology that should be glossary terms but uses incorrect or non-canonical alternatives. It is a **sensor** (feedback control) in the Harness Engineering framework.

## How It Works

### Input
- A set of changed files (from `git diff --name-only` or similar)
- The Domain Glossary files:
  - `.claude/constraints_middle/DOMAIN_GLOSSARY.md`
  - `.claude/constraints_middle/ubiquitous_language/business/GLOSSARY.md`
  - `.claude/constraints_middle/ubiquitous_language/organizational/GLOSSARY.md`
  - `.claude/constraints_middle/ubiquitous_language/development/GLOSSARY.md`

### Process

1. **Extract all glossary terms** from the four glossary files.
2. **Build a mapping** of: glossary term → common incorrect/variant forms.
3. **Scan changed files** for occurrences of incorrect forms.
4. **Report violations** with file path, line number, incorrect term, and suggested correction.

### Output Format

```
Domain Linter Report
====================

Violation 1:
  File: {{MAIN_SOURCE_FILE}}:{{LINE_NUMBER}}
  Incorrect: "{{DOMAIN_TERM_WRONG_EXAMPLE}}"
  Suggested: "{{DOMAIN_TERM_EXAMPLE}}"
  Context: const varName = ...

Violation 2:
  File: {{ENTRY_POINT_FILE}}:{{LINE_NUMBER}}
  Incorrect: "{{DOMAIN_TERM_WRONG_EXAMPLE}}"
  Suggested: "{{DOMAIN_TERM_EXAMPLE}}" (or approved variant)
  Context: <h2>{{DOMAIN_TERM_WRONG_EXAMPLE}}</h2>

Summary: N violations found across M files.
```

## When to Run

- **Before committing** — run on staged files
- **After SDD execution** — run on all changed files
- **During code review** — run on PR diff
- **Periodically** — run on entire codebase for drift detection

## Limitations

- **False positives:** Some generic terms may appear in valid non-domain contexts. The linter flags these for review, not automatic correction.
- **JS data keys:** JavaScript internal keys (e.g., `{{SERVICE_BUNDLE_KEY_1}}`, `{{SERVICE_BUNDLE_KEY_2}}`) are glossary-approved and should not be flagged.
- **CSS classes:** Class names follow CSS conventions (kebab-case) and may differ from glossary terms. They are not flagged.
- **HTML IDs:** Same as CSS classes — not flagged.

## Integration with Harness

This linter is a **feedback control** (sensor) in the harness verification loop:

1. **Pre-flight (guide):** Agent consults glossary before writing code.
2. **Post-change (sensor):** Domain Linter runs on changed files.
3. **Drift detection (sensor):** Periodic full-codebase scan.

## References

- Eric Evans, "Domain-Driven Design: Tackling Complexity in the Heart of Software" (2003-04-15), Chapter 2: Communication and the Use of Language
- Birgitta Böckeler, "Harness engineering for coding agent users" (2026-04-02) - https://martinfowler.com/articles/harness-engineering.html
