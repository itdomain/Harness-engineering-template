# Domain Glossary

> **Init note**: This file is the master glossary for {{PROJECT_NAME}}. Populate it with your domain's canonical terms after running `/init-harness`. See `.claude/constraints_middle/CONSULT_THE_GLOSSARY.md` for how Claude uses this file.

## Usage Rules for Agents

1. **Before naming any variable, function, class, or UI element**, look up the correct term here.
2. **Never invent synonyms**. If a concept doesn't appear in this glossary, add it before writing code.
3. **Bounded-context glossaries** (business, organizational, development) override this file when in conflict.
4. **If unsure**, ask the user before proceeding.

## Global Terms

| Term | Definition | Usage context |
|------|-----------|---------------|
| `{{PROJECT_NAME}}` | The project itself | All contexts |
| `{{TERM_1}}` | {{DEFINITION_1}} | {{CONTEXT_1}} |
| `{{TERM_2}}` | {{DEFINITION_2}} | {{CONTEXT_2}} |
| `{{TERM_3}}` | {{DEFINITION_3}} | {{CONTEXT_3}} |

> Add your canonical domain terms here. Every term used in code, UI, or specs must appear in this table.

## Known Inconsistencies

| Legacy term | Correct term | Where found |
|-------------|-------------|-------------|
| (none yet) | — | — |

> Track naming drift here. When you find a legacy term in the codebase, add it to this table and schedule a rename.

## Bounded Context Glossaries

- **Business**: `.claude/constraints_middle/ubiquitous_language/business/GLOSSARY.md`
- **Organizational**: `.claude/constraints_middle/ubiquitous_language/organizational/GLOSSARY.md`
- **Development**: `.claude/constraints_middle/ubiquitous_language/development/GLOSSARY.md`
