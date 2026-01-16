# TDD Workflow Complete Example

This document demonstrates the complete TDD workflow using the enhanced test-plan and test-report templates.

## Scenario: Implementing User Authentication Feature

**Feature Key:** `user-auth`
**Branch:** `feature/user-auth`

---

## Phase 1: Test Planning (Before Any Code)

### Step 1: Create test-plan.md

Based on `@test-plan-template.md`, create `docs/features/user-auth/test-plan.md`:

```markdown
# Test Plan: User Authentication

**Feature Key:** `user-auth`
**Created:** 2026-01-16
**Last Updated:** 2026-01-16
**Owner:** Development Team

## Overview

Implement secure user authentication with email/password, including validation, error handling, and session management.

## Test Strategy

- **Scope:** Login, logout, session validation, input validation
- **Out of Scope:** OAuth, 2FA (future features)
- **Test Types:** Unit (validation), Integration (auth flow), E2E (full user journey)
- **Risk Level:** High (security-critical)

## Test Cases

| ID | Category | Scenario | Test Type | Priority | Acceptance Criteria | Dependencies | Edge Cases |
|:---|:---------|:---------|:----------|:---------|:--------------------|:-------------|:-----------|
| TC-001 | Auth | User login with valid credentials | Integration | P0 | Returns 200, sets auth token, redirects to /dashboard | User exists in DB | - |
| TC-002 | Auth | User login with invalid password | Integration | P0 | Returns 401, shows "Invalid credentials" | User exists in DB | Rate limiting after 5 attempts |
| TC-003 | Auth | User login with non-existent email | Integration | P0 | Returns 401, shows "Invalid credentials" | - | - |
| TC-004 | Validation | Empty email field | Unit | P1 | Returns 400, shows "Email required" | - | Whitespace-only input |
| TC-005 | Validation | Invalid email format | Unit | P1 | Returns 400, shows "Invalid email format" | - | Special chars, missing @ |
| TC-006 | Security | SQL injection attempt in email | Integration | P0 | Input sanitized, returns 401 | - | Various SQL payloads |
| TC-007 | Auth | User logout | Integration | P1 | Clears session, redirects to login | Active session exists | - |

## Requirements Traceability Matrix

| Test ID | Requirement ID | Requirement Description | Status |
|:--------|:---------------|:------------------------|:-------|
| TC-001 | REQ-AUTH-001 | User can login with valid credentials | Planned |
| TC-002 | REQ-AUTH-001 | User can login with valid credentials | Planned |
| TC-003 | REQ-AUTH-002 | System rejects invalid login attempts | Planned |
| TC-004 | REQ-VAL-001 | All inputs are validated before processing | Planned |
| TC-005 | REQ-VAL-001 | All inputs are validated before processing | Planned |
| TC-006 | REQ-SEC-001 | Prevent SQL injection attacks | Planned |
| TC-007 | REQ-AUTH-003 | User can logout and end session | Planned |

## Test Classification

### By Priority
- **P0 (Critical):** 4 tests - Must pass before release
- **P1 (Important):** 3 tests - Should pass before release

### By Type
- **Unit Tests:** 2
- **Integration Tests:** 5

### By Category
- **Auth:** 4 tests
- **Validation:** 2 tests
- **Security:** 1 test
```

---

## Phase 2: TDD Implementation (RED → GREEN)

### Task: Implement TC-001 (Valid Login)

#### RED Phase

**Step 1: Write the failing test**

Create `tests/auth.test.ts`:

```typescript
import { authenticateUser } from '../src/auth/authenticate';

describe('User Authentication', () => {
  test('TC-001: authenticates user with valid credentials', async () => {
    const credentials = {
      email: 'alice@example.com',
      password: 'SecurePass123!'
    };

    const result = await authenticateUser(credentials);

    expect(result.status).toBe(200);
    expect(result.token).toBeDefined();
    expect(result.user.email).toBe('alice@example.com');
  });
});
```

**Step 2: Run the test**

```bash
$ npm test tests/auth.test.ts

FAIL tests/auth.test.ts
  User Authentication
    ✕ TC-001: authenticates user with valid credentials (5ms)

  ● User Authentication › TC-001: authenticates user with valid credentials

    ReferenceError: authenticateUser is not defined

      5 |   test('TC-001: authenticates user with valid credentials', async () => {
      6 |     const credentials = { email: 'alice@example.com', password: 'SecurePass123!' };
    > 7 |     const result = await authenticateUser(credentials);
        |                          ^
      8 |     expect(result.status).toBe(200);

      at Object.<anonymous> (tests/auth.test.ts:7:26)
```

**✅ RED Evidence Captured:**
- Error: `ReferenceError: authenticateUser is not defined`
- File: `tests/auth.test.ts`
- Line: `7`

#### GREEN Phase

**Step 3: Write minimal implementation**

Create `src/auth/authenticate.ts`:

```typescript
import { hashPassword, verifyPassword } from './crypto';
import { findUserByEmail } from '../db/users';
import { generateToken } from './tokens';

export async function authenticateUser(credentials: {
  email: string;
  password: string;
}) {
  const user = await findUserByEmail(credentials.email);

  if (!user) {
    return { status: 401, error: 'Invalid credentials' };
  }

  const validPassword = await verifyPassword(credentials.password, user.passwordHash);

  if (!validPassword) {
    return { status: 401, error: 'Invalid credentials' };
  }

  const token = generateToken(user.id);

  return {
    status: 200,
    token,
    user: {
      id: user.id,
      email: user.email,
      name: user.name
    }
  };
}
```

**Step 4: Run the test again**

```bash
$ npm test tests/auth.test.ts

PASS tests/auth.test.ts (1.2s)
  User Authentication
    ✓ TC-001: authenticates user with valid credentials (45ms)

Test Suites: 1 passed, 1 total
Tests:       1 passed, 1 total

Coverage:
  src/auth/authenticate.ts: 95.2% statements, 88.9% branches
```

**✅ GREEN Evidence Captured:**
- Status: `PASS`
- Duration: `45ms`
- Coverage: `95.2% statements, 88.9% branches`

---

## Phase 3: Update test-report.md

Based on `@test-report-template.md`, update `docs/features/user-auth/test-report.md`:

```markdown
# Test Report: User Authentication

**Feature Key:** `user-auth`
**Test Execution Date:** 2026-01-16 14:30:00
**Executed By:** Development Team
**Environment:** Development

## Executive Summary

- **Total Test Cases:** 7 (planned)
- **Passed:** 1
- **Failed:** 0
- **In Progress:** 6
- **Code Coverage:** 95.2% lines, 88.9% branches
- **Total Execution Time:** 45ms
- **Status:** 🟡 In Progress (1/7 complete)

## TDD Evidence Table

| ID | Scenario | RED Evidence (Before Implementation) | GREEN Evidence (After Implementation) | Test Command | Duration |
|:---|:---------|:-------------------------------------|:--------------------------------------|:-------------|:---------|
| TC-001 | User login with valid credentials | ❌ `ReferenceError: authenticateUser is not defined` at `tests/auth.test.ts:7` | ✅ `PASS` All assertions passed. Token generated. Coverage: 95.2% | `npm test tests/auth.test.ts` | 45ms |
| TC-002 | User login with invalid password | (To be filled during RED phase) | (To be filled during GREEN phase) | - | - |
| TC-003 | User login with non-existent email | (To be filled during RED phase) | (To be filled during GREEN phase) | - | - |
| TC-004 | Empty email field | (To be filled during RED phase) | (To be filled during GREEN phase) | - | - |
| TC-005 | Invalid email format | (To be filled during RED phase) | (To be filled during GREEN phase) | - | - |
| TC-006 | SQL injection attempt | (To be filled during RED phase) | (To be filled during GREEN phase) | - | - |
| TC-007 | User logout | (To be filled during RED phase) | (To be filled during GREEN phase) | - | - |

## Detailed Test Results

### TC-001: User login with valid credentials

**RED Phase Output:**
```
FAIL tests/auth.test.ts
  ● User Authentication › TC-001: authenticates user with valid credentials

    ReferenceError: authenticateUser is not defined

      5 |   test('TC-001: authenticates user with valid credentials', async () => {
      6 |     const credentials = { email: 'alice@example.com', password: 'SecurePass123!' };
    > 7 |     const result = await authenticateUser(credentials);
        |                          ^
      8 |     expect(result.status).toBe(200);

      at Object.<anonymous> (tests/auth.test.ts:7:26)
```

**GREEN Phase Output:**
```
PASS tests/auth.test.ts (1.2s)
  User Authentication
    ✓ TC-001: authenticates user with valid credentials (45ms)

  Assertions:
    ✓ Status code: 200
    ✓ Token present and valid format
    ✓ User email matches input

  Coverage:
    - src/auth/authenticate.ts: 95.2% statements, 88.9% branches
```

## Requirements Coverage

| Requirement ID | Description | Test IDs | Status |
|:---------------|:------------|:---------|:-------|
| REQ-AUTH-001 | User can login with valid credentials | TC-001, TC-002 | 🟡 Partial (TC-001 ✅) |
| REQ-AUTH-002 | System rejects invalid login attempts | TC-003 | ⏳ Pending |
| REQ-VAL-001 | All inputs are validated | TC-004, TC-005 | ⏳ Pending |
| REQ-SEC-001 | Prevent SQL injection | TC-006 | ⏳ Pending |
| REQ-AUTH-003 | User can logout | TC-007 | ⏳ Pending |

## Code Coverage Report

```
File                        | % Stmts | % Branch | % Funcs | % Lines | Uncovered Lines
----------------------------|---------|----------|---------|---------|------------------
src/auth/                   |   95.20 |    88.90 |  100.00 |   95.20 |
  authenticate.ts           |   95.20 |    88.90 |  100.00 |   95.20 | 23-24
----------------------------|---------|----------|---------|---------|------------------
All files                   |   95.20 |    88.90 |  100.00 |   95.20 |
```

## Test Classification Summary

### By Priority
- **P0 (Critical):** 1/4 passed ✅
- **P1 (Important):** 0/3 passed ⏳

### By Type
- **Unit Tests:** 0/2 passed ⏳
- **Integration Tests:** 1/5 passed ✅

### By Category
- **Auth:** 1/4 passed ✅
- **Validation:** 0/2 passed ⏳
- **Security:** 0/1 passed ⏳

## TDD Compliance

✅ **VERIFIED** - TC-001 followed strict TDD:
- RED phase documented with failure evidence
- GREEN phase documented with pass evidence
- No test passed immediately (proving it tests new behavior)
```

---

## Phase 4: Continue with Remaining Tests

Repeat the RED → GREEN → Update Report cycle for TC-002 through TC-007.

After implementing **TC-002** (Invalid Password):

```markdown
| TC-002 | User login with invalid password | ❌ `AssertionError: Expected status 401, received undefined` at `tests/auth.test.ts:23` | ✅ `PASS` Status 401, error message matched | `npm test tests/auth.test.ts:18-28` | 23ms |
```

After implementing **TC-003** (Non-existent Email):

```markdown
| TC-003 | User login with non-existent email | ❌ `Error: Cannot read property 'passwordHash' of null` at `src/auth/authenticate.ts:12` | ✅ `PASS` Correctly returns 401 without exposing user existence | `npm test tests/auth.test.ts:30-40` | 28ms |
```

---

## Phase 5: Final Validation

When all tests are complete, the test-report.md should look like:

```markdown
## Executive Summary

- **Total Test Cases:** 7
- **Passed:** 7
- **Failed:** 0
- **Code Coverage:** 94.3% lines, 91.2% branches
- **Total Execution Time:** 342ms
- **Status:** ✅ All Tests Passing

## TDD Compliance

✅ **FULLY COMPLIANT**
- All 7 test cases have documented RED evidence
- All 7 test cases have documented GREEN evidence
- Test IDs match between test-plan.md and test-report.md
- All required sections present
- No test passed immediately without RED phase
```

---

## Benefits Demonstrated

### 1. **Complete Traceability**
- Every requirement (REQ-XXX) → Test case (TC-XXX) → Evidence (RED/GREEN)
- Can prove TDD was followed for every feature

### 2. **Audit Trail**
- Exact error messages from RED phase prove tests were written first
- Coverage metrics show quality of implementation
- Timeline shows when each test was implemented

### 3. **Regression Prevention**
- If TC-003 starts failing, we can see:
  - Original RED: `Cannot read property 'passwordHash' of null`
  - Original GREEN: `Correctly returns 401`
  - Something changed user lookup logic

### 4. **Documentation Quality**
- New team members can see exactly how feature was built
- Test cases serve as living documentation
- Requirements coverage matrix shows what's implemented

### 5. **Review Efficiency**
- Spec reviewer can verify TDD compliance automatically
- Code reviewer knows tests were written first
- No guesswork about test quality

---

## Common Pitfalls to Avoid

### ❌ Bad: Vague RED Evidence
```markdown
| TC-001 | Login test | Failed | Passed | npm test | 45ms |
```
**Problem:** No proof test was written first, no error details

### ✅ Good: Specific RED Evidence
```markdown
| TC-001 | User login valid | ❌ `ReferenceError: authenticateUser is not defined` at `auth.test.ts:7` | ✅ `PASS` All assertions passed | npm test | 45ms |
```

---

### ❌ Bad: Test IDs Don't Match
**test-plan.md:**
```markdown
| TC-001 | Login test | ... |
```

**test-report.md:**
```markdown
| Test-1 | Login test | ... |
```
**Problem:** Can't verify one-to-one mapping

### ✅ Good: Consistent Test IDs
Both files use `TC-001`, `TC-002`, etc.

---

### ❌ Bad: Missing Sections
test-report.md only has the evidence table, missing:
- Executive Summary
- Requirements Coverage
- Code Coverage Report

### ✅ Good: Complete Report
All sections from template filled out

---

## Quick Checklist

Before marking work complete, verify:

- [ ] test-plan.md exists with all Test IDs
- [ ] test-report.md exists with matching Test IDs
- [ ] Every Test ID has RED evidence (error message, file, line)
- [ ] Every Test ID has GREEN evidence (pass confirmation, duration)
- [ ] Requirements Traceability Matrix complete
- [ ] Code Coverage Report shows adequate coverage (>80%)
- [ ] Test Classification Summary matches test-plan.md
- [ ] No "TBD", "N/A", or empty cells in evidence columns

**If all checkboxes are checked:** TDD compliance verified ✅

**If any checkbox is unchecked:** TDD was violated, fix before proceeding ❌
