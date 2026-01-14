# Implementation Plan: Artifact-Driven SDD Workflow

## Goal
Update the `superpowers` skill suite to strictly enforce the generation of documentation artifacts (`business.md`, `prd.md`, `tech-design.md`, `test-plan.md`, `cr-report.md`, etc.) within a feature-specific directory (`docs/features/<feature-key>/`).

## Pre-requisites
- [x] Brainstorming complete (`docs/features/artifact-driven-sdd/`)
- [x] PRD and Tech Design approved

## Tasks

### Phase 1: Brainstorming & Planning Skills

- [ ] **Task 1: Update `superpowers:brainstorming`**
    - [ ] Modify `SKILL.md` to prompt for `feature-key`.
    - [ ] Update instructions to create `docs/features/<feature-key>/` directory.
    - [ ] Update instructions to split design output into `business.md`, `prd.md`, and `tech-design.md`.
    - [ ] Add verification step to ensure files exist.
    - [ ] *Test:* Run brainstorming for a dummy feature to verify file creation.

- [ ] **Task 2: Update `superpowers:writing-plans`**
    - [ ] Modify `SKILL.md` to accept `feature-key`.
    - [ ] Update instructions to read context from `docs/features/<feature-key>/{prd,tech-design}.md`.
    - [ ] Update instructions to write the plan to `docs/features/<feature-key>/implementation-plan.md`.
    - [ ] Add logic to insert "Create `test-plan.md`" as the first actionable task in the generated plan.

### Phase 2: Execution & Verification Skills

- [ ] **Task 3: Update `superpowers:subagent-driven-development` (Controller)**
    - [ ] Modify `SKILL.md` to check for `test-plan.md` before dispatching implementers.
    - [ ] Update instructions to extract task text *and* pass the `feature-key` to subagents.

- [ ] **Task 4: Update Subagent Prompts (`implementer-prompt.md`)**
    - [ ] Update prompt to strictly enforce TDD.
    - [ ] Add instruction to append test execution results to `docs/features/<feature-key>/test-report.log`.
    - [ ] Add "Definition of Done" constraint: Code + Test Pass + Log Updated.

- [ ] **Task 5: Update Subagent Prompts (`code-quality-reviewer-prompt.md`)**
    - [ ] Update prompt to require writing output to `docs/features/<feature-key>/cr-report.md`.
    - [ ] Define the required Markdown format for the report.
    - [ ] Add constraint: Review status must be explicitly written in the report.

### Phase 3: Completion & Polish

- [ ] **Task 6: Update `superpowers:finishing-a-development-branch`**
    - [ ] Modify `SKILL.md` to scan `docs/features/<feature-key>/` for all 7 required artifacts.
    - [ ] Add blocking logic if any artifact is missing.

- [ ] **Task 7: Update `superpowers:test-driven-development`**
    - [ ] Update `SKILL.md` to reflect the new `test-plan.md` and `test-report.log` requirements.

## Verification Plan
1.  **Bootstrapping Test:** Use the *newly updated* skills to implement a small "dummy" feature (e.g., `feature-dummy-test`).
2.  **Artifact Check:** Verify `docs/features/feature-dummy-test/` contains all 7 files and they are populated correctly.
