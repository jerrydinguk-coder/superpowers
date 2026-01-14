# PRD: Artifact-Driven SDD Workflow

## 1. User Stories

### Story 1: Organized Context
**As a** Developer (or AI Agent),
**I want** all documentation for a specific feature to be in one predictable place,
**So that** I don't have to hunt through `docs/plans`, `docs/specs`, and chat logs to find relevant info.

### Story 2: Persistent Planning
**As a** Project Manager,
**I want** the implementation plan to be stored with the feature docs,
**So that** I can see exactly how the feature was broken down and executed.

### Story 3: Proof of Testing
**As a** QA Engineer,
**I want** to see a `test-plan.md` outlining coverage *before* code is written, and a `test-report.log` *after*,
**So that** I have proof that the feature works as intended and edge cases were considered.

### Story 4: Formalized Review
**As a** Lead Developer,
**I want** code review feedback to be saved in `cr-report.md`,
**So that** there is a permanent record of the review process and the approval decision.

## 2. Functional Requirements

### FR1: Feature-Based Directory Structure
*   The system must enforce a `docs/features/<feature-key>/` structure.
*   The system must support the following file types in this directory:
    *   `business.md`
    *   `prd.md`
    *   `tech-design.md`
    *   `implementation-plan.md`
    *   `test-plan.md`
    *   `test-report.log`
    *   `cr-report.md`

### FR2: Skill Workflow Updates
*   **Brainstorming:** Must produce `business.md`, `prd.md`, `tech-design.md`.
*   **Writing Plans:** Must produce `implementation-plan.md` (and prompt for `test-plan.md`).
*   **SDD Execution:** Must verify `test-plan.md`, append to `test-report.log`, and write `cr-report.md`.
*   **Finishing:** Must validate existence of all artifacts.

## 3. Acceptance Criteria

*   [ ] `superpowers:brainstorming` prompts for a `feature-key`.
*   [ ] `superpowers:brainstorming` creates the folder and the first 3 docs.
*   [ ] `superpowers:subagent-driven-development` fails/warns if `test-plan.md` is missing.
*   [ ] Subagents successfully append test output to `test-report.log`.
*   [ ] Subagents successfully write structured reviews to `cr-report.md`.
*   [ ] `superpowers:finishing-a-development-branch` blocks finish if artifacts are missing.
