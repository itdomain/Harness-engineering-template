# Governance

All security rules are in `.claude/constraints_middle/security-constraints.md` (web/app security) and `.claude/constraints_middle/llm-security-constraints.md` (LLM interaction security). Import both files for complete governance.

LLM security principles: external data wrapped in XML delimiters, instruction isolation, data normalization (HTML → markdown), source verification for knowledge updates, anomaly detection in model outputs, browser sandbox enforcement, MCP least-privilege permissions.
