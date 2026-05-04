# Harness Init Guide

Detailed walkthrough for initializing the harness on a new project.

## What `/init-harness` Does

Runs in four phases:

**Phase 1 — Scan**: Reads project root files to detect language, test framework, test command, sensitive paths.

**Phase 2 — Ask**: Presents detected values and asks 7 targeted questions. Never assumes — always confirms.

**Phase 3 — Substitute**: Replaces all `{{PLACEHOLDER}}` tokens across every harness file with your project's values.

**Phase 4 — Report**: Lists what was customized, what still needs manual attention, and next steps.

## Questions Asked During Init

| # | Question | Auto-detected? | Example answer |
|---|----------|---------------|----------------|
| 1 | Project name | Default: directory name | `my-saas-app` |
| 2 | Project description (1-2 sentences) | No | `Billing dashboard for SMBs. Allows users to manage subscriptions and invoices.` |
| 3 | Tech stack | Yes (confirm/correct) | `Node.js 20 + React 18 + PostgreSQL` |
| 4 | Test command | Yes (confirm/correct) | `pnpm test:unit` |
| 5 | Primary source files (1-3) | No | `src/`, `app/` |
| 6 | Deploy target | No (optional) | `Vercel` or skip |
| 7 | Additional sensitive paths | Detected .env files shown | `.env.local, secrets/` |

## Placeholder Reference

All `{{PLACEHOLDER}}` tokens used throughout the harness:

| Placeholder | Description | Set by |
|-------------|-------------|--------|
| `{{PROJECT_NAME}}` | Project display name | Question 1 |
| `{{PROJECT_SLUG}}` | URL-safe project identifier | Derived from Q1 |
| `{{PROJECT_DESCRIPTION}}` | 1-2 sentence description | Question 2 |
| `{{PROJECT_TAGLINE}}` | Short tagline (optional) | Question 2 |
| `{{TECH_STACK_SUMMARY}}` | Full stack description | Question 3 |
| `{{PRIMARY_LANGUAGE}}` | Primary programming language | Auto-detected |
| `{{BACKEND_LANGUAGE}}` | Backend language (if applicable) | Auto-detected |
| `{{BACKEND_RUNTIME}}` | Backend runtime + version | Auto-detected |
| `{{RUNTIME_VERSION}}` | Runtime version (Node, Python, etc.) | Auto-detected |
| `{{TEST_FRAMEWORK}}` | Test framework name | Auto-detected |
| `{{TEST_ENVIRONMENT}}` | Test environment (jsdom, node, etc.) | Auto-detected |
| `{{TEST_COMMAND}}` | Command to run tests | Question 4 |
| `{{TEST_FILE_PATTERN}}` | Glob pattern for test files | Auto-detected |
| `{{TEST_DIRECTORY}}` | Root directory for test files | Auto-detected |
| `{{TEST_CONFIG_FILE}}` | Test config file name | Auto-detected |
| `{{MUTATION_TOOL}}` | Mutation testing tool | Derived from language |
| `{{MUTATION_CONFIG_FILE}}` | Mutation config file | Derived from tool |
| `{{MUTATION_REPORT_FILE}}` | Mutation report output path | Derived from tool |
| `{{MAIN_SOURCE_FILE}}` | Primary source file (most-changed) | Question 5 |
| `{{MAIN_SOURCE_FILES}}` | Source file(s)/directories | Question 5 |
| `{{ENTRY_POINT_FILE}}` | Application entry point | Question 5 |
| `{{MAIN_STYLESHEET}}` | Main stylesheet (if applicable) | Auto-detected |
| `{{PACKAGE_MANAGER}}` | Package manager (npm, pnpm, pip, etc.) | Auto-detected |
| `{{PACKAGE_MANAGER_VERSION}}` | Package manager version | Auto-detected |
| `{{ADD_DEPENDENCY_COMMAND}}` | Command to add a dependency | Derived from PM |
| `{{DEPLOY_TARGET}}` | Deployment platform | Question 6 |
| `{{HOSTING_PROVIDER}}` | Hosting provider name | Question 6 |
| `{{WEB_SERVER}}` | Web server (if applicable) | Question 6 |
| `{{ENV_FILE}}` | Path to environment file | Auto-detected |
| `{{SENSITIVE_DIR}}` | Sensitive directory path | Auto-detected |
| `{{SENSITIVE_PATHS}}` | All sensitive paths (comma list) | Auto + Question 7 |
| `{{PUBLIC_HANDLER_FILE}}` | Public-facing handler file | Manual |
| `{{PRIVATE_HANDLER_FILE}}` | Private handler file | Manual |
| `{{SERVER_CONFIG_FILE}}` | Server config (.htaccess, nginx.conf) | Auto-detected |
| `{{EMAIL_LIBRARY}}` | Email sending library | Auto-detected |
| `{{SMTP_HOST}}` | SMTP hostname | Manual |
| `{{SMTP_PORT}}` | SMTP port | Manual |
| `{{ENV_VAR_SMTP_USER}}` | SMTP username env var name | Manual |
| `{{ENV_VAR_SMTP_PASS}}` | SMTP password env var name | Manual |
| `{{CONTACT_EMAIL}}` | Contact/support email address | Manual |
| `{{DESIGN_SYSTEM_NAME}}` | Design system name | Manual |
| `{{TYPOGRAPHY_SYSTEM_NAME}}` | Typography system name | Manual |
| `{{HEADLINE_FONT}}` | Headline font name | Manual |
| `{{UI_FONT}}` | UI/navigation font name | Manual |
| `{{BODY_FONT}}` | Body copy font name | Manual |
| `{{COLOR_BG}}` | Background color value | Manual |
| `{{COLOR_SURFACE}}` | Surface/card color value | Manual |
| `{{COLOR_RAISED}}` | Elevated surface color | Manual |
| `{{COLOR_ACCENT}}` | Primary accent color | Manual |
| `{{COLOR_TEXT}}` | Primary text color | Manual |
| `{{COLOR_NEUTRAL}}` | Secondary text color | Manual |
| `{{COLOR_HIGHLIGHT}}` | Highlight color | Manual |
| `{{COLOR_ERROR}}` | Error/fail color | Manual |
| `{{TEAM_MEMBER_1}}` | First team member name | Manual |
| `{{TEAM_MEMBER_2}}` | Second team member name | Manual |
| `{{SHARED_UTIL_FUNCTION}}` | Shared utility function name | Manual |
| `{{MODAL_MANAGER_CLASS}}` | Modal manager class name | Manual |
| `{{DOMAIN_EVENT_HANDLER}}` | Example domain event handler | Manual |
| `{{DOMAIN_DATA_STRUCTURE}}` | Example domain data structure | Manual |
| `{{PERFORMANCE_METRICS}}` | Performance metrics to track | Manual |
| `{{AI_MODEL}}` | AI model ID for commit attribution | Fixed: claude-sonnet-4-6 |

## Manual Steps After Init

After `/init-harness` completes, these files need your domain knowledge:

### Required — SDD won't work without these
1. **`.claude/context_inner/architecture.md`** — Fill in: system overview, core files table, tech stack details
2. **`.claude/constraints_middle/DOMAIN_GLOSSARY.md`** — Add canonical terms for your domain

### Recommended — Improves guidance quality
3. **`.claude/constraints_middle/architectural-constraints.md`** — What patterns to preserve and avoid
4. **`.claude/constraints_middle/ubiquitous_language/business/GLOSSARY.md`** — Business domain terms
5. **`.claude/constraints_middle/ubiquitous_language/development/GLOSSARY.md`** — Add tech-specific terms beyond SDD workflow defaults

### Optional — Enhanced monitoring
6. **`.claude/controls_outer/drift-detection.md`** — Project-specific drift checks (tokens, sections, dependencies)
7. **`.claude/constraints_middle/security-constraints.md`** — Add stack-specific security rules beyond generic baseline

## Troubleshooting

**`/init-harness` not found**: Ensure you copied `.claude/commands/init-harness.md` from the template to your project.

**Placeholders still showing after init**: Run `/init-harness` again. If specific placeholders cannot be auto-detected, Claude will ask for them.

**Wrong test command detected**: During Phase 2, correct the test command when prompted. Init uses your answer, not the detected value.

**Conflicting CLAUDE.md**: If your project already had a CLAUDE.md, merge the harness sections manually. The SDD Workflow, Governance, and Harness Structure sections must be present.

**settings.local.json conflicts**: Init regenerates this file. If you had custom permission rules, add them back after init completes.

## Updating the Harness

When a new version of the template is released:
1. Check the release notes for changed files
2. Copy only changed template files to your project
3. Re-apply your project-specific customizations
4. Run `/init-harness` again if placeholder names changed

## Adding Discipline Agents

Expansion slots in `.claude/agents/` and `.claude/commands/` are empty by default. To add discipline-specific agents:

1. Create `.claude/agents/<discipline>/<agent-name>.md`
2. Create `.claude/commands/<discipline>/<command-name>.md`
3. Register both in `.claude/AGENTS.md` and `.claude/COMMANDS.md`

See `.knowledge_base/harness_engineering_reference/INDEX.md` for agent design principles.
