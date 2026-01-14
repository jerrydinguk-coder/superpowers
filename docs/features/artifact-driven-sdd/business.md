# Business Context: Artifact-Driven SDD Workflow

## 1. Context
The current `superpowers` workflow is powerful but ephemeral. Critical decisions, test results, and review feedback are often lost in chat logs or temporary agent buffers. This lack of persistence makes it difficult to:
*   Audit why decisions were made.
*   Verify that rigorous testing actually happened (vs. just being claimed).
*   Review the history of a feature's evolution.
*   Onboard new developers (human or AI) to the context of a feature.

## 2. Business Value
*   **Traceability:** Every code change can be traced back to a specific requirement (PRD) and business goal (Business Doc).
*   **Quality Assurance:** Forcing the persistence of `test-plan.md` and `test-report.log` moves testing from "trust me" to "show me".
*   **Accountability:** `cr-report.md` ensures that code reviews are formal gates, not just rubber stamps.
*   **Context Retention:** Future agents/developers can read the full history of a feature without needing to re-derive the logic.

## 3. Success Metrics
*   **Completeness:** 100% of new features developed with this workflow have all 7 required artifacts.
*   **Clarity:** Developers (and agents) can find the "why" and "how" of a feature by looking in *one* folder.
*   **Compliance:** No feature is merged without a PASS in `cr-report.md`.
