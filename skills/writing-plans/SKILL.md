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

**Save plans to:** `docs/features/<feature-key>/implementation-plan.md`

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

## Mandatory Task 0: Test Planning & Initialization

**The FIRST task in every plan MUST be:**

```markdown
### Task 0: Create Test Plan & Initialize Reports

**Files:**
- Create: `docs/features/<feature-key>/test-plan.md`
- Create: `docs/features/<feature-key>/test-report.md`
- Create: `docs/features/<feature-key>/cr-report.md`

**Description:**

1.  **Test Plan:** Use `@test-plan-template.md` as the base structure.
    - Read `docs/features/<feature-key>/prd.md` to extract requirements
    - Read `docs/features/<feature-key>/tech-design.md` to understand architecture
    - Create comprehensive test scenarios with:
      - **Unique Test IDs** (TC-001, TC-002, etc.)
      - **Categories** (Auth, Validation, Security, etc.)
      - **Test Types** (Unit, Integration, E2E)
      - **Priority Levels** (P0/P1/P2)
      - **Acceptance Criteria** (specific, measurable)
      - **Dependencies** (what must exist for test to run)
      - **Edge Cases** (boundary conditions, error states)
    - Include **Requirements Traceability Matrix** linking test cases to requirements
    - Include **Test Classification Summary** (by priority, type, category)

    **Required table format:**
    | ID | Category | Scenario | Test Type | Priority | Acceptance Criteria | Dependencies | Edge Cases |
    |:---|:---------|:---------|:----------|:---------|:--------------------|:-------------|:-----------|
    | TC-001 | Auth | User login with valid credentials | Integration | P0 | Returns 200, sets auth token | User exists in DB | - |

2.  **Test Report:** Use `@test-report-template.md` as the base structure.
    - Initialize with header section (Feature Key, Date, Environment)
    - Create TDD Evidence Table with ALL Test IDs from test-plan.md
    - Leave RED/GREEN evidence columns empty (to be filled during implementation)
    - Include sections for: Executive Summary, Detailed Results, Coverage Report, Classification Summary

    **Required table format:**
    | ID | Scenario | RED Evidence (Before Implementation) | GREEN Evidence (After Implementation) | Test Command | Duration |
    |:---|:---------|:-------------------------------------|:--------------------------------------|:-------------|:---------|
    | TC-001 | User login valid | (To be filled during RED phase) | (To be filled during GREEN phase) | - | - |

3.  **CR Report:** Create `cr-report.md` initialized with:
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
- test-plan.md contains all test scenarios with unique IDs
- test-plan.md includes Requirements Traceability Matrix
- test-report.md structure matches test-plan.md (same Test IDs)
- All three artifact files created and committed
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
