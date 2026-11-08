# Unit Testing & TDD Quality Checklist

Use this checklist to ensure that automated tests provide maximum regression protection with zero brittleness during refactoring.

---

## 1. Test Architecture & Structure (Vladimir Khorikov)
- [ ] **Arrange-Act-Assert (AAA):** Is the test clearly partitioned into Arrange (setup), Act (execution), and Assert (verification) sections?
- [ ] **Observable Behavior:** Does the test verify what the system achieves for the user/client, rather than asserting on internal private methods?
- [ ] **Single Concept per Test:** Does each test verify exactly one logical business scenario or outcome?
- [ ] **Meaningful Test Naming:** Does the name describe the scenario and expected behavior in plain language a non-programmer could follow? (Khorikov advises against rigid naming templates such as `[Unit]_[Scenario]_[Result]`.)

---

## 2. Test Doubles Discipline (Mocks vs. Stubs)
- [ ] **Stub Invariant:** Are Stubs used strictly to feed incoming data into the SUT?
- [ ] **Zero Stub Assertions:** Is the test free of interaction assertions on stubs (`expect(stubRepo.find).toHaveBeenCalled()`)?
- [ ] **Mock Discipline:** Are Mocks used exclusively to verify outgoing external side effects (e.g., dispatching an email or sending a webhook)?
- [ ] **No In-Memory Mocking:** Are domain objects, value objects, and internal calculation classes tested as real objects rather than mocked?

---

## 3. The Four Pillars Evaluation
- [ ] **Protection Against Regression:** Does the test exercise core domain complexity and edge cases where bugs could actually occur?
- [ ] **Resistance to Refactoring:** Can the internal implementation of the class be completely rewritten without this test breaking?
- [ ] **Fast Feedback:** Does this unit test run quickly (milliseconds) without disk or network I/O?
- [ ] **Maintainability:** Is the test code simple, readable, and free of complex control flow logic (`if`, `for`, `try/catch` inside tests are forbidden)?
