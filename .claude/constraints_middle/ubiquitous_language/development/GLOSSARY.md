# Development Context Glossary — {{PROJECT_NAME}}

> **Bounded Context:** Software Development Process, Agents, Commands, and Technical Infrastructure. This context defines all development workflow terminology, agentic roles, and technical concepts.

## Context Boundary

This glossary governs terminology used in:
- `.claude/` — agents, commands, skills, governance
- `specs/` — SDD artifacts, templates, archives
- `CLAUDE.md` — setup, SDD workflow, commit guidelines
- `{{MAIN_SOURCE_FILE}}` — technical function names, patterns
- `{{PUBLIC_HANDLER_FILE}}`, `{{PRIVATE_HANDLER_FILE}}` — backend infrastructure

## SDD Workflow Terms

| Term | Definition | Canonical Name |
|------|-----------|----------------|
| **SDD** | Spec-Driven Development — 5-phase methodology | Always "SDD" or "Spec-Driven Development" |
| **requirements.md** | First SDD output — business logic, edge cases, performance, success metrics | Always "requirements.md" |
| **design.md** | Second SDD output — schema/API changes, data flow, component architecture | Always "design.md" |
| **tasks.md** | Third SDD output — atomic implementation plan with test tasks | Always "tasks.md" |
| **validation_report.md** | Fourth SDD output — PASS/FAIL against requirements | Always "validation_report.md" |
| **fix_request.md** | Generated on FAIL validation, triggers re-evaluation by Architect | Always "fix_request.md" |
| **history.log** | Progress log during execution | Always "history.log" |
| **test_history.log** | Spec-to-test mapping log | Always "test_history.log" |
| **RED-GREEN cycle** | Test-first development: Red (failing test) → Green (passing test) | Always "RED-GREEN cycle" or "TDD RED-GREEN cycle" |
| **TDD RED** | Test generation phase — write failing tests | Always "TDD RED" or "RED cycle" |
| **TDD Green** | Implementation phase — make tests pass | Always "TDD Green" or "GREEN cycle" |
| **Definition of Done** | Checklist per task in tasks.md | Always "Definition of Done" |
| **Human Review Checkpoints** | Manual verification milestones in tasks.md | Always "Human Review Checkpoints" |
| **Archive** | Moving validated specs to specs/archive/ | Always "Archive" (verb: "archive", noun: "archive") |
| **spec-to-test mapping** | Documentation linking requirements to test suites | Always "spec-to-test mapping" |
| **atomic commit** | One logical change per commit | Always "atomic commit" |

## Slash Commands

| Term | Definition | Canonical Name |
|------|-----------|----------------|
| **/start-sdd** | Initialize SDD flow for feature or bug | Always "/start-sdd" |
| **/design-sdd** | Generate design.md from requirements.md | Always "/design-sdd" |
| **/plan-sdd** | Generate tasks.md (atomic task plan) | Always "/plan-sdd" |
| **/execute-sdd** | Generate tests + execute tasks.md | Always "/execute-sdd" |
| **/validate-sdd** | Validate against requirements, produce validation report | Always "/validate-sdd" |
| **/deploy** | Deployment orchestration | Always "/deploy" |
| **/setup-local** | Local environment setup | Always "/setup-local" |
| **/dependencies-update** | Check and update dependencies | Always "/dependencies-update" |
| **/git-commit-push** | Stage, commit, push with proper messages | Always "/git-commit-push" |
| **dry-run** | Flag for preview-only execution | Always "dry-run" |

## Agent Roles

| Term | Definition | Canonical Name |
|------|-----------|----------------|
| **Architect Agent** | Technical Scope & Design — translates requirements to design.md | Always "Architect Agent" |
| **Builder Agent** | Implementation — executes tasks.md with TDD Green cycle | Always "Builder Agent" |
| **Planner Agent** | Execution Strategy & Task Breakdown — converts design.md to tasks.md | Always "Planner Agent" |
| **TDD Builder Agent** | Test Generation — produces failing tests from requirements + design | Always "TDD Builder Agent" |
| **Auditor Agent** | Quality Assurance — verifies against requirements, produces validation_report.md | Always "Auditor Agent" |
| **Dependency Updater Agent** | Version Intelligence — checks latest dependency versions | Always "Dependency Updater Agent" |
| **Deploy Agent** | Deployment Planning & Package Preparation | Always "Deploy Agent" |
| **Local Setup Agent** | Environment Detection & Project Bootstrapping | Always "Local Setup Agent" |

## Technical Terms

> **Init note**: Add your project's canonical technical terms here — class names, function names, module names, patterns.

| Term | Definition | Type |
|------|-----------|------|
| `{{TECH_TERM_1}}` | {{DEFINITION_1}} | {{class/function/pattern/module}} |
| `{{TECH_TERM_2}}` | {{DEFINITION_2}} | {{type}} |

## Security Governance Terms

| Term | Definition | Canonical Name |
|------|-----------|----------------|
| **HSTS** | HTTP Strict Transport Security | Always "HSTS" |
| **Strict-Transport-Security** | HTTP header name | Always "Strict-Transport-Security" |
| **SSL stripping** | Attack type HSTS prevents | Always "SSL stripping" |
| **SSLStrip** | Attack tool name | Always "SSLStrip" |
| **MITM** | Man-in-the-Middle attack | Always "MITM" |
| **Session Fixation** | Attack type session regeneration prevents | Always "Session Fixation" |
| **CSRF** | Cross-Site Request Forgery | Always "CSRF" |
| **CSP** | Content Security Policy | Always "CSP" |
| **DOM XSS** | Cross-Site Scripting via DOM manipulation | Always "DOM XSS" |
| **Rate Limiting** | Request throttling mechanism | Always "Rate Limiting" |
| **POST rate limit** | {{RATE_LIMIT_POST}} per IP | Always "POST rate limit" |
| **GET rate limit** | {{RATE_LIMIT_GET}} per IP | Always "GET rate limit" |
| **X-XSS-Protection** | Deprecated header | Always "X-XSS-Protection" |
| **Defense-in-depth** | Security architecture principle | Always "Defense-in-depth" |
| **Session Regeneration** | Security rule requiring session ID rotation | Always "Session Regeneration" |
| **{{SECURE_COMPARE_FUNCTION}}()** | Timing-safe comparison for CSRF tokens | Always "{{SECURE_COMPARE_FUNCTION}}()" |
| **{{WEB_SERVER_MODULE_HEADERS}}** | {{WEB_SERVER}} module for security headers | Always "{{WEB_SERVER_MODULE_HEADERS}}" |
| **{{WEB_SERVER}}-level** | Server-level header enforcement | Always "{{WEB_SERVER}}-level" |


## Design System Terms

> **Init note**: If this is a UI project, add your design system tokens and typography terms here.

| Term | Value | Role |
|------|-------|------|
| `{{DESIGN_TOKEN_1}}` | `{{COLOR_BG}}` | Primary background |
| `{{DESIGN_TOKEN_2}}` | `{{COLOR_ACCENT}}` | Primary accent |
| `{{HEADLINE_FONT}}` | — | Headline font |
| `{{BODY_FONT}}` | — | Body font |

## References

- Eric Evans, "Domain-Driven Design: Tackling Complexity in the Heart of Software" (2003-04-15), Chapter 2: Communication and the Use of Language
