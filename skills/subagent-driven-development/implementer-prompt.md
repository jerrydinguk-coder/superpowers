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
    1. **TDD is MANDATORY.**
       - First, identify all scenarios from `docs/features/[feature-key]/test-plan.md` relevant to this task.
       - **Phase RED:** Write tests for these scenarios and RUN them. They MUST fail. **Record the failure message.**
       - **Phase GREEN:** Implement minimal code to make tests pass.
    2. **Detailed Test Report:** You MUST update `docs/features/[feature-key]/test-report.log` with a structured entry for this task.

    ### Test Report Required Format:
    ```markdown
    ## Task N: [Task Name] Execution Report
    **Time:** [Timestamp]

    ### 1. Scenario Coverage (Mapped from test-plan.md)
    | Scenario ID | Description | RED State (Failure) | GREEN State (Pass) |
    | :--- | :--- | :--- | :--- |
    | [ID] | [Short Desc] | [Actual Error Message] | [Execution Status/Output] |

    ### 2. Full Test Execution Evidence
    **Command:** `npm test path/to/test.ts`
    **Raw Output (Last 10 lines):**
    ```text
    [Paste actual terminal output here]
    ```
    ```

    3. Commit your work
    4. Self-review (see below)
    5. Report back

    Work from: [directory]

    **Definition of Done:**
    - Code implemented
    - Tests passing
    - `test-report.log` updated with passing results

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
