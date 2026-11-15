# Book 10: Unit Testing: Principles, Practices, and Patterns
**Author:** Vladimir Khorikov  
**Core Domain:** Test Quality, Test Double Taxonomy, Avoiding Brittle Tests & Architecture Alignment  
**Key Themes:** The Four Pillars of a Good Test, Mocks vs. Stubs, Classical vs. London School, Resilient Refactoring.

---

## 1. Executive Summary & Core Thesis
Khorikov frames unit testing around sustainable changes. Tests gain value by catching real regressions, supporting refactoring, providing fast feedback, and remaining maintainable. The publisher's contents cover test anatomy in Chapter 3, test quality in Chapter 4, and mocks and test fragility in Chapter 5. [Manning publisher contents](https://livebook.manning.com/learn/unit-testing)

---

## 2. The Four Pillars of a Good Unit Test

These four attributes help evaluate the quality of a test:

```text
       [ The 4 Pillars of a Good Unit Test ]
                          |
   +--------------+---------------+--------------+----------------+
   |              |               |              |                |
[ 1. REGRESSION ] [ 2. REFACTOR ] [ 3. FAST      ] [ 4. EASY      ]
[ PROTECTION   ] [ RESISTANCE  ] [ FEEDBACK     ] [ TO MAINTAIN  ]
Catches real     Zero false     Executes in      Clear intent,
bugs when code   positives when ms; runs on      low cognitive
changes.         refactoring.   every save.      setup overhead.
```

### The Most Important Pillar: Resistance to Refactoring
* A **brittle test** fails when you refactor the internal code structure, even though the application's observable behavior is 100% correct.
* **The "Crying Wolf" Catastrophe:** When brittle tests fail continuously during safe refactoring, developers lose trust in the test suite. They start blindly updating test assertions without investigating, allowing real bugs to slip into production unnoticed.

---

## 3. Mocks vs. Stubs: The Strict Taxonomy

One of the greatest sources of brittle tests is the misuse of test doubles. Khorikov establishes an unambiguous distinction based on the direction of communication with the System Under Test (SUT):

| Test Double | Direction of Communication | Role | Assertion Rule |
| :--- | :--- | :--- | :--- |
| **STUB** | **Incoming** (Inputs to SUT) | Feeds data into the SUT (e.g., canned DB query, fake config). | **NEVER assert interactions with stubs!** Assert only the final output or state. Asserting that a stub method was called binds your test to internal implementation details. |
| **MOCK** | **Outgoing** (Side effects from SUT) | Emulates external side effects (e.g., sending an email, kicking a cash drawer). | **Assert interactions!** Verify that the SUT produced the expected external command or message. |

```typescript
// BAD: Brittle test asserting an incoming stub call
const stubRepo = mock<OrderRepository>();
stubRepo.getById.mockReturnValue(order);
sut.calculateTotal(orderId);
expect(stubRepo.getById).toHaveBeenCalledTimes(1); // WRONG! Implementation detail.

// GOOD: Testing observable behavior
const result = sut.calculateTotal(orderId);
expect(result.amount).toBe(15000); // CORRECT! Tests the business outcome.
```

---

## 4. Classical (Detroit) vs. London (Mockist) Schools
* **London School:** Mocks every collaborator of a class. Results in extreme test fragmentation and extreme brittleness.
* **Classical School (Khorikov’s Recommendation):**
  - Use real domain objects together; test a cohesive unit of **behavior**, not a single arbitrary class.
  - Mock **only** out-of-process dependencies that represent unmanaged external systems (e.g., SMS gateways, credit card terminals, external tax APIs).
  - Never mock in-process domain collaborators. Managed dependencies (a database only your app uses) are tested for real in integration tests, not mocked.

---

## 5. Testing Observable Behavior vs. Implementation Details
* **Observable Behavior:** Public operations that achieve a client goal, or publicly visible state that reflects that goal.
* **Implementation details:** Private helpers and intermediate calls are poor assertion targets when callers depend only on the result. If a test breaks when those details change, express the assertion in terms of the behavior that needs coverage.
* **Naming:** Describe the scenario and observable result in domain language. Khorikov explains the problem with rigid templates in his article about the material covered by §3.4. [Khorikov's original explanation](https://khorikov.org/posts/2021-02-25-test-naming-conventions/)

---

## 6. Actionable Rules for our Craftsmanship Skill
1. **The Stub Invariant:** Never write an assertion (`expect().toHaveBeenCalled()`) on a stub. Assert only on domain return values, state mutations, or outward mock side effects.
2. **The Observable Behavior Rule:** Tests must verify what the system achieves for the user/caller, never how it achieves it internally.
3. **The Unmanaged Mock Rule:** Mock only unmanaged out-of-process dependencies (external APIs, message buses visible to other systems). Keep unit tests free of the database by testing pure domain logic with real objects; exercise the database and other managed dependencies in integration tests with the real thing.
