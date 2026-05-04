# Confirmation Patterns

> **Human-in-the-loop pattern.** Define what always needs confirmation, what can be auto-approved, and what needs conditional approval.

## Always Requires Confirmation

These actions MUST get explicit user approval before proceeding:

- `git push` to any remote branch
- Production deployment (`/deploy`)
- Installing new dependencies (`{{ADD_DEPENDENCY_COMMAND}}`)
- Deleting files or directories
- Modifying `{{ENV_FILE}}` or any secret
- Changing security headers or CSP directives
- Modifying `{{SERVER_CONFIG_FILE}}` security rules
- Running destructive git operations (reset --hard, push --force, branch -D)
- Any action that cannot be easily undone

## Auto-Approved

These actions can proceed without explicit confirmation:

- Running tests (`{{TEST_COMMAND}}`)
- Fixing test failures (never modifying assertions)
- Running security audit commands
- Reading files for analysis
- Running `git status`, `git diff`, `git log`
- Creating new files in `specs/` or `.claude/`
- Typo corrections in source files that don't change behavior
- CSS formatting and deduplication
- Adding comments to explain WHY (not WHAT)
- Creating archive directories for completed SDD work

## Conditional Approval

These actions need approval only under certain conditions:

| Action | Auto-approve | Requires confirmation |
|--------|-------------|----------------------|
| CSS changes | Style tweaks, color swaps | New sections, structural changes |
| JS changes | Bug fixes matching spec | New features, new files |
| `{{ENTRY_POINT_FILE}}` changes | Content updates, typo fixes | New sections, structural changes |
| Dependency updates | Patch versions | Major versions, new packages |
| Spec changes | Typos, clarifications | New requirements, scope changes |
| Agent changes | Typos in definitions | New agents, workflow changes |
| Glossary changes | Adding single terms | Restructuring bounded contexts |
