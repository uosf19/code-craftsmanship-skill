# Disciplined Refactoring Checklist

Use these derived defaults when changing structure while preserving observable behavior.

---

## 1. Pre-Refactoring Safety Check
- [ ] **Baseline.** Run the tests relevant to the affected behavior and record pre-existing failures. Add characterization coverage if the current behavior lacks protection. If you cannot verify a high-risk change, narrow it or report the verification gap.
- [ ] **Scope.** Separate structural and behavior changes into reviewable steps. Preserve existing callers, serialized data, and effects unless the task calls for a behavior change.
- [ ] **Existing edits.** Inspect the affected files and current diff. Preserve user edits, including when they overlap the work.

---

## 2. Smell Identification & Transformation Plan
- [ ] **Long method.** Extract a cohesive operation if the caller becomes clearer and the boundary hides meaningful detail.
- [ ] **Data clump or primitive obsession.** Consider a value type when fields share an invariant, validation needs a single home, or the type prevents misuse.
- [ ] **Feature envy.** Consider moving behavior to the module that owns the invariant if it reduces coupling.
- [ ] **Repeated switches.** When the same decision changes in several places, consider a shared decision table or polymorphism. Keep a clear exhaustive switch or match when the variant set is closed and the branches are easier to understand together.

---

## 3. Micro-Step Execution
- [ ] **Small steps.** Keep each edit small enough to inspect and verify. Run focused checks after each meaningful step, then run the project's broader checks as appropriate.
- [ ] **Failure recovery.** Compare with the baseline. If an edit introduced a regression, correct it or undo only the changes from that step while preserving pre-existing work.
- [ ] **Version control.** Commit only with authorization. If separate commits aid review, keep structural edits in their own commit.
