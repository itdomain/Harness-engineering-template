# Conventions

## Communication

{{COMMUNICATION_STYLE}}

> **Init note**: `/init-harness` will ask whether to enable caveman mode. Leave this placeholder until then, or set manually: add `Use caveman mode (full intensity) for every response` to enable.

## Ubiquitous Language

Always consult `.claude/constraints_middle/DOMAIN_GLOSSARY.md` before naming anything. Use the exact canonical term from the glossary — never invent synonyms or abbreviations. If a concept has no glossary entry, add one to the appropriate bounded-context glossary before proceeding.

See `.claude/constraints_middle/CONSULT_THE_GLOSSARY.md` for detailed instructions.

## Coding Best Practices

> Document project-specific coding conventions here. Examples: link behavior rules, error handling patterns, API response conventions, naming rules beyond the glossary.

## Accessibility Standards

> **AAA-level accessibility is the baseline.** All interactive elements must have visible focus indicators (minimum 3px outline with sufficient contrast). All images must have appropriate `alt` text or be marked decorative with `aria-hidden="true"`.

## Commit Message Guidelines

All commit messages MUST follow this format for traceability of code origins:

```
<type>(<scope>): <subject>

Co-Generated with <Model Name>
```

**Git Rules:**
- **Never commit `.gitignore`** — it is a local configuration file and must be excluded from all commits.
- **Always mention current LLM Model Name used in the commit message**
- **Attribution:** Use the `Co-Generated with <Model Name>` line — do NOT use `Co-Authored-By` trailers.
- **Type:** `feat`, `fix`, `docs`, `refactor`, `style`, `chore`, `test`
- **Scope:** Short descriptor — e.g. `sdd`, `ui`, `agent`, `svg`, `template`
- **Subject:** Imperative mood, lowercase, no period — e.g. `update SDD folder structure`

**Example:**
```
feat(sdd): update SDD folder structure and bugfix support

Co-Generated with {{AI_MODEL}}
```
