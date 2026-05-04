# Skill Registry

Compact registry of all skills. Skills load on demand via trigger patterns.

Harness structure: `.claude/context_inner/` (inner — knowledge), `.claude/constraints_middle/` (middle — guides), `.claude/controls_outer/` (outer — sensors), `.claude/handoff_boundary/` (boundary — human-in-the-loop).

| Skill | Description | Trigger |
|-------|-------------|---------|
| caveman | Compressed communication (lite/full/ultra/wenyan) | "caveman", "less tokens", /caveman |
| git-commit-push | Commit/push with AI attribution, type(scope): subject | "commit", "push", "git commit" |
| playwright-cli | Browser automation, testing via Playwright CLI | "playwright", "browser test" , "use browser"|
