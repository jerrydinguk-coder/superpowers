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
    2. **Update Test Report:** You MUST update `docs/features/[feature-key]/test-report.md`.
       - **Initialization:** If the file doesn't exist, create it by copying the TABLE from `test-plan.md` and adding "RED Output" and "GREEN Output" columns.
       - **Update:** Fill in the RED/GREEN columns for the scenarios you just implemented (Match by ID). Do NOT just append text; update the table row for the specific ID.

    3. Commit your work
    4. Self-review (see below)
    5. Report back

    Work from: [directory]

    **Definition of Done:**
    - Code implemented
    - Tests passing
    - `test-report.md` table updated for relevant IDs

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
