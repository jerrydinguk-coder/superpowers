# Test-Driven Development (TDD) - Complete Guide

This directory contains everything you need to practice strict, professional TDD in the Superpowers workflow.

---

## 📚 Documentation Structure

### Core Files

| File | Purpose | When to Use |
|:-----|:--------|:------------|
| **SKILL.md** | Main TDD skill definition | Read first to understand TDD principles |
| **QUICK-REFERENCE.md** | Quick reference guide (中文) | Daily use when implementing features |
| **TDD-WORKFLOW-EXAMPLE.md** | Complete example walkthrough | Learn by example, reference during implementation |
| **testing-anti-patterns.md** | Common mistakes to avoid | When adding mocks or test utilities |

### Templates

| File | Purpose | When to Use |
|:-----|:--------|:------------|
| **test-plan-template.md** | Test planning template | Before writing any code (Task 0) |
| **test-report-template.md** | Test report template | Initialize at start, update after each RED/GREEN cycle |

---

## 🚀 Quick Start

### 1. New Feature Implementation

**Step 1: During planning (writing-plans skill)**

The `writing-plans` skill will automatically create test-plan.md:

```bash
# writing-plans skill execution:
# 1. Reads prd.md and tech-design.md
# 2. Creates test-plan.md (BEFORE implementation-plan.md)
# 3. Creates implementation-plan.md (can reference Test IDs)
```

**Step 2: Fill in test scenarios (done by writing-plans agent)**
- Assign Test IDs (TC-001, TC-002, etc.)
- Define priorities (P0/P1/P2)
- Specify test types (Unit/Integration/E2E)
- Add Requirements Traceability Matrix

**Step 3: During implementation (Task 0)**

The first implementer subagent will initialize test-report.md:

```bash
# Implementer executes Task 0:
# 1. Reads test-plan.md (already exists)
# 2. Creates test-report.md (copies Test IDs from test-plan.md)
# 3. Creates cr-report.md
```

**Step 4: Start RED-GREEN-REFACTOR cycle**
- For each Test ID: Write test → Run (RED) → Implement → Run (GREEN) → Refactor
- Update test-report.md with evidence after each phase

---

## 📖 Learning Path

### For Beginners
1. Read **SKILL.md** sections:
   - "The Iron Law"
   - "Red-Green-Refactor"
   - "Why Order Matters"
2. Study **TDD-WORKFLOW-EXAMPLE.md** - complete user authentication example
3. Reference **QUICK-REFERENCE.md** while implementing your first feature

### For Experienced Developers
1. Skim **SKILL.md** for workflow-specific requirements
2. Use **QUICK-REFERENCE.md** as your daily reference
3. Check **testing-anti-patterns.md** when designing test utilities

### For Code Reviewers
1. Review **SKILL.md** → "Verification Checklist"
2. Use **QUICK-REFERENCE.md** → "关键要点总结"
3. Validate test-report.md has proper RED/GREEN evidence for all Test IDs

---

## 🎯 Key Concepts

### The Three Phases

#### Phase 1: Planning (BEFORE any code)
- **Input:** Requirements from `prd.md` and `tech-design.md`
- **Output:** `test-plan.md` with all Test IDs and scenarios
- **Template:** `test-plan-template.md`

#### Phase 2: RED → GREEN (TDD cycle)
- **RED:** Write test → Run → MUST fail → Record error
- **GREEN:** Write code → Run → MUST pass → Record success
- **Update:** `test-report.md` with evidence for each Test ID

#### Phase 3: Verification (Before merge)
- **Validate:** Every Test ID has RED + GREEN evidence
- **Check:** Requirements Traceability Matrix complete
- **Confirm:** Code coverage ≥ 80%

---

## 📋 Templates Overview

### test-plan-template.md

**Contains:**
- Test Cases table (ID, Category, Scenario, Type, Priority, Acceptance Criteria, Dependencies, Edge Cases)
- Requirements Traceability Matrix (links Test IDs to Requirement IDs)
- Test Classification Summary (by Priority, Type, Category)
- Test Strategy and Environment Requirements

**Key Features:**
- Comprehensive test scenario planning
- Built-in traceability to requirements
- Risk assessment and scope definition

### test-report-template.md

**Contains:**
- Executive Summary (totals, coverage, status)
- TDD Evidence Table (RED/GREEN evidence for each Test ID)
- Detailed Test Results (full test output)
- Requirements Coverage (which requirements are tested)
- Code Coverage Report (statement/branch/function metrics)
- Performance Benchmarks

**Key Features:**
- Strict Test ID mapping to test-plan.md
- Mandatory RED/GREEN evidence for TDD compliance
- Audit trail for every test execution

---

## ✅ Compliance Checklist

Before marking work complete, verify:

### Test Plan
- [ ] test-plan.md exists in `docs/features/<feature-key>/`
- [ ] All Test IDs follow format: TC-001, TC-002, etc.
- [ ] Test priorities assigned (P0/P1/P2)
- [ ] Test types specified (Unit/Integration/E2E)
- [ ] Requirements Traceability Matrix complete

### Test Report
- [ ] test-report.md exists in `docs/features/<feature-key>/`
- [ ] Every Test ID from test-plan.md appears in test-report.md
- [ ] Every Test ID has RED evidence (error message, file, line)
- [ ] Every Test ID has GREEN evidence (pass confirmation, duration)
- [ ] Executive Summary shows accurate totals
- [ ] Code Coverage Report shows ≥80% statement coverage
- [ ] No empty cells, "N/A", or "TBD" in evidence columns

### TDD Process
- [ ] Tests were written BEFORE implementation code
- [ ] Every test was watched to fail (RED phase)
- [ ] Failures were for correct reasons (feature missing, not typos)
- [ ] Minimal code written to pass each test
- [ ] All tests passing
- [ ] No warnings or errors in test output

**If all boxes checked:** ✅ TDD compliance verified

**If any box unchecked:** ❌ TDD violated - fix before proceeding

---

## 🔗 Integration with Other Skills

### This skill is used by:
- **writing-plans** (Task 0: Create test-plan.md)
- **subagent-driven-development** (Implementer creates/updates test-report.md)
- **finishing-a-development-branch** (Validates TDD compliance before merge)

### This skill references:
- **testing-anti-patterns** (Avoid common mistakes)
- **requesting-code-review** (Code review considers test quality)

### Related artifacts:
- `docs/features/<feature-key>/prd.md` (Source of requirements)
- `docs/features/<feature-key>/tech-design.md` (Technical context)
- `docs/features/<feature-key>/implementation-plan.md` (Execution plan)

---

## 💡 Pro Tips

### For Maximum Efficiency
1. **Fill test-plan.md thoroughly** - saves time during implementation
2. **Update test-report.md immediately** after RED/GREEN - don't batch updates
3. **Use Test IDs in commit messages** - e.g., "Implement TC-001, TC-002: User login"
4. **Reference test-plan.md during code review** - ensures nothing was missed

### For High Quality
1. **Write acceptance criteria clearly** - should be measurable and specific
2. **Include edge cases in test-plan.md** - forces you to think ahead
3. **Keep RED/GREEN evidence detailed** - proves TDD was followed
4. **Track regression tests** - mark which tests prevent known bugs

### For Collaboration
1. **Share test-plan.md early** - get feedback before coding
2. **Keep test-report.md updated** - shows progress to team
3. **Link Test IDs to Requirements** - helps product managers verify features
4. **Document performance benchmarks** - sets expectations for performance

---

## 🆘 Troubleshooting

### "Test passed immediately, no RED phase"
**Problem:** You're testing existing code, not new behavior
**Solution:** Delete implementation, start over with test-first

### "Can't find where to record RED evidence"
**Problem:** test-report.md not initialized properly
**Solution:** Copy structure from test-report-template.md, ensure Test IDs match test-plan.md

### "Test ID mapping doesn't match"
**Problem:** Test IDs in test-plan.md don't match test-report.md
**Solution:** Use exact same IDs (TC-001, not Test-1 or tc-001)

### "Missing Requirements Traceability Matrix"
**Problem:** Can't prove all requirements are tested
**Solution:** Add matrix to test-plan.md linking Test IDs to Requirement IDs from prd.md

### "Code coverage too low"
**Problem:** Coverage below 80% threshold
**Solution:** Add more test cases for uncovered branches/statements

---

## 📞 Getting Help

1. **Read QUICK-REFERENCE.md** - Answers most common questions
2. **Study TDD-WORKFLOW-EXAMPLE.md** - See how it's done correctly
3. **Check testing-anti-patterns.md** - Make sure you're not doing anti-patterns
4. **Review SKILL.md** - Understand the philosophy behind the rules

---

## 📝 Version History

- **v1.0.0** (2026-01-16) - Initial release with templates and comprehensive guides
  - Added test-plan-template.md and test-report-template.md
  - Added TDD-WORKFLOW-EXAMPLE.md with user authentication example
  - Added QUICK-REFERENCE.md (中文) for daily use
  - Enhanced SKILL.md with Artifact-Driven TDD phases

---

**Maintained by:** Superpowers Development Team
**Last Updated:** 2026-01-16
