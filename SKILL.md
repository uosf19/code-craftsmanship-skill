---
name: code-craftsmanship
description: Master software craftsmanship, clean code, refactoring patterns, complexity management, and resilient test-driven engineering distilled from 10 foundational software engineering classics. Use when writing, reviewing, refactoring, or testing code across any language or stack to ensure maintainability, deep modularity, and zero-defect execution.
---

# Code Craftsmanship & Modern Software Engineering Skill

This skill is a universal, language-agnostic engineering playbook for writing clean, robust, deeply modular, and maintainable production code. It synthesizes foundational principles from 10 landmark software engineering classics into four actionable execution pillars: **Code Hygiene**, **Complexity Management**, **Disciplined Refactoring**, and **Resilient Testing**.

---

## The Four Craftsmanship Pillars

```text
       [ Code Craftsmanship & Software Engineering ]
                             |
   +----------------+----------------+----------------+----------------+
   |                |                |                |                |
[ PILLAR 1 ]     [ PILLAR 2 ]     [ PILLAR 3 ]     [ PILLAR 4 ]
Code Hygiene &   Complexity &     Disciplined      Resilient
Deep Modularity  Defensive Design Refactoring      Testing & TDD
(Clean Code +    (Philosophy of   (Fowler + Beck)  (Khorikov + Google)
 Pragmatic Prog)  Design + McConnell)
```

---

## Pillar 1: Code Hygiene & Deep Modularity
*Foundations: Clean Code (Martin), The Pragmatic Programmer (Thomas & Hunt), A Philosophy of Software Design (Ousterhout).*

### 1. Intention-Revealing Naming & Semantics
- **Rule:** A name must state *why it exists*, *what it does*, and *how it is used* without requiring the reader to inspect the implementation.
- **Scope-to-Length Proportionality:** The length of a variable name must be directly proportional to its lexical scope (e.g., `i` for a 3-line loop; `customerTaxIdentificationNumber` for a domain aggregate).
- **No Disinformation:** Never use type suffixes (`orderList`, `accountMap`) unless the structure is bound strictly to that concrete container type.

### 2. Deep Modules vs. Shallow Breakups
- **Rule: Pull Complexity Downwards.** Design modules, classes, and services to have a **minimal, concise public interface** that hides substantial internal implementation complexity.
- **Eliminate Shallow Wrappers:** Reject classes or methods that simply forward arguments to an underlying service without adding business value (Zero Pass-Through Methods).
- **Single Level of Abstraction Principle (SLAP):** A function must execute operations at a uniform conceptual level. Never mix high-level business workflow statements with low-level string/buffer manipulations.

### 3. Command-Query Separation (CQS)
- Methods must strictly either **mutate state** (Command) or **return state** (Query), never both.
- Methods that calculate values must be free of observable side effects.

### 4. The Zero-Null Invariant
- Never pass `null` into methods; never return `null` from methods.
- Return empty collections, typed Result/Optional wrappers, or domain Null Objects to eradicate `NullPointerException` clutter.

---

## Pillar 2: Complexity Management & Defensive Design
*Foundations: Code Complete (McConnell), A Philosophy of Software Design (Ousterhout), Software Engineering at Google (Winters et al.).*

### 1. The Barricade Pattern (حواجز الحماية)
- **Rule:** Partition the software into an untrusted external perimeter and a trusted internal domain core:
  ```text
  [ External World: HTTP, Webhooks, Files ] 
                      |
           [ BARRICADE GATEWAY ]  <-- Sanitize, Validate with Schemas (Zod/DTOs)
                      |
  [ Internal Domain: Trusted Execution ] <-- Enforce Invariants, Zero Duplicate Checks
  ```
- All data crossing public boundaries must be sanitized and converted into immutable Value Objects. Internal routines assume data validity.

### 2. Design by Contract (DbC) & Crash Early
- Routines declare explicit Preconditions, Postconditions, and Class Invariants.
- **Fail Fast:** If an internal business invariant or contract is breached, crash early. Never swallow exceptions with empty `catch` blocks or substitute fake fallback values that corrupt financial/inventory state.

### 3. Hyrum's Law Defense & Information Hiding
- *"All observable behaviors of your system will be depended on by somebody."*
- Expose the smallest possible API footprint. Hide ordering, internal ID generation algorithms, intermediate cache states, and raw error stack traces from external callers.

### 4. Complexity Thresholds
- **Cyclomatic Complexity:** Routines must maintain cyclomatic complexity $\le 10$. Any method exceeding 10 independent execution branches must be decomposed into Polymorphism or Table-Driven lookups.
- **Parameter Capping:** Routines are limited to a maximum of 3 parameters. Functions requiring 4+ arguments must encapsulate them into a strongly typed Parameter Object.

---

## Pillar 3: Disciplined Refactoring
*Foundations: Refactoring (Fowler), The Pragmatic Programmer (Thomas & Hunt), Modern Software Engineering (Farley).*

### 1. The Two-Hat Rule (قاعدة القبعتين)
- When touching code, wear only one hat at a time:
  - **Feature Hat:** Add new observable behavior; do not alter existing structure; tests change or expand.
  - **Refactoring Hat:** Improve internal design, names, and structure; zero observable behavior changes; tests must remain strictly green.
- **Never mix feature development and structural refactoring in the same Git commit.**

### 2. Code Smells Response Guide

| Detected Code Smell | Primary Diagnostic | Mandatory Refactoring Action |
| :--- | :--- | :--- |
| **Long Method / Large Class** | Method $>30$ lines or class with multiple responsibilities. | *Extract Function* or *Extract Class*. |
| **Data Clumps** | 3+ primitive fields always traveling together. | *Introduce Parameter Object* or *Value Object*. |
| **Feature Envy** | Method calls another object's getters continuously. | *Move Function* to the target object. |
| **Shotgun Surgery** | A single requirement change touches 10 files. | *Move Field/Method* to combine related concerns. |
| **Sprawling Switch/Ifs** | Large conditionals switching on entity type codes. | *Replace Conditional with Polymorphism / Strategy*. |

### 3. The Boy Scout Rule
- *"Always leave the campground cleaner than you found it."*
- Every pull request touching legacy code must perform at least one micro-refactoring (renaming an obscure variable, extracting a helper, or removing dead code).

---

## Pillar 4: Resilient Testing & TDD
*Foundations: Unit Testing: Principles, Practices, and Patterns (Khorikov), Test-Driven Development (Beck), Software Engineering at Google (Winters et al.).*

### 1. The Four Pillars of a Good Test
Evaluate every automated test against Khorikov’s non-negotiable criteria:
1. **Protection Against Regressions:** Catches real bugs when code changes.
2. **Resistance to Refactoring (The Keystone):** Emits zero false alarms when code is refactored without behavioral changes.
3. **Fast Feedback:** Executes in milliseconds; runs continuously in development loops.
4. **Maintainability:** Clear arrange-act-assert structure with low cognitive setup overhead.

### 2. Mocks vs. Stubs: The Golden Rule
- **STUBS (Incoming Data):** Stubs supply canned inputs to the System Under Test (SUT).
  - **Rule:** **NEVER assert interactions on a stub!** (e.g., `expect(repo.getById).toHaveBeenCalled()` is strictly forbidden). Assert only the SUT's final return value or state.
- **MOCKS (Outgoing Side Effects):** Mocks emulate unmanaged external side effects leaving the SUT (e.g., sending an SMS, emitting an outbox event).
  - **Rule:** **Assert interactions on mocks.** Verify that the side effect was dispatched with the correct payload.

### 3. Test Observable Behavior, Not Implementation Details
- Tests must verify **what** the code achieves for its caller, never **how** it achieves it internally.
- Never write unit tests for private methods directly; test them exclusively through public API behavior.

### 4. The Beyoncé Rule
- *"If you liked it, you should have put a test on it."*
- No bug fix or feature PR is accepted without an automated test that reproduces the failure (Red) and proves the fix (Green).

---

## Master Code Review Checklist

Run this checklist on every code review before merging:

```markdown
### 1. Hygiene & Naming
- [ ] Are all names intention-revealing, pronounceable, and free of type encodings?
- [ ] Does each function operate at a single level of abstraction (SLAP)?
- [ ] Is Command-Query Separation (CQS) maintained?
- [ ] Is the codebase free of naked `null` returns and parameters?

### 2. Deep Modularity & Complexity
- [ ] Is this module "Deep" (simple interface hiding substantial utility)?
- [ ] Is cyclomatic complexity $\le 10$ across all routines?
- [ ] Are routines restricted to $\le 3$ parameters (or wrapped in Parameter Objects)?
- [ ] Are external inputs sanitized at a Barricade before entering the domain?

### 3. Refactoring & Structure
- [ ] Does this commit wear only one hat (pure feature OR pure refactor)?
- [ ] Are code smells (Data Clumps, Feature Envy) eliminated?
- [ ] Did the developer leave the touched file cleaner than they found it?

### 4. Testing Integrity
- [ ] Do tests verify observable behavior rather than internal private methods?
- [ ] Are Stubs free of interaction assertions? (Assert state, not stub calls).
- [ ] Are tests fast, deterministic, and resistant to false alarms during refactoring?
```

---

## Detailed References Directory
Consult individual book summaries and deep chapter notes in the `references/` directory:
- [01. The Pragmatic Programmer (Thomas & Hunt)](file:///c:/Users/yusse/Documents/Projects/SaaS/docs/skills/code-craftsmanship/references/01-pragmatic-programmer.md)
- [02. Clean Code (Robert C. Martin)](file:///c:/Users/yusse/Documents/Projects/SaaS/docs/skills/code-craftsmanship/references/02-clean-code.md)
- [03. Refactoring (Martin Fowler)](file:///c:/Users/yusse/Documents/Projects/SaaS/docs/skills/code-craftsmanship/references/03-refactoring.md)
- [04. Code Complete (Steve McConnell)](file:///c:/Users/yusse/Documents/Projects/SaaS/docs/skills/code-craftsmanship/references/04-code-complete.md)
- [05. A Philosophy of Software Design (John Ousterhout)](file:///c:/Users/yusse/Documents/Projects/SaaS/docs/skills/code-craftsmanship/references/05-philosophy-of-software-design.md)
- [06. Modern Software Engineering (David Farley)](file:///c:/Users/yusse/Documents/Projects/SaaS/docs/skills/code-craftsmanship/references/06-modern-software-engineering.md)
- [07. Head First Design Patterns (Freeman & Robson)](file:///c:/Users/yusse/Documents/Projects/SaaS/docs/skills/code-craftsmanship/references/07-head-first-design-patterns.md)
- [08. Software Engineering at Google (Winters et al.)](file:///c:/Users/yusse/Documents/Projects/SaaS/docs/skills/code-craftsmanship/references/08-software-engineering-at-google.md)
- [09. Test-Driven Development: By Example (Kent Beck)](file:///c:/Users/yusse/Documents/Projects/SaaS/docs/skills/code-craftsmanship/references/09-test-driven-development.md)
- [10. Unit Testing: Principles, Practices, and Patterns (Khorikov)](file:///c:/Users/yusse/Documents/Projects/SaaS/docs/skills/code-craftsmanship/references/10-unit-testing-principles.md)
