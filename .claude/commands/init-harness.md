---
name: init-harness
description: Initialize the Harness Engineering scaffold for this project. Run once after copying the template to a new project.
---

# /init-harness

Scan the project codebase, ask 7 targeted questions, substitute all `{{PLACEHOLDER}}` tokens in harness files, and generate a project-specific `settings.local.json`.

**Run this command once** after copying the harness template into a new project.

---

## Step 1 — Scan the codebase

Read the following files from the project root (if they exist):
- Top-level directory listing
- `package.json` → read `scripts`, `dependencies`, `devDependencies`, `engines`
- `tsconfig.json` → note if TypeScript
- `go.mod` → read module name, Go version
- `Cargo.toml` → read package name, edition
- `requirements.txt` → note Python project
- `pyproject.toml` → read tool.pytest, tool.poetry if present
- `pom.xml` → note Java/Maven project
- `build.gradle` → note Java/Kotlin/Gradle project
- `composer.json` → note PHP project
- `Gemfile` → note Ruby project
- `vitest.config.*`, `jest.config.*`, `pytest.ini`, `.rspec` → test framework config
- `stryker.config.*`, `mutmut.toml`, `pitest.*` → mutation tool config
- `.env`, `.env.*`, `.env.local`, `.env.example` → note existence (DO NOT read contents)
- `nginx.conf`, `.htaccess`, `apache*.conf` → server config
- Any `Makefile` → read test targets

Build these resolved values from the scan:

| Variable | How to detect |
|----------|--------------|
| `PRIMARY_LANGUAGE` | package.json/tsconfig → JS/TS; go.mod → Go; Cargo.toml → Rust; requirements.txt/pyproject → Python; pom.xml/build.gradle → Java; composer.json → PHP; Gemfile → Ruby |
| `TEST_FRAMEWORK` | vitest.config → {{TEST_FRAMEWORK}}; jest.config → Jest; pytest.ini/pyproject[tool.pytest] → pytest; go.mod present → go test; .rspec → RSpec; composer.json devDeps phpunit → PHPUnit |
| `TEST_COMMAND` | package.json scripts: prefer `test:feature` > `test:unit` > `test`; Makefile `test` target; else UNKNOWN |
| `TEST_FILE_PATTERN` | vitest.config include/exclude patterns; jest.config testMatch; else standard default per language |
| `TEST_DIRECTORY` | Infer from TEST_FILE_PATTERN (first path segment) |
| `TEST_CONFIG_FILE` | Detected config filename |
| `PACKAGE_MANAGER` | `pnpm-lock.yaml` → pnpm; `yarn.lock` → yarn; `bun.lockb` → bun; `package-lock.json` → npm; `requirements.txt`/`poetry.lock` → pip/poetry; `go.mod` → go; `Cargo.lock` → cargo; `composer.lock` → composer |
| `MUTATION_TOOL` | JS/TS → {{MUTATION_TOOL}}; Java → Pitest; Python → mutmut; Rust → cargo-mutants; PHP → Infection; Ruby → mutant; else → none |
| `MUTATION_CONFIG_FILE` | {{MUTATION_TOOL}} → `{{MUTATION_CONFIG_FILE}}`; mutmut → `setup.cfg [mutmut]`; else → none |
| `MUTATION_REPORT_FILE` | {{MUTATION_TOOL}} → `stryker-report/index.html`; else → `mutation-report/index.html` |
| `ENV_FILE` | First detected `.env*` file; default → `.env` |
| `SENSITIVE_PATHS` | All detected `.env*` files + `secrets/`, `private/`, `credentials/`, `*.pem`, `*.key` paths |
| `SERVER_CONFIG_FILE` | `.htaccess` → `{{SERVER_CONFIG_FILE}}`; `nginx.conf` → `nginx.conf`; else → none |
| `ENTRY_POINT_FILE` | `{{ENTRY_POINT_FILE}}` (if exists); `src/index.*`; `main.*`; `app.*`; `cmd/*/main.go`; first file in package.json `main` |
| `ADD_DEPENDENCY_COMMAND` | npm → `{{ADD_DEPENDENCY_COMMAND}}`; pnpm → `pnpm add`; yarn → `yarn add`; pip → `pip install`; go → `go get`; cargo → `cargo add`; composer → `{{ADD_DEPENDENCY_COMMAND}}` |
| `INSTALL_COMMAND` | npm → `{{ADD_DEPENDENCY_COMMAND}}`; pnpm → `pnpm install`; yarn → `yarn`; pip → `pip install -r requirements.txt`; go → `go mod download`; cargo → `cargo build` |
| `RUNTIME_VERSION` | `engines` field in package.json; go.mod go directive; python_requires in pyproject; else → "see README" |

---

## Step 2 — Present scan results + ask questions

Display detected values clearly. Use normal mode (not caveman) for this interaction — clarity matters during setup.

Ask the following questions. Accept Enter to confirm detected values:

**Q1. Project name**
```
Project name? [detected: <directory-name>]
```
Default: current directory name (last path segment).

**Q2. Project description**
```
Describe this project in 1-2 sentences (purpose + users):
```
No default. Required — used throughout harness documentation.

**Q3. Tech stack confirmation**
```
Detected stack: <PRIMARY_LANGUAGE> / <TEST_FRAMEWORK> / <PACKAGE_MANAGER>
Is this correct? [Y/n] If N, describe the actual stack:
```

**Q4. Test command**
```
Test command (for running a single feature's tests): [detected: <TEST_COMMAND> or UNKNOWN]
```
If UNKNOWN: required input.

**Q5. Primary source files**
```
Primary source files or directories (1-3, most changed during feature work):
e.g., "src/", "lib/", "app/controllers/"
```
No default. Required.

**Q6. Deploy target (optional)**
```
Deploy target? (e.g., Vercel, AWS ECS, Fly.io, Docker/k8s, self-hosted, skip)
[Enter to skip]
```

**Q7. Additional sensitive paths**
```
Sensitive paths Claude should never read (I found: <SENSITIVE_PATHS>):
Add more? [Enter to keep detected only]
```

---

## Step 3 — Resolve full placeholder set

Map all answers to placeholders:

```
PROJECT_NAME          = Q1 answer
PROJECT_SLUG          = Q1 lowercase, spaces→dashes, remove special chars
PROJECT_DESCRIPTION   = Q2 answer
PROJECT_TAGLINE       = "" (leave blank — user fills in architecture.md)
TECH_STACK_SUMMARY    = Q3 confirmed/corrected
PRIMARY_LANGUAGE      = from scan + Q3
BACKEND_LANGUAGE      = from Q3 (if multi-language) or same as PRIMARY_LANGUAGE
BACKEND_RUNTIME       = from Q3 or scan
RUNTIME_VERSION       = from scan + Q3
TEST_FRAMEWORK        = from scan + Q3
TEST_ENVIRONMENT      = from test config (jsdom/node/happy-dom) or "standard"
TEST_COMMAND          = Q4 answer
TEST_FILE_PATTERN     = from scan, or derived from Q4
TEST_DIRECTORY        = from TEST_FILE_PATTERN
TEST_CONFIG_FILE      = from scan
MUTATION_TOOL         = from scan (language-derived)
MUTATION_CONFIG_FILE  = from scan or derived
MUTATION_REPORT_FILE  = from scan or derived
MAIN_SOURCE_FILE      = first item from Q5
MAIN_SOURCE_FILES     = Q5 answer (all items)
ENTRY_POINT_FILE      = from scan
MAIN_STYLESHEET       = detected CSS/SCSS/less file if exists, else "none"
PACKAGE_MANAGER       = from scan
PACKAGE_MANAGER_VERSION = from scan
ADD_DEPENDENCY_COMMAND = derived from PACKAGE_MANAGER
INSTALL_COMMAND       = derived from PACKAGE_MANAGER
DEPLOY_TARGET         = Q6 answer or "TBD — set in deploy.md"
HOSTING_PROVIDER      = Q6 answer or "TBD"
WEB_SERVER            = from scan (nginx/apache) or Q6 or "TBD"
ENV_FILE              = from scan
SENSITIVE_DIR         = first directory in SENSITIVE_PATHS
SENSITIVE_PATHS       = detected + Q7 additions (comma-separated)
PUBLIC_HANDLER_FILE   = "TBD — see escalation-matrix.md"
PRIVATE_HANDLER_FILE  = "TBD — see escalation-matrix.md"
SERVER_CONFIG_FILE    = from scan or "none"
EMAIL_LIBRARY         = from scan (PHPMailer/nodemailer/etc.) or "TBD"
SMTP_HOST             = "TBD"
SMTP_PORT             = "TBD"
ENV_VAR_SMTP_USER     = "SMTP_USER"
ENV_VAR_SMTP_PASS     = "SMTP_PASS"
CONTACT_EMAIL         = "TBD"
DESIGN_SYSTEM_NAME    = "TBD — see architecture.md"
TYPOGRAPHY_SYSTEM_NAME = "TBD — see architecture.md"
HEADLINE_FONT         = "TBD"
UI_FONT               = "TBD"
BODY_FONT             = "TBD"
COLOR_BG              = "TBD"
COLOR_SURFACE         = "TBD"
COLOR_RAISED          = "TBD"
COLOR_ACCENT          = "TBD"
COLOR_TEXT            = "TBD"
COLOR_NEUTRAL         = "TBD"
COLOR_HIGHLIGHT       = "TBD"
COLOR_ERROR           = "TBD"
TEAM_MEMBER_1         = "TBD"
TEAM_MEMBER_2         = "TBD"
SHARED_UTIL_FUNCTION  = "TBD"
MODAL_MANAGER_CLASS   = "TBD"
DOMAIN_EVENT_HANDLER  = "TBD"
DOMAIN_DATA_STRUCTURE = "TBD"
PERFORMANCE_METRICS   = "Core Web Vitals" (frontend) or "p95 latency, error rate" (API) or "TBD"
AI_MODEL              = "claude-sonnet-4-6"
MANIFEST_FILE         = package.json / requirements.txt / go.mod / Cargo.toml / composer.json / Gemfile
```

---

## Step 4 — Substitute placeholders in all harness files

For each file in:
- `CLAUDE.md`
- `.claudeignore`
- `.claude/**/*.md`
- `.knowledge_base/**/*.md`

Search for all `{{PLACEHOLDER}}` patterns and replace with resolved value.

Use the Edit tool for files that already exist. If a placeholder has value "TBD", keep it as "TBD — [description]" so the user knows what to fill in.

Log each file processed. If a file has no placeholders, skip silently.

Important: Do NOT substitute placeholders inside code blocks (``` blocks) — those are examples, not live references.

---

## Step 5 — Generate settings.local.json

Create `.claude/settings.local.json` with permissions tuned for the detected stack:

```json
{
  "permissions": {
    "allow": [
      "Bash(git *)",
      "Bash(npx playwright*)",
      "<STACK_SPECIFIC_RULES>"
    ],
    "deny": [
      "Read(<ENV_FILE>)"
    ]
  }
}
```

Stack-specific allow rules to include:

| Language | Add these rules |
|----------|----------------|
| JavaScript/TypeScript (npm) | `Bash(npm *)`, `Bash(npx vitest*)`, `Bash(npx stryker*)`, `Bash(node *)` |
| JavaScript/TypeScript (pnpm) | `Bash(pnpm *)`, `Bash(npx vitest*)`, `Bash(npx stryker*)`, `Bash(node *)` |
| JavaScript/TypeScript (yarn) | `Bash(yarn *)`, `Bash(npx vitest*)`, `Bash(npx stryker*)`, `Bash(node *)` |
| Python | `Bash(python *)`, `Bash(python3 *)`, `Bash(pytest *)`, `Bash(pip *)`, `Bash(uv *)` |
| Go | `Bash(go test*)`, `Bash(go build*)`, `Bash(go run*)`, `Bash(go mod*)` |
| Rust | `Bash(cargo test*)`, `Bash(cargo build*)`, `Bash(cargo run*)` |
| PHP | `Bash(php *)`, `Bash(composer *)` |
| Ruby | `Bash(ruby *)`, `Bash(bundle *)`, `Bash(rspec *)` |
| Java (Maven) | `Bash(mvn *)` |
| Java (Gradle) | `Bash(gradle *)`, `Bash(./gradlew *)` |

Add a deny rule for each path in SENSITIVE_PATHS:
- `"Read(<path>)"` for each sensitive file/directory

Use the Write tool (not Edit) for settings.local.json — always regenerate from scratch.

---

## Step 6 — Create .gitkeep files (if missing)

Ensure `.gitkeep` exists in all expansion slot directories:
- `.claude/agents/ba/`
- `.claude/agents/frontend/`
- `.claude/agents/backend/`
- `.claude/agents/qa/`
- `.claude/agents/data/`
- `.claude/commands/ba/`
- `.claude/commands/frontend/`
- `.claude/commands/backend/`
- `.claude/commands/qa/`
- `.claude/commands/data/`
- `specs/features/`
- `specs/bugs/`
- `specs/archive/features/`
- `specs/archive/bugs/`

---

## Step 7 — Report

Print a summary:

```
Harness initialized for: <PROJECT_NAME>

✓ Files customized: <count>
✓ settings.local.json generated for: <PRIMARY_LANGUAGE> stack

Placeholders still set to TBD (need your domain knowledge):
  - .claude/context_inner/architecture.md → fill in system architecture
  - .claude/constraints_middle/DOMAIN_GLOSSARY.md → add domain terms
  - .claude/constraints_middle/architectural-constraints.md → add preserve/avoid patterns
  - .claude/constraints_middle/ubiquitous_language/business/GLOSSARY.md → business terms
  - .claude/controls_outer/drift-detection.md → add project-specific drift checks
  [list any other TBD placeholders found]

Next steps:
  1. Fill in .claude/context_inner/architecture.md
  2. Add domain terms to .claude/constraints_middle/DOMAIN_GLOSSARY.md
  3. Run /setup-local to verify your development environment
  4. Run /start-sdd feature <category> <feature-name> to begin first feature
  
See INIT_GUIDE.md for detailed guidance on each manual step.
```

---

## Rules

- Use normal mode (not caveman) for all interaction during init — clarity matters
- Never read `.env` file contents — only detect existence
- Never assume test command — always confirm with user if not found in package.json
- Always show detected values before asking for confirmation
- Process ALL files with placeholders — do not skip any
- settings.local.json: always use Write (not Edit) — recreate from scratch
- If the project already has a CLAUDE.md without placeholders: do not overwrite — warn user to merge manually
- Complete all substitutions before generating the report
- If any required question (Q1-Q5) is left blank: re-ask — do not proceed with empty values

---

## Supported Tech Stacks

This init command supports projects using:
- **JavaScript / TypeScript**: Node.js, React, Vue, Angular, Express, Next.js, etc.
- **Python**: Django, FastAPI, Flask, scripts, data science
- **Go**: standard library, Gin, Echo, gRPC services
- **Rust**: Cargo-based projects
- **PHP**: Laravel, WordPress, vanilla PHP
- **Ruby**: Rails, Sinatra, scripts
- **Java / Kotlin**: Maven or Gradle projects
- **Mixed stacks**: frontend (JS/TS) + backend (any) — detect both, generate rules for both

For unsupported stacks: complete Phase 1-3 (scan + questions + placeholders), skip Phase 5 stack-specific rules, and note in the report that settings.local.json needs manual configuration.
