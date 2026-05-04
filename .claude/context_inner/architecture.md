# Architecture

> **Init note**: This file is populated by `/init-harness` with your answers and auto-detected stack information. Fill in all `{{PLACEHOLDER}}` sections and expand as needed.

## Overview

{{PROJECT_DESCRIPTION}}

**Tech stack**: {{TECH_STACK_SUMMARY}}

## Core Files

| File | Role |
|------|------|
| `{{ENTRY_POINT_FILE}}` | Application entry point |
| `{{MAIN_SOURCE_FILE}}` | Primary source module |
| `{{MAIN_STYLESHEET}}` | Main styles |
| `{{PUBLIC_HANDLER_FILE}}` | Public-facing handler |
| `{{ENV_FILE}}` | Environment configuration (excluded from git) |

> Add all significant files to this table. This is the primary reference Claude uses to understand your file structure.

## Tech Stack Details

| Layer | Technology | Version |
|-------|-----------|---------|
| Runtime | {{PRIMARY_LANGUAGE}} | {{RUNTIME_VERSION}} |
| Backend | {{BACKEND_LANGUAGE}} | {{BACKEND_RUNTIME}} |
| Test framework | {{TEST_FRAMEWORK}} | — |
| Package manager | {{PACKAGE_MANAGER}} | {{PACKAGE_MANAGER_VERSION}} |
| Deploy target | {{DEPLOY_TARGET}} | — |

## UI / Section Map

> If this is a UI project, describe your page/section structure here. If it is an API or library, describe your module structure instead.

```
{{UI_SECTION_MAP_OR_MODULE_MAP}}
```

## Design System

> If applicable, add your design tokens and typography here. Delete this section for non-UI projects.

### Palette — {{DESIGN_SYSTEM_NAME}}

| Token | Value | Role |
|-------|-------|------|
| `{{PALETTE_TOKEN_1}}` | `{{COLOR_BG}}` | Primary background |
| `{{PALETTE_TOKEN_2}}` | `{{COLOR_SURFACE}}` | Card/section surface |
| `{{PALETTE_TOKEN_3}}` | `{{COLOR_ACCENT}}` | Primary accent |
| `{{PALETTE_TOKEN_4}}` | `{{COLOR_TEXT}}` | Primary text |
| `{{PALETTE_TOKEN_5}}` | `{{COLOR_NEUTRAL}}` | Secondary text |

### Typography — {{TYPOGRAPHY_SYSTEM_NAME}}

| Element | Font | Styling |
|---------|------|---------|
| Headlines | {{HEADLINE_FONT}} | Bold |
| UI/Navigation | {{UI_FONT}} | Semi-Bold |
| Body Copy | {{BODY_FONT}} | Regular |

## Key Paths

| Path | Purpose |
|------|---------|
| `{{MAIN_SOURCE_FILES}}` | Primary source files |
| `{{TEST_DIRECTORY}}/` | Test files |
| `{{ENV_FILE}}` | Environment secrets — never commit |
| `{{SENSITIVE_DIR}}/` | Sensitive files — excluded from Claude access |
| `specs/` | SDD spec artifacts |
| `.claude/` | Harness configuration |
