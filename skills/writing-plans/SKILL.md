---
name: writing-plans
description: Use when you have a spec or requirements for a multi-step task, before touching code
---

# Writing Plans

## Overview

Write comprehensive implementation plans assuming the engineer has zero context for our codebase and questionable taste. Document everything they need to know: which files to touch for each task, code, testing, docs they might need to check, how to test it. Give them the whole plan as bite-sized tasks. DRY. YAGNI. TDD. Frequent commits.

Assume they are a skilled developer, but know almost nothing about our toolset or problem domain. Assume they don't know good test design very well.

**Announce at start:** "I'm using the writing-plans skill to create the implementation plan."

**Context:** This should be run in a dedicated worktree (created by brainstorming skill).

**Input:**
- Ask for the `feature-key` (e.g., `user-auth`) to locate the PRD and Tech Design.
- Read `docs/features/<feature-key>/prd.md` and `docs/features/<feature-key>/tech-design.md` for context.

**Output:**
- `docs/features/<feature-key>/test-plan.md` (created FIRST, before planning)
- `docs/features/<feature-key>/implementation-plan.md` (created SECOND, references test-plan.md)

## The Planning Process

### Step 1: Create Test Plan (BEFORE Implementation Plan)

**MANDATORY:** Before writing any implementation plan, create the test plan.

**Process:**
1. **Read requirements:**
   - Read `docs/features/<feature-key>/prd.md` to extract all requirements
   - Read `docs/features/<feature-key>/tech-design.md` to understand technical architecture
   - Identify all user scenarios, edge cases, and error conditions

2. **Create test-plan.md:**
   - Use `@test-plan-template.md` as the base structure
   - Create file: `docs/features/<feature-key>/test-plan.md`

3. **Fill in comprehensive test scenarios:**
   - **Unique Test IDs:** TC-001, TC-002, etc. (will be referenced in implementation plan)
   - **Categories:** Group tests logically (Auth, Validation, Security, etc.)
   - **Test Types:** Unit, Integration, E2E
   - **Priority Levels:** P0 (critical), P1 (important), P2 (nice-to-have)
   - **Acceptance Criteria:** Specific, measurable outcomes
   - **Dependencies:** What must exist for test to run
   - **Edge Cases:** Boundary conditions, error states

4. **Add Requirements Traceability Matrix:**
   - Link each Test ID to Requirement IDs from prd.md
   - Ensure every requirement has at least one test

5. **Add Test Classification Summary:**
   - By Priority (P0/P1/P2 distribution)
   - By Type (Unit/Integration/E2E distribution)
   - By Category (feature area distribution)

**Example test-plan.md table:**
```markdown
| ID | Category | Scenario | Test Type | Priority | Acceptance Criteria | Dependencies | Edge Cases |
|:---|:---------|:---------|:----------|:---------|:--------------------|:-------------|:-----------|
| TC-001 | Auth | User login with valid credentials | Integration | P0 | Returns 200, sets auth token | User exists in DB | - |
| TC-002 | Auth | User login with invalid password | Integration | P0 | Returns 401, shows error | User exists in DB | Rate limiting |
```

**Why test-plan.md comes first:**
- Test scenarios guide what needs to be implemented
- Implementation tasks can reference specific Test IDs
- Ensures test-first thinking from the start
- Prevents scope creep (only implement what's tested)

### Step 2: Write Implementation Plan (AFTER Test Plan)

**Now that test-plan.md exists, write the implementation plan.**

**Save to:** `docs/features/<feature-key>/implementation-plan.md`

## Bite-Sized Task Granularity

**Each step is one action (2-5 minutes):**
- "Write the failing test" - step
- "Run it to make sure it fails" - step
- "Implement the minimal code to make the test pass" - step
- "Run the tests and make sure they pass" - step
- "Commit" - step

## Plan Document Header

**Every plan MUST start with this header:**

```markdown
# [Feature Name] Implementation Plan

> **For Claude:** REQUIRED SUB-SKILL: Use superpowers:executing-plans to implement this plan task-by-task.

**Goal:** [One sentence describing what this builds]

**Architecture:** [2-3 sentences about approach]

**Tech Stack:** [Key technologies/libraries]

---
```

## Mandatory Task 0: Initialize Test Reports

**The FIRST task in every implementation plan MUST be:**

**NOTE:** test-plan.md already exists (created during planning phase). Task 0 only initializes the tracking/reporting files.

```markdown
### Task 0: Initialize Test Reports

**Context:** test-plan.md was created during planning and already contains all Test IDs and scenarios.

**Files:**
- Read: `docs/features/<feature-key>/test-plan.md` (already exists)
- Create: `docs/features/<feature-key>/test-report.md`
- Create: `docs/features/<feature-key>/cr-report.md`

**Description:**

1.  **Test Report:** Use `@test-report-template.md` as the base structure.
    - Initialize with header section (Feature Key, Date, Environment)
    - **CRITICAL:** Read test-plan.md and copy ALL Test IDs
    - Create TDD Evidence Table with exact same Test IDs from test-plan.md
    - Leave RED/GREEN evidence columns empty (to be filled during implementation)
    - Include sections for: Executive Summary, Detailed Results, Coverage Report, Classification Summary

    **Required table format (Test IDs must match test-plan.md):**
    ```markdown
    | ID | Scenario | RED Evidence (Before Implementation) | GREEN Evidence (After Implementation) | Test Command | Duration |
    |:---|:---------|:-------------------------------------|:--------------------------------------|:-------------|:---------|
    | TC-001 | User login valid | (To be filled during RED phase) | (To be filled during GREEN phase) | - | - |
    | TC-002 | Invalid password | (To be filled during RED phase) | (To be filled during GREEN phase) | - | - |
    ```

2.  **CR Report:** Create `cr-report.md` initialized with:
    ```markdown
    # Code Review Report

    **Feature Key:** <feature-key>
    **Status:** IN_PROGRESS
    **Reviewer:** TBD
    **Date:** TBD

    ## Review Checklist
    - [ ] All requirements implemented
    - [ ] TDD followed (RED/GREEN evidence in test-report.md)
    - [ ] Code quality standards met
    - [ ] Test coverage adequate
    - [ ] No security vulnerabilities

    ## Findings
    (To be filled by reviewer)
    ```

**Acceptance Criteria:**
- test-report.md Test IDs exactly match test-plan.md Test IDs
- test-report.md structure ready for RED/GREEN evidence
- cr-report.md initialized with checklist
- All files committed
```

## Task Structure

```markdown
### Task N: [Component Name]

**Files:**
- Create: `exact/path/to/file.py`
- Modify: `exact/path/to/existing.py:123-145`
- Test: `tests/exact/path/to/test.py`

**Step 1: Write the failing test**

```python
def test_specific_behavior():
    result = function(input)
    assert result == expected
```

**Step 2: Run test to verify it fails**

Run: `pytest tests/path/test.py::test_name -v`
Expected: FAIL with "function not defined"

**Step 3: Write minimal implementation**

```python
def function(input):
    return expected
```

**Step 4: Run test to verify it passes**

Run: `pytest tests/path/test.py::test_name -v`
Expected: PASS

**Step 5: Commit**

```bash
git add tests/path/test.py src/path/file.py
git commit -m "feat: add specific feature"
```
```

## Remember
- Exact file paths always
- Complete code in plan (not "add validation")
- Exact commands with expected output
- Reference relevant skills with @ syntax
- DRY, YAGNI, TDD, frequent commits

## Execution Handoff

After saving the plan, offer execution choice:

**"Plan complete and saved to `docs/features/<feature-key>/implementation-plan.md`. Two execution options:**

**1. Subagent-Driven (this session)** - I dispatch fresh subagent per task, review between tasks, fast iteration

**2. Parallel Session (separate)** - Open new session with executing-plans, batch execution with checkpoints

**Which approach?"**

**If Subagent-Driven chosen:**
- **REQUIRED SUB-SKILL:** Use superpowers:subagent-driven-development
- **IMPORTANT:** Pass the `feature-key` to the skill so it can find the artifacts.
- Stay in this session
- Fresh subagent per task + code review

**If Parallel Session chosen:**
- Guide them to open new session in worktree
- **REQUIRED SUB-SKILL:** New session uses superpowers:executing-plans
