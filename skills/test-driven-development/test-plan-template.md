# Test Plan: [Feature Name]

**Feature Key:** `<feature-key>`
**Created:** YYYY-MM-DD
**Last Updated:** YYYY-MM-DD
**Owner:** [Name/Team]

## Overview

Brief description of what this feature does and why we're testing it.

## Test Strategy

- **Scope:** What will be tested
- **Out of Scope:** What won't be tested
- **Test Types:** Unit, Integration, E2E
- **Risk Level:** Low/Medium/High

## Test Cases

| ID | Category | Scenario | Test Type | Priority | Acceptance Criteria | Dependencies | Edge Cases |
|:---|:---------|:---------|:----------|:---------|:--------------------|:-------------|:-----------|
| TC-001 | [Auth] | User login with valid credentials | Integration | P0 | Returns 200, sets auth token in cookie, redirects to /dashboard | User exists in DB | - |
| TC-002 | [Auth] | User login with invalid password | Integration | P0 | Returns 401, shows "Invalid credentials" error | User exists in DB | Multiple failed attempts |
| TC-003 | [Auth] | User login with non-existent email | Integration | P0 | Returns 401, shows "Invalid credentials" error | - | - |
| TC-004 | [Validation] | Empty email field | Unit | P1 | Returns 400, shows "Email required" | - | Whitespace-only input |
| TC-005 | [Validation] | Invalid email format | Unit | P1 | Returns 400, shows "Invalid email format" | - | Special chars, missing @ |
| TC-006 | [Security] | SQL injection attempt in email field | Integration | P0 | Input sanitized, returns 401 | - | Various SQL payloads |

## Requirements Traceability Matrix

Maps test cases back to requirements from `prd.md`.

| Test ID | Requirement ID | Requirement Description | Status |
|:--------|:---------------|:------------------------|:-------|
| TC-001 | REQ-AUTH-001 | User can login with valid credentials | Planned |
| TC-002 | REQ-AUTH-001 | User can login with valid credentials | Planned |
| TC-003 | REQ-AUTH-002 | System rejects invalid login attempts | Planned |
| TC-004 | REQ-VAL-001 | All inputs are validated | Planned |
| TC-005 | REQ-VAL-001 | All inputs are validated | Planned |
| TC-006 | REQ-SEC-001 | Prevent common security vulnerabilities | Planned |

## Test Classification

### By Priority
- **P0 (Critical):** 3 tests - Must pass before release
- **P1 (Important):** 2 tests - Should pass before release
- **P2 (Nice to have):** 1 test - Can be deferred if needed

### By Type
- **Unit Tests:** 2
- **Integration Tests:** 4
- **E2E Tests:** 0

### By Category
- **Auth:** 3 tests
- **Validation:** 2 tests
- **Security:** 1 test

## Test Environment Requirements

- Node.js >= 18.x
- Test database with seed data
- Mock email service
- Test user credentials: `test@example.com` / `TestPass123!`

## Known Limitations

- Email delivery not tested (mocked)
- Rate limiting not covered (separate test suite)

## Notes

Additional context, assumptions, or important information for implementers.
