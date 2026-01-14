# Design Document: Artifact-Driven SDD Workflow for Superpowers

## 1. Overview
This design upgrades the `superpowers` workflow to strictly enforce artifact generation at every stage of the development lifecycle. The goal is to ensure that every "node" in the workflow produces tangible documentation, increasing traceability, clarity, and quality.

## 2. Directory Structure
We will adopt a **Feature-based Folder** structure to keep all context localized.

**Pattern:** `docs/features/<feature-key>/`

**Required Artifacts per Feature:**
1.  `business.md` (Business context & value)
2.  `prd.md` (Product Requirements Document)
3.  `tech-design.md` (Technical Architecture & Design)
4.  `implementation-plan.md` (Step-by-step execution plan)
5.  `test-plan.md` (Planned test scenarios)
6.  `test-report.log` (Execution logs from test runs)
7.  `cr-report.md` (Code Review findings & approval)

## 3. Skill Modifications

### A. `superpowers:brainstorming`
*   **Input:** Prompt user for `feature-key` (kebab-case) at start.
*   **Process:**
    *   Guide user through defining Business Value, User Stories, and Technical approach.
    *   Explicitly separate these into the three distinct files (`business.md`, `prd.md`, `tech-design.md`).
*   **Exit Criteria:** All three files must exist in `docs/features/<feature-key>/`.

### B. `superpowers:writing-plans`
*   **Input:** Accept `feature-key`.
*   **Context:** Read `prd.md` and `tech-design.md` from the feature folder.
*   **Output:** Write `implementation-plan.md` to the feature folder.
*   **Logic:** Automatically insert "Create/Update `test-plan.md`" as a mandatory early task.

### C. `superpowers:subagent-driven-development`
*   **Input:** Accept `feature-key`.
*   **Pre-Flight:** Verify `test-plan.md` exists (or is the first task).
*   **Implementer Subagent:**
    *   **Mandate:** Execute tests and append output to `test-report.log`.
    *   **Definition of Done:** Code implemented + Tests passing + Log updated.
*   **Reviewer Subagent:**
    *   **Mandate:** Write structured review to `cr-report.md`.
    *   **Format:** Status (PASS/FAIL), Findings, Recommendations.
    *   **Definition of Done:** CR Report updated with PASS status.

### D. `superpowers:test-driven-development` (Reference)
*   **Update:** Document the requirement for `test-plan.md` (planning) vs `test-report.log` (proof).

### E. `superpowers:finishing-a-development-branch`
*   **Validation:** implementation of a "Documentation Completeness Check" that verifies all 6-7 artifacts exist before allowing the branch to be finished/merged.

## 4. Migration Strategy
*   This is a "breaking change" for the workflow.
*   Existing `docs/plans/` can remain for historical purposes.
*   New work will strictly follow the `docs/features/` pattern.

## 5. Next Steps
1.  Update `superpowers:brainstorming`.
2.  Update `superpowers:writing-plans`.
3.  Update `superpowers:subagent-driven-development` (and its prompt templates).
4.  Update `superpowers:finishing-a-development-branch`.
