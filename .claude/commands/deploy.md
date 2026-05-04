---
name: deploy
description: Generate deployment plan and execute deployment for {{PROJECT_NAME}}
---

# /deploy

Generate a deployment plan and execute deployment to `{{DEPLOY_TARGET}}`.

## Flags

| Flag | Description |
|------|------------|
| `--dry-run` | Generate plan only — do not execute |
| `--via git` | Deploy via git push (default if configured) |
| `--via sftp` | Deploy via SFTP |
| `--via ci` | Trigger CI/CD pipeline |

## Prerequisites
- All SDD tasks for this change are complete (`/validate-sdd` passed or explicitly skipped)
- `{{TEST_COMMAND}}` passes
- No uncommitted changes in `{{ENV_FILE}}` or `{{SENSITIVE_DIR}}/`

## Steps

### Step 1 — Pre-flight
Run `.claude/constraints_middle/preflight.md` checklist.
Run `{{TEST_COMMAND}}` — must pass before proceeding.

### Step 2 — Classify files
Show user: files to deploy, files excluded, reasons for exclusion.
**Always exclude**: `{{ENV_FILE}}`, `{{SENSITIVE_DIR}}/`, test files, `.claude/`, `node_modules/`, `vendor/`.

### Step 3 — Show plan + confirm
Display the full deployment plan. Wait for explicit user confirmation before executing.

### Step 4 — Execute
Run deployment via configured method (git push / SFTP / CI trigger).
Show output in real time.

### Step 5 — Post-deploy verification
Run smoke test / health check against production URL.
Verify critical paths work.

### Step 6 — Report
Output: deployment result, verification results, any errors encountered.

### Step 7 — Rollback (if needed)
If deployment fails: provide rollback command/procedure.
Escalate per `.claude/handoff_boundary/escalation-matrix.md`.

## Rules
- **Never deploy without user confirmation after seeing the plan**
- Never include `{{ENV_FILE}}` in deployment
- Always run tests before deploying
- `--dry-run` is safe: generates plan only, no deployment executed
- Production deployments require explicit `--production` flag (not default)

> **Init note**: After running `/init-harness`, fill in `{{DEPLOY_TARGET}}` with your actual deployment target (Vercel, AWS, Fly.io, self-hosted, etc.) and add any platform-specific steps.
