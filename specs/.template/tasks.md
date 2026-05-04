# Tasks: [FEATURE NAME]

> **Based on**: `specs/features/[category]/[feature-name]/requirements.md` + `specs/features/[category]/[feature-name]/design.md`
> **Created**: [DATE]
> **Status**: Draft

---

## Test Tasks (TDD)

> These test tasks must be executed BEFORE implementation tasks. Each test task is paired with a corresponding implementation task.

### TEST Task 1: Test [Feature/Module]
- **Description**: Generate test file for [module/component]
- **Test file**: `{{TEST_FILE_PATTERN}}`
- **Definition of Done**:
  - [ ] Test file exists in `{{TEST_DIRECTORY}}/`
  - [ ] Test suites cover all affected modules per design.md
  - [ ] Tests follow RED pattern (will fail until implementation)
  - [ ] `test_history.log` documents spec-to-test mapping
- **Corresponds to**: Implementation Task 1
- **Dependencies**: None

### TEST Task 2: Test [Feature/Module]
- **Description**: ...
- **Test file**: `{{TEST_FILE_PATTERN}}`
- **Definition of Done**:
  - [ ] ...
- **Corresponds to**: Implementation Task N
- **Dependencies**: TEST Task 1

---

## Implementation Plan

### Task 1: [Title]
- **Description**: What exactly needs to be built/changed
- **Files to touch**: `path/to/file`
- **Definition of Done**:
  - [ ] Code implements the behavior described in design.md
  - [ ] All corresponding TEST tests PASS
  - [ ] No deviations from spec logic
  - [ ] Follows project coding standards (CLAUDE.md)
- **Dependencies**: None / TEST Task N

### Task 2: [Title]
- **Description**: ...
- **Files to touch**: ...
- **Definition of Done**:
  - [ ] Code implements the behavior described in design.md
  - [ ] All corresponding TEST tests PASS
  - [ ] No deviations from spec logic
  - [ ] Follows project coding standards (CLAUDE.md)
- **Dependencies**: Task N / TEST Task N

### Task 3: [Title]
- **Description**: ...
- **Files to touch**: ...
- **Definition of Done**:
  - [ ] Code implements the behavior described in design.md
  - [ ] All corresponding TEST tests PASS
  - [ ] No deviations from spec logic
  - [ ] Follows project coding standards (CLAUDE.md)
- **Dependencies**: Task N / TEST Task N

---

## Human Review Checkpoints

| Checkpoint | After Task | What to Verify |
|------------|-----------|----------------|
| 1 | N | Layout / visual correctness |
| 2 | N | Functionality / interaction |
| 3 | All | End-to-end flow against success metrics |

---

_Execute tasks in order. Each task = one logical commit. Halt if implementation deviates from spec/design._
