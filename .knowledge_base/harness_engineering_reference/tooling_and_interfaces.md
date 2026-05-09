# Tooling and Interfaces

## Interface Types

Harness engineering for coding agents concept encompases several categories of interfaces that form the harness:

### Language Servers and Static Analyzers
Provide real-time feedback on code quality, type errors, and style violations.

### Pre-commit Hooks
Automated checks that run before code is committed. Currently absent from this project. The pre-flight checklist concept replaces this gap.

### AI Review Assistants
The Claude Code agent itself, configured through CLAUDE.md, agent definitions, skills, and commands. This is the primary "AI review assistant" in this project.

## Configuration Mechanisms

Harness engineering concept identifies three configuration mechanisms:

1. **Documentation files.** CLAUDE.md, agent definitions, command files, skill definitions. These are the "documentation files" in this project.

2. **Bootstrap scripts.** Scripts that set up the development environment. Currently: `/setup-local` command.

3. **Custom rule sets.** Governance documents, security policies, coding standards. Currently: `.claude/constraints_middle/` (preflight, security-constraints, llm-security-constraints, architectural-constraints, ubiquitous_language/).

## Current Tooling Inventory

> **Init note**: This table is populated after `/init-harness` based on your project's detected stack.

| Tool | Interface type | Purpose |
|------|---------------|---------|
| `CLAUDE.md` | Context provision | Primary harness entry point |
| Agent definitions | Configuration | SDD workflow agents |
| Slash commands | Interface layer | SDD + operational commands |
| Skills | Interface layer | Reusable capabilities (caveman, git-commit-push, playwright-cli) |
| Constraints bundle | Configuration | Pre-flight, security, architecture, glossary |
| `{{TEST_FRAMEWORK}}` + `{{TEST_ENVIRONMENT}}` | Test runner | TDD and mutation testing |
| `{{MCP_TOOL_NAME}}` | MCP integration | Browser automation (if configured) |

## Reusable Configuration Bundles

Harness engineering concept describes reusable configurations as "bundles of guides and sensors for standard service topologies, enabling consistent application."

**Current state:** Skills are isolated and single-purpose. No way to bundle guides + sensors for a pattern.

**Target state:** Reusable bundles for common patterns:
- **API pattern:** Security constraints + input validation guides + CSRF sensors
- **Frontend TDD pattern:** {{TEST_FRAMEWORK}} setup + {{TEST_ENVIRONMENT}} globals + accessibility checks
- **Deployment pattern:** Pre-deploy checklist + post-deploy verification

These bundles would be composable — a single change might activate multiple bundles depending on what files are being modified.

## Key Takeaway

The quality of a harness is bounded by the quality of its interfaces. A well-engineered harness provides clear, consistent, and composable interfaces between the model, the tools, and the human. The current project has a solid foundation but lacks the composability that reusable bundles would provide.

## References

- Birgitta Böckeler, "Harness engineering for coding agent users" (2026-04-02) - https://martinfowler.com/articles/harness-engineering.html
