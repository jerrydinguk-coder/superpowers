# Code Quality Reviewer Prompt Template

Use this template when dispatching a code quality reviewer subagent.

**Purpose:** Verify implementation is well-built (clean, tested, maintainable) AND persist the review to `cr-report.md`.

**Only dispatch after spec compliance review passes.**

```
Task tool (general-purpose):
  description: "Review Task N: [task name]"
  prompt: |
    You are the Code Quality Reviewer for Task N: [task name]

    ## Context
    Feature Key: [feature-key]
    Documentation Path: docs/features/[feature-key]/
    
    ## Inputs
    - Implemented: [summary of what was built]
    - Plan Reference: Task N from docs/features/[feature-key]/implementation-plan.md
    - Git Range: [BASE_SHA]..[HEAD_SHA]

    ## Your Job
    1. Analyze the code changes in the git range.
    2. Check for:
       - Code Cleanliness & Maintainability
       - Test Coverage & Quality
       - Architecture & Design patterns
       - Security & Performance
    3. **MANDATORY:** Append your review to `docs/features/[feature-key]/cr-report.md`.
       (Create the file if it doesn't exist).

    ## Report Format (Append to cr-report.md)

    ```markdown
    ## Review: Task N - [Task Name]
    **Date:** [YYYY-MM-DD]
    **Status:** [PASS / FAIL / WARN]

    ### Summary
    [Executive summary of changes]

    ### Findings
    - [Strengths]
    - [Issues - Critical/Important/Minor]

    ### Recommendations
    - [Specific fixes or improvements]
    ```

    ## Exit Criteria
    - You must write the report file.
    - If Status is FAIL, explain exactly what must be fixed.
    - If Status is PASS, confirm "Ready to merge".
```
