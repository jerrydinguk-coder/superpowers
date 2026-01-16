# Spec Compliance Reviewer Prompt Template

Use this template when dispatching a spec compliance reviewer subagent.

**Purpose:** Verify implementer built what was requested (nothing more, nothing less)

```
Task tool (general-purpose):
  description: "Review spec compliance for Task N"
  prompt: |
    You are reviewing whether an implementation matches its specification.

    ## What Was Requested

    [FULL TEXT of task requirements]

    ## What Implementer Claims They Built

    [From implementer's report]

    ## CRITICAL: Do Not Trust the Report

    The implementer finished suspiciously quickly. Their report may be incomplete,
    inaccurate, or optimistic. You MUST verify everything independently.

    **DO NOT:**
    - Take their word for what they implemented
    - Trust their claims about completeness
    - Accept their interpretation of requirements

    **DO:**
    - Read the actual code they wrote
    - Compare actual implementation to requirements line by line
    - Check for missing pieces they claimed to implement
    - Look for extra features they didn't mention

    ## Your Job

    ### Part 1: Verify TDD Compliance (CRITICAL - Check First)

    Before reviewing code, verify TDD was properly followed:

    **Step 1: Check test-plan.md exists**
    - Path: `docs/features/[feature-key]/test-plan.md`
    - If missing: FAIL immediately

    **Step 2: Check test-report.md was updated**
    - Path: `docs/features/[feature-key]/test-report.md`
    - Identify which Test IDs (TC-XXX) are relevant to this task

    **Step 3: Validate Test ID Mapping**
    For each Test ID relevant to this task:
    - [ ] Test ID exists in test-plan.md
    - [ ] Same Test ID exists in test-report.md
    - [ ] Test IDs match exactly (TC-001, not TC-1 or Test1)

    **Step 4: Validate RED Evidence (MANDATORY)**
    For EVERY relevant Test ID, verify "RED Evidence" column contains:
    - [ ] Error message or failure description (NOT empty)
    - [ ] File name where test failed
    - [ ] Line number (if applicable)
    - [ ] NOT "N/A", NOT "TBD", NOT "(To be filled)"

    **If RED evidence is missing or incomplete:**
    ```
    ❌ TDD VIOLATION: Test [TC-XXX] missing proper RED evidence

    Found: [what's actually in the column]
    Required: Error message, file name, line number

    TDD requires watching the test FAIL before implementing.
    Missing RED evidence means implementer violated TDD.
    ```

    **Step 5: Validate GREEN Evidence**
    For EVERY relevant Test ID, verify "GREEN Evidence" column contains:
    - [ ] Pass confirmation (e.g., "PASS", "✅")
    - [ ] Test duration or assertions verified
    - [ ] NOT empty, NOT "N/A", NOT "TBD"

    **Step 6: Validate Required Sections**
    Check test-report.md includes:
    - [ ] Executive Summary (with test counts, coverage %)
    - [ ] TDD Evidence Table (with RED/GREEN columns filled)
    - [ ] Requirements Coverage (links to prd.md)

    **TDD Compliance Result:**
    - If ALL checks pass: Continue to Part 2 (Code Review)
    - If ANY check fails: STOP and report TDD violation

    ### Part 2: Verify Spec Compliance

    Read the implementation code and verify:

    **Missing requirements:**
    - Did they implement everything that was requested?
    - Are there requirements they skipped or missed?
    - Did they claim something works but didn't actually implement it?

    **Extra/unneeded work:**
    - Did they build things that weren't requested?
    - Did they over-engineer or add unnecessary features?
    - Did they add "nice to haves" that weren't in spec?

    **Misunderstandings:**
    - Did they interpret requirements differently than intended?
    - Did they solve the wrong problem?
    - Did they implement the right feature but wrong way?

    **Verify by reading code, not by trusting report.**

    ## Report Format

    **If TDD violations found:**
    ```
    ❌ SPEC REVIEW FAILED - TDD Not Followed

    TDD Violations:
    - [List each Test ID with missing/incomplete RED evidence]
    - [List missing sections in test-report.md]
    - [List Test ID mapping issues]

    Cannot proceed with spec compliance review until TDD evidence is documented.
    ```

    **If TDD compliant but spec issues found:**
    ```
    ✅ TDD Compliant (all tests have RED/GREEN evidence)
    ❌ Spec Issues Found:

    Missing:
    - [list specifically what's missing, with file:line references]

    Extra/Unneeded:
    - [list what was added but not requested]

    Misunderstandings:
    - [list misinterpreted requirements]
    ```

    **If both TDD and spec compliant:**
    ```
    ✅ TDD Compliant (all tests have RED/GREEN evidence)
    ✅ Spec Compliant (all requirements met, nothing extra)
    ```
```
