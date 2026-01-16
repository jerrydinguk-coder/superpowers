---
name: finishing-a-development-branch
description: Use when implementation is complete, all tests pass, and you need to decide how to integrate the work - guides completion of development work by presenting structured options for merge, PR, or cleanup
---

# Finishing a Development Branch

## Overview

Guide completion of development work by presenting clear options and handling chosen workflow.

**Core principle:** Verify tests → Present options → Execute choice → Clean up.

**Announce at start:** "I'm using the finishing-a-development-branch skill to complete this work."

## The Process

### Step 0: Verify Documentation Artifacts

**Before anything else, verify the Feature Documentation exists and is complete.**

1.  **Identify Feature Key:** Extract from branch name (e.g., `feature/user-auth` -> `user-auth`) or ask user.

2.  **Check File Existence:**
    Run `ls docs/features/<feature-key>/` and verify ALL 7 files exist:
    *   `business.md`
    *   `prd.md`
    *   `tech-design.md`
    *   `implementation-plan.md`
    *   `test-plan.md`
    *   `test-report.md`
    *   `cr-report.md`

**If any are missing:**
```
Missing required artifacts in docs/features/<feature-key>/:
- [list missing files]

Cannot proceed. Please generate missing documentation.
```
Stop. Don't proceed.

3.  **Validate TDD Compliance (CRITICAL):**

    Read `test-plan.md` and `test-report.md` to verify TDD was followed:

    **a. Test ID Mapping:**
    - Extract all Test IDs from test-plan.md (e.g., TC-001, TC-002, etc.)
    - Verify EVERY Test ID appears in test-report.md
    - Verify test-report.md has no extra Test IDs not in test-plan.md

    **b. RED Evidence Validation:**
    For EVERY Test ID in test-report.md, verify "RED Evidence" column contains:
    - Error message or failure description
    - File name where failure occurred
    - Line number (if applicable)
    - NOT empty, NOT "N/A", NOT "TBD"

    **c. GREEN Evidence Validation:**
    For EVERY Test ID in test-report.md, verify "GREEN Evidence" column contains:
    - Pass confirmation (e.g., "PASS", "✅")
    - Assertions verified or test duration
    - NOT empty, NOT "N/A", NOT "TBD"

    **d. Required Sections:**
    Verify test-report.md includes:
    - [ ] Executive Summary (with totals, coverage %)
    - [ ] TDD Evidence Table (with RED/GREEN columns)
    - [ ] Detailed Test Results (for at least major tests)
    - [ ] Requirements Coverage (traceability to prd.md)
    - [ ] Code Coverage Report (statement/branch percentages)

**If TDD validation fails:**
```
TDD compliance validation FAILED in docs/features/<feature-key>/test-report.md:

Issues found:
- [List specific issues, e.g.:]
  - Test ID TC-003 missing RED evidence
  - Test ID TC-007 not found in test-report.md
  - Missing Executive Summary section
  - Code Coverage Report section empty

Cannot proceed. TDD was not properly followed.
All tests must have documented RED (failure) and GREEN (pass) evidence.
```
Stop. Don't proceed.

4.  **Validate Code Review:**
    Read `cr-report.md` and verify:
    - Contains "Status: PASS" or "Status: APPROVED"
    - If status is "FAIL" or "IN_PROGRESS", stop

**If code review not approved:**
```
Code review not approved in docs/features/<feature-key>/cr-report.md

Status: [current status]

Cannot proceed. Code review must be approved before finishing.
```
Stop. Don't proceed.

**If all validations pass:** Continue to Step 1.

### Step 1: Verify Tests

**Before presenting options, verify tests pass:**

```bash
# Run project's test suite
npm test / cargo test / pytest / go test ./...
```

**If tests fail:**
```
Tests failing (<N> failures). Must fix before completing:

[Show failures]

Cannot proceed with merge/PR until tests pass.
```

Stop. Don't proceed to Step 2.

**If tests pass:** Continue to Step 2.

### Step 2: Determine Base Branch

```bash
# Try common base branches
git merge-base HEAD main 2>/dev/null || git merge-base HEAD master 2>/dev/null
```

Or ask: "This branch split from main - is that correct?"

### Step 3: Present Options

Present exactly these 4 options:

```
Implementation complete. What would you like to do?

1. Merge back to <base-branch> locally
2. Push and create a Pull Request
3. Keep the branch as-is (I'll handle it later)
4. Discard this work

Which option?
```

**Don't add explanation** - keep options concise.

### Step 4: Execute Choice

**MANDATORY: For Options 1, 2, and 4, you MUST preview commands and get confirmation.**

#### Option 1: Merge Locally

**1. Preview Commands:**
```
I will run the following commands to merge locally:

git checkout <base-branch>
git pull
git merge <feature-branch>
<test command>
git branch -d <feature-branch>
git worktree remove <worktree-path>

Run these commands? (y/n)
```

**2. Wait for "y".**

**3. Execute:**
```bash
# Switch to base branch
git checkout <base-branch>
... (run the previewed commands)
```

#### Option 2: Push and Create PR

**1. Preview Commands:**
```
I will run the following commands to create a PR:

git push -u origin <feature-branch>
gh pr create --title "<title>" ...
git worktree remove <worktree-path>

Run these commands? (y/n)
```

**2. Wait for "y".**

**3. Execute:**
```bash
# Push branch
git push -u origin <feature-branch>
... (run the previewed commands)
```

#### Option 3: Keep As-Is

Report: "Keeping branch <name>. Worktree preserved at <path>."

**Don't cleanup worktree.**

#### Option 4: Discard

**1. Preview Commands:**
```
I will run the following commands to PERMANENTLY DISCARD this work:

git checkout <base-branch>
git branch -D <feature-branch>
git worktree remove <worktree-path>

Type 'discard' to confirm.
```

**2. Wait for exact "discard" confirmation.**

**3. Execute:**
```bash
git checkout <base-branch>
... (run the previewed commands)
```

### Step 5: Cleanup Worktree

**For Options 1, 2, 4:**

Check if in worktree:
```bash
git worktree list | grep $(git branch --show-current)
```

If yes:
```bash
git worktree remove <worktree-path>
```

**For Option 3:** Keep worktree.

## Quick Reference

| Option | Merge | Push | Keep Worktree | Cleanup Branch |
|--------|-------|------|---------------|----------------|
| 1. Merge locally | ✓ | - | - | ✓ |
| 2. Create PR | - | ✓ | ✓ | - |
| 3. Keep as-is | - | - | ✓ | - |
| 4. Discard | - | - | - | ✓ (force) |

## Common Mistakes

**Skipping test verification**
- **Problem:** Merge broken code, create failing PR
- **Fix:** Always verify tests before offering options

**Open-ended questions**
- **Problem:** "What should I do next?" → ambiguous
- **Fix:** Present exactly 4 structured options

**Automatic worktree cleanup**
- **Problem:** Remove worktree when might need it (Option 2, 3)
- **Fix:** Only cleanup for Options 1 and 4

**No confirmation for discard**
- **Problem:** Accidentally delete work
- **Fix:** Require typed "discard" confirmation

## Red Flags

**Never:**
- Proceed with failing tests
- Merge without verifying tests on result
- Delete work without confirmation
- Force-push without explicit request

**Always:**
- Verify tests before offering options
- Present exactly 4 options
- Get typed confirmation for Option 4
- Clean up worktree for Options 1 & 4 only

## Integration

**Called by:**
- **subagent-driven-development** (Step 7) - After all tasks complete
- **executing-plans** (Step 5) - After all batches complete

**Pairs with:**
- **using-git-worktrees** - Cleans up worktree created by that skill
