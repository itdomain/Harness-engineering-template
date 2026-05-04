---
name: git-commit-push
description: >
  Prepare, commit, and push changes to the git repository with proper commit messages
  following CLAUDE.md conventions. Automatically excludes .gitignore from commits,
  identifies the current AI model from session context for attribution,
  and follows the type(scope): subject format.
  Use whenever the user asks to commit, push, stage, or make a git commit.
  Also triggers on phrases like "commit and push", "make a commit", "push changes",
  "stage and commit", "git commit".
---

## Git Commit & Push Skill

Stage, commit, and push changes following project conventions.

### Rules

- **Never include `.gitignore` in commits** — it is a local configuration file, always excluded.
- **Always include AI model attribution** — extract the current model ID from system prompt (e.g., `{{AI_MODEL}}`, `claude-opus-4-7`) and add `Generated with <model-name>` to the commit message. Do NOT include email addresses.
- **Follow CLAUDE.md commit format**: `type(scope): subject` (imperative, lowercase, no period).
  - Types: `feat`, `fix`, `docs`, `refactor`, `style`, `chore`, `test`
  - Scope: short descriptor (e.g., `agents`, `config`, `skills`)
- **Attribution format:** `Generated with <model-name>` — matches CLAUDE.md convention.
- **Always show the commit message before committing** — let user confirm.
- **Push only after explicit user confirmation** or if user said "commit and push" (implicit confirmation).

### Steps

1. **Check status**: Run `git status` to identify changed, deleted, and untracked files.
2. **Filter `.gitignore`**: Exclude `.gitignore` from staging — never include it.
3. **Stage files**: `git add` only the relevant changed files (modified, deleted, new).
4. **Determine scope**: Identify the primary area changed (e.g., `agents`, `config`, `skills`).
5. **Draft commit message**:
   - Subject line: `type(scope): brief description of changes`
   - Attribution line: `Generated with <current-model-name>` (no email)
6. **Show the draft message** to user. If user approves, commit.
7. **Commit**: Use heredoc for multi-line commit message:
   ```
   git commit -m "$(cat <<'EOF'
   type(scope): subject line

   Generated with <model-name>
   EOF
   )"
   ```
8. **Push**: After commit, run `git push`. Report the new commit SHA and branch.

### Commit Message Examples

```
docs(agents): update operational agent definitions

Generated with {{AI_MODEL}}
```

```
chore: restore deleted agent config files

Generated with {{AI_MODEL}}
```

### Error Handling

- If no changes staged: report "No changes to commit."
- If push fails (remote updates): suggest `git pull --rebase` first.
- If pre-commit hook fails: diagnose and fix the issue, create a new commit (not amend).
- If branch is behind: warn user before push.
