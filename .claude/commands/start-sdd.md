---
name: /start-sdd
description: Initialize SDD flow for a new feature — creates specs directory and requirements.md
---

## /start-sdd — Requirements Phase

You are initializing a Spec-Driven Development (SDD) flow. The user has provided a feature name or bug ID.

### Steps

1. **SDD Pre-Check** (feedforward gate — runs before any files are created):
   - Read `.knowledge_base/harness_engineering_reference/sdd_in_harness_concept.md` for the full decision framework.
   - Evaluate the problem across four dimensions using the user's description:

   | Dimension | Question | Signal |
   |-----------|----------|--------|
   | **Size** | How many files? How many lines of change? | < 2 files / < 30 lines → ad-hoc |
   | **Clarity** | Are requirements well-specified? Edge cases known? | Vague goals → exploratory first |
   | **Brownfield** | Does this conflict with existing architecture? | Conflict → architecture review |
   | **Business impact** | Does this touch pricing, CTAs, or live content? | Yes → escalate to human |

   - Apply the decision matrix and produce a verdict **before proceeding**:

   | Verdict | Condition | Action |
   |---------|-----------|--------|
   | **HALT — Ad-hoc** | Small (< 2 files, < 30 lines) | Stop. Tell the user SDD overhead exceeds the task. Suggest a direct fix. Do NOT create any files. Allow override if user explicitly confirms. |
   | **HALT — Clarify first** | Any + Low/None clarity | Stop. Ask targeted clarifying questions. Do NOT create any files until requirements are sufficiently specified. |
   | **ESCALATE** | Business impact (pricing, CTAs, content) | Stop. Flag for human decision before proceeding. |
   | **PROCEED — Full pipeline** | Medium + High clarity | Continue to step 2. Note: standard SDD applies. |
   | **PROCEED — Exploratory design** | Medium + Medium clarity | Continue to step 2. Note: add a research step before `/plan-sdd`. |
   | **PROCEED — Phased SDD** | Large + High/Medium clarity | Continue to step 2. Note: split into sub-features during planning. |

   - State the verdict and mode explicitly before continuing. On HALT or ESCALATE, stop here.

2. **Validate inputs**: Determine type from command argument:
   - **Feature**: `/start-sdd feature [category] [name]` — e.g. `feature ui modal-close`
   - **Bugfix**: `/start-sdd bug [bug-id]` — e.g. `bug BUG-42`
3. **Create directory**:
   - Feature: `specs/features/<category>/<feature-name>/` (slugified, lowercase, hyphens)
   - Bugfix: `specs/bugs/<bug-id>/`
4. **Copy templates**: Copy all files from `specs/.template/` into the target directory.
5. **Generate `requirements.md`**:
   - Read `CLAUDE.md` for project context (palette, typography, structure, security rules, coding standards).
   - Read the current `{{ENTRY_POINT_FILE}}` to understand existing sections.
   - For **features**: fill in `requirements.md` with concrete content:
     - **Business Logic**: specific flows, user journeys, affected sections
     - **Edge Cases**: realistic edge cases for this specific feature
     - **Performance Requirements**: tied to project's Core Web Vitals / asset standards
     - **Success Metrics**: quantifiable acceptance criteria
   - For **bugfixes**: fill in `requirements.md` with:
     - **Bug Description**: what is broken, where, and how it manifests
     - **Reproduction Steps**: exact steps to reproduce
     - **Expected Behavior**: what should happen
     - **Actual Behavior**: what currently happens
     - **Edge Cases**: related edge cases (e.g. does it break under certain conditions?)
     - **Success Metrics**: how we verify the fix works
6. **Mutation testing opt-in** (sensor feedback):
   - Ask the user: "Mutation testing is computationally expensive. Enable for this run? (Y/n)"
   - If YES (default): write `specs/features/<cat>/<name>/mutate.md` (or `specs/bugs/<id>/mutate.md`):
     ```markdown
     ---
     enabled: true
     ---

     # Mutation Testing Configuration

     Mutation testing is enabled for this feature. Run `/mutate` after `/execute-sdd` to harden the test suite.

     **Thresholds**: 80% high, 60% low, 40% break. Survived mutants below 60% must be fixed.
     ```
   - If NO: write `specs/features/<cat>/<name>/mutate.md` with `enabled: false`.
   - **Recommendation**: Enable for features touching core business logic, public-facing JS, or security-sensitive code. Skip for minor UI tweaks, CSS-only changes, or documentation updates.
7. **Confirm**: Report what was created, the mutation testing decision, and the full path. The user can now run `/design-sdd`.

### Rules
- Never skip reading `CLAUDE.md` — all output must conform to project standards.
- Do NOT write any implementation code. Requirements only.
- If the feature touches multiple sections, note each in Business Logic.
- Ask the user for clarification if the provided description is vague and lacks sufficient detail.
