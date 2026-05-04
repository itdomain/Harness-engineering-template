# Consult the Glossary — Agent Instructions

> **Purpose:** This file teaches the AI agent how to consult and use the Domain Glossary before suggesting any refactors, writing any code, or generating any specs.

## The Ubiquitous Language Rule

> Change in the language is change to the model. (refactor the code, renaming classes, methods and modules to conform to the new model)

The glossary is not documentation. It is the authoritative vocabulary for the entire project. Every term in the glossary is the **only** valid name for its concept. Synonyms, abbreviations, and invented terms are forbidden.

## When to Consult the Glossary

**Always consult the glossary before:**

1. **Writing code** — Function names, variable names, class names should prefer glossary terms over technical or generic names.
2. **Writing specs** — requirements.md, design.md, tasks.md must use glossary terms exclusively.
3. **Writing documentation** — All doc comments, READMEs, inline comments must use glossary terms.
4. **Suggesting refactors** — Renames must use glossary terms as the target name.
5. **Reviewing code** — Flag any deviation from glossary terms as a "ubiquitous language violation."
6. **Generating test names** — Test suite and test case names must use glossary terms.

## How to Consult the Glossary

### Step 1: Identify the Bounded Context

| If working on... | Consult... |
|-----------------|-----------|
| Services, offerings, design centers, bundles | `.claude/constraints_middle/ubiquitous_language/business/GLOSSARY.md` |
| Team members, roles, personas | `.claude/constraints_middle/ubiquitous_language/organizational/GLOSSARY.md` |
| SDD workflow, agents, commands, technical terms | `.claude/constraints_middle/ubiquitous_language/development/GLOSSARY.md` |
| General terms (company name, CTAs, taglines) | `.claude/constraints_middle/DOMAIN_GLOSSARY.md` |

### Step 2: Find the Term

Search the glossary for the concept you're about to name or reference. The glossary has three sections:
- **Global Terms** — cross-context terms (company name, CTAs)
- **Bounded Context Glossaries** — context-specific terms

### Step 3: Use the Canonical Name

Once you find the term, use the exact string from the "Canonical Name" column. Never:
- Abbreviate (e.g., don't write "BC" for a multi-word canonical term)
- Translate to synonyms (e.g., don't write a generic substitute for a glossary term)
- Change capitalization (e.g., don't lowercase a properly-cased canonical term)

### Step 4: Flag Missing Terms

If the concept has no glossary entry:
1. **Do not invent a term.** Pause and add the term to the appropriate glossary.
2. Add a "Canonical Name" column entry with the proposed name.
3. Note it as "proposed" until confirmed.
4. Proceed with the proposed name.

### Step 5: Flag Inconsistencies

If observed usage in the codebase differs from the glossary:
1. **Do not silently conform to the code.** The glossary is authoritative.
2. Flag the discrepancy in the PR/spec description.
3. Update the code to match the glossary, OR
4. Update the glossary if the code is correct and the glossary is stale.
5. Update `.claude/constraints_middle/DOMAIN_GLOSSARY.md` "Known Inconsistencies" section.

## Examples

### Good: Using the Glossary

```javascript
// Before (generic name):
function handleClick() { ... }

// After (glossary term):
function handle{{DomainNoun}}Click() { ... }
// "{{DomainNoun}}" is a canonical term from the glossary
```

```javascript
// Before (invented name):
const dataItems = [...];

// After (glossary term):
const {{DOMAIN_DATA_STRUCTURE}} = [...];
// The canonical data structure name is from the glossary
```

```markdown
<!-- Before (inconsistent term): -->
## Our Services

<!-- After (glossary term): -->
## {{CANONICAL_SECTION_LABEL}}
// "{{CANONICAL_SECTION_LABEL}}" is the canonical section-label from the glossary
```

### Bad: Violating the Glossary

```javascript
// BAD: generic term instead of the canonical glossary term
function handle{{WrongTerm}}Click() { ... }

// BAD: invented synonym instead of the canonical data structure name
const genericBundles = [...];

// BAD: lowercase when the glossary term uses title case
const termName = "canonical term but lowercased";
```

## Verification Loop

After making changes, verify:
1. **Search the codebase** for any non-glossary terms that should be glossary terms.
2. **Check test names** — do they use glossary terms?
3. **Check spec files** — do requirements.md, design.md, tasks.md use glossary terms?
4. **Check UI labels** — do HTML headings and section labels match glossary terms?

## The Glossary as a Guide (Feedforward Control)

The glossary is not just a reference — it is an active **feedforward control**. Every glossary term is a constraint that prevents errors before they happen:

| Glossary Entry | Prevents | How |
|----------------|---------|-----|
| Canonical UI term | Generic naming ("card", "item", "widget") | Agent must use canonical name |
| Canonical data name | Invented terms not in glossary | Glossary scan flags violations |
| Role title (capitalized) | Inconsistent persona capitalization | Naming guide |
| "CSRF token" | Generic "token" terminology | Development glossary |

**This is a Guide because:** It shapes behavior *before* code is written. The agent consults the glossary, sees the canonical term, and uses it. A sensor (Domain Linter) would catch the error *after* — the Guide prevents it *before*.

**Cross-reference:** See `.knowledge_base/harness_engineering_reference/guides.md` for the full Guide framework.

## References

- Eric Evans, "Domain-Driven Design: Tackling Complexity in the Heart of Software" (2003-04-15), Chapter 2: Communication and the Use of Language
