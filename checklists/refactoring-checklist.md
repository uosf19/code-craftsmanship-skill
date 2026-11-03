# Disciplined Refactoring Checklist

Use this checklist whenever improving existing code to guarantee that internal structure is transformed without breaking external behavior.

---

## 1. Pre-Refactoring Safety Check
- [ ] **Green Test Baseline:** Does the target code have automated test coverage? Do all tests pass right now?
  - *If NO:* Write a characterization test verifying current observable behavior before touching any production code.
- [ ] **One-Hat Discipline:** Are you wearing strictly the Refactoring Hat? (No new features or bug fixes allowed in this pass).
- [ ] **Clean Working Directory:** Is Git working tree clean so you can revert instantly with `git checkout` if a refactor goes red?

---

## 2. Smell Identification & Transformation Plan
- [ ] **Long Method:** Target extracted into cohesive helper methods named after business intent.
- [ ] **Data Clump:** Recurring group of primitives ($3+$) converted into an immutable Value Object.
- [ ] **Feature Envy:** Method that frequently calls another class moved directly onto that class.
- [ ] **Switch Statement on Type:** Replaced with Polymorphic subclasses or Strategy pattern objects.
- [ ] **Primitive Obsession:** Domain concepts (Currency, Phone, Email, Tax ID) wrapped into dedicated Value Objects.

---

## 3. Micro-Step Execution
- [ ] **Atomic Step:** Make exactly one structural change at a time (e.g., extract one method or rename one variable).
- [ ] **Immediate Test Run:** Run test suite immediately after the change.
- [ ] **Revert on Failure:** If tests break or unexpected side effects emerge, revert to green immediately. Never debug inside an incomplete refactoring step.
- [ ] **Separate Commit:** Commit the refactoring change with a descriptive message (e.g., `refactor: extract calculateTaxFromLine from OrderService`).
