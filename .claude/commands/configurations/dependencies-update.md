---
name: dependencies-update
description: Check and update project dependencies to latest eligible versions
---

# /dependencies-update

Check for outdated dependencies and update them following the Dependency Age Protection rule.

## Flags

| Flag | Description |
|------|------------|
| `--check` | Report available updates without applying them |
| `--patch-only` | Apply patch-level updates only |
| `--all` | Include all dependencies (default: production only) |

## Prerequisites
- `{{MANIFEST_FILE}}` exists and is committed
- `{{TEST_COMMAND}}` passes before any updates

## Steps

### Step 1 — Snapshot
Run `{{TEST_COMMAND}}` — baseline must pass before updating.
Record current dependency versions.

### Step 2 — Delegate to dependency-updater agent
Run `.claude/agents/configurations/dependency-updater.md` workflow.

### Step 3 — Confirm
Show update plan to user. Wait for confirmation.

### Step 4 — Apply updates
Run `{{INSTALL_COMMAND}}` after confirmation.

### Step 5 — Verify
Run `{{TEST_COMMAND}}`. On failure: revert failing package only.

### Step 6 — Commit
Stage manifest changes only. Do not stage `{{ENV_FILE}}` or generated files.

## Rules
- **Dependency Age Protection**: never update to a release less than 7 days old
- Never update major versions without explicit user approval
- Always run tests after updates
- Revert individual failures — not all updates at once

> **Init note**: Update `{{MANIFEST_FILE}}`, `{{TEST_COMMAND}}`, and `{{INSTALL_COMMAND}}` with actual values after `/init-harness`.
