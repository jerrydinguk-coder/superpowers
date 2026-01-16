# Implementer Subagent Prompt Template

Use this template when dispatching an implementer subagent.

```
Task tool (general-purpose):
  description: "Implement Task N: [task name]"
  prompt: |
    You are implementing Task N: [task name]

    ## Task Description

    [FULL TEXT of task from plan - paste it here, don't make subagent read file]

    ## Context
    Feature Key: [feature-key]
    Documentation Path: docs/features/[feature-key]/

    [Scene-setting: where this fits, dependencies, architectural context]

    ## Before You Begin

    If you have questions about:
    - The requirements or acceptance criteria
    - The approach or implementation strategy
    - Dependencies or assumptions
    - Anything unclear in the task description

    **Ask them now.** Raise any concerns before starting work.

    ## Your Job

    Once you're clear on requirements:

    ### Step 1: Verify Test Plan Exists

    **CHECKPOINT 1:** Before writing ANY code, verify `docs/features/[feature-key]/test-plan.md` exists.
    - If it doesn't exist: STOP. Ask your controller to create it first.
    - If it exists: Read it completely to understand all test scenarios.

    ### Step 2: TDD Implementation (MANDATORY)

    **CHECKPOINT 2:** Identify relevant test cases from test-plan.md for this task.
    - Look for Test IDs (TC-001, TC-002, etc.) that apply to your task
    - Note their acceptance criteria, edge cases, and dependencies

    **Phase RED (Before Implementation):**
    1. Write test code for each relevant Test ID
    2. RUN each test - they MUST fail
    3. **CRITICAL:** Verify failure is expected (e.g., "function not defined", not a typo)
    4. **RECORD the exact failure:** Copy error message, file name, line number

    **Phase GREEN (Implementation):**
    1. Write minimal code to make tests pass
    2. RUN tests again - they must pass
    3. **RECORD the success:** Copy test output, duration, assertions passed

    ### Step 3: Update Test Report (MANDATORY)

    **CHECKPOINT 3:** Update `docs/features/[feature-key]/test-report.md`.

    **If test-report.md doesn't exist:**
    - Use the template from `skills/test-driven-development/test-report-template.md`
    - Fill in the header section (Feature Key, Date, Environment, etc.)
    - Create the TDD Evidence Table with ALL Test IDs from test-plan.md

    **Update Rules:**
    - Find the row with matching Test ID (e.g., TC-001)
    - Fill in the "RED Evidence" column with exact error message from RED phase
    - Fill in the "GREEN Evidence" column with success message from GREEN phase
    - Fill in "Test Command" and "Duration" columns
    - Update the "Executive Summary" section with current totals
    - Update the "Detailed Test Results" section for each test

    **CRITICAL VALIDATION:**
    - Every Test ID you implemented MUST have both RED and GREEN evidence
    - If a test passed immediately (no RED phase), you violated TDD - delete code and restart
    - Test IDs in test-report.md must match test-plan.md exactly (TC-001, not Test1 or TC-1)

    ### Step 4: Commit Your Work

    Commit message should reference Test IDs implemented (e.g., "Implement TC-001, TC-002: User authentication")

    ### Step 5: Self-Review

    **CHECKPOINT 4:** Before reporting back, verify:
    - [ ] test-plan.md exists and was read
    - [ ] All relevant Test IDs identified correctly
    - [ ] Every test has RED evidence recorded in test-report.md
    - [ ] Every test has GREEN evidence recorded in test-report.md
    - [ ] Test IDs match exactly between plan and report
    - [ ] Code coverage metrics updated in test-report.md
    - [ ] Executive Summary totals are accurate

    ### Step 6: Report Back

    Work from: [directory]

    **Definition of Done:**
    - Code implemented
    - Tests passing
    - `test-report.md` updated for ALL relevant Test IDs with RED + GREEN evidence
    - Test ID mapping verified (plan ↔ report)
    - No test passed immediately without RED phase

    **While you work:** If you encounter something unexpected or unclear, **ask questions**.
    It's always OK to pause and clarify. Don't guess or make assumptions.

    ## Before Reporting Back: Self-Review

    Review your work with fresh eyes. Ask yourself:

    **Completeness:**
    - Did I fully implement everything in the spec?
    - Did I miss any requirements?
    - Are there edge cases I didn't handle?

    **Quality:**
    - Is this my best work?
    - Are names clear and accurate (match what things do, not how they work)?
    - Is the code clean and maintainable?

    **Discipline:**
    - Did I avoid overbuilding (YAGNI)?
    - Did I only build what was requested?
    - Did I follow existing patterns in the codebase?

    **Testing:**
    - Do tests actually verify behavior (not just mock behavior)?
    - Did I follow TDD if required?
    - Are tests comprehensive?

    If you find issues during self-review, fix them now before reporting.

    ## Report Format

    When done, report:
    - What you implemented
    - What you tested and test results
    - Files changed
    - Self-review findings (if any)
    - Any issues or concerns
```
