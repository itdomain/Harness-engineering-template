---
name: /git-commit-push
description: Stage, commit, and push changes to the git repository with proper commit messages following CLAUDE.md conventions.
---

## /git-commit-push — Git Commit & Push Orchestration

Stage, commit, and push changes with proper commit messages and AI model attribution.

### Steps

1. **Read the skill**: `.claude/skills/git-commit-push/SKILL.md`
2. **Check git status**: `git status` to identify all changes.
3. **Filter `.gitignore`**: Never include `.gitignore` in commits.
4. **Stage relevant files**: `git add` only modified/deleted/new files (excluding `.gitignore`).
5. **Determine commit scope**: Identify the primary area of change.
6. **Extract model name**: From system prompt, identify the current AI model.
7. **Draft commit message**: `type(scope): subject` + `Co-Generated-With: <model-name>`.
8. **Show draft to user** for confirmation.
9. **Commit** using heredoc format.
10. **Push** to remote. Report new commit SHA.

### Rules

- Never include `.gitignore` in commits.
- Never include email addresses in Co-Generated-With lines.
- Always show the commit message before committing.
- Push only after commit succeeds.
- If push fails due to remote updates, suggest rebase.
- Follow CLAUDE.md commit format strictly.

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
