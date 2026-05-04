---
name: setup-local
description: Detect OS, install missing tools, bootstrap the project, and verify local dev environment
---

# /setup-local

Delegates to `.claude/agents/configurations/local-setup.md`.

## Flags

| Flag | Description |
|------|------------|
| `--check-only` | Verify environment without installing anything |
| `--verbose` | Show detailed output for each step |

## Prerequisites
- Project dependencies listed in `{{MANIFEST_FILE}}` (package.json / requirements.txt / go.mod / etc.)
- `{{ENV_FILE}}` template exists (`.env.example` or equivalent)

## Steps

### Step 1 — Detect OS and shell
Identify: Windows/macOS/Linux, bash/zsh/PowerShell/fish.

### Step 2 — Delegate to local-setup agent
Run `.claude/agents/configurations/local-setup.md` workflow.

### Step 3 — Report
Output environment readiness report with pass/fail for each required tool.

## Rules
- Show all commands before running them
- Never run destructive setup commands automatically
- If `{{ENV_FILE}}` is missing: create from `.env.example`, never from scratch
- Always end with: running `{{TEST_COMMAND}}` to verify setup is complete

> **Init note**: After `/init-harness`, update `{{MANIFEST_FILE}}`, `{{RUNTIME_VERSION}}`, and `{{TEST_COMMAND}}` placeholders with actual values.
