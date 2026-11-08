---
name: code-craftsmanship
description: Code-quality playbook for clean code, deep modules, defensive design, refactoring, and unit testing, informed by 10 classic software engineering books. Use when the user asks to review, refactor, or write tests for existing code, or asks about naming, function design, code smells, test doubles, or TDD. Skip for pure architecture, infrastructure, or UI-design questions.
---

# Code Craftsmanship Skill

A language-agnostic playbook for readable, robust, maintainable code. It is **informed by** ten books (listed in `README.md`); the references are short study notes, not substitutes for the books.

---

## Source Notes & Attribution

Material in this skill falls into three kinds:

1. **Book ideas and named techniques**, attributed to their book (Boy Scout Rule: Martin; Two Hats: Beck via Fowler; Barricades: McConnell; Deep Modules: Ousterhout; Mocks vs. Stubs: Khorikov).
2. **Industry conventions** (CQS from Meyer, Law of Demeter, cyclomatic complexity).
3. **Team defaults derived for this skill**: numeric limits such as "3 parameters", "complexity 10", "15% refactoring time", "30-line functions". They are starting points, not claims from the books.

Rules below are **defaults**. Deviate when a documented reason exists (generated code, parsers, mapping tables, performance-critical paths) and say so in the review.

---

## How to Apply

1. **Classify the task**: writing/naming → Pillar 1; defensive design/complexity → Pillar 2; refactoring → Pillar 3 + `checklists/refactoring-checklist.md`; tests → Pillar 4 + `checklists/unit-testing-checklist.md`; code review → `checklists/code-review-checklist.md`.
2. **Read progressively**: open only the relevant reference from `references/`.
3. **Explain trade-offs** when two principles conflict (see Conflicts below).
4. **Run the matching checklist** before approving a change.

---

## Pillar 1: Code Hygiene & Deep Modularity
*Clean Code (Martin), The Pragmatic Programmer (Thomas & Hunt), A Philosophy of Software Design (Ousterhout).*

### 1. Intention-revealing names
- A name states why it exists, what it does, and how it is used.
- Name length grows with scope (`i` in a 3-line loop; a descriptive name for a domain concept).
- No misleading type suffixes (`orderList` for a Set).

### 2. Deep modules over shallow breakups
- Prefer a small interface hiding substantial implementation; pull complexity downward instead of pushing edge cases to callers.
- Reject pass-through methods that add no value.
- Keep one level of abstraction per function (Martin's SLAP idea).
- Function size is a smell signal, not a goal. Default: a function that does not fit on one screen (~30 lines, derived) deserves a look; do not split code into shallow fragments just to hit a number.

### 3. Command-Query Separation (Meyer)
- Default: a method either changes state or returns information.
- Accepted exceptions: operations that must be atomic (`pop`, compare-and-set, `INSERT ... RETURNING`), and fluent builders. Document them.

### 4. Null policy
- Default: do not return `null` for collections or "not found" cases; return empty collections, Optional/Result types, or Null Objects. Avoid passing `null` as an argument.
- Where the language has nullable types enforced by the compiler, follow the language idiom.

### 5. Comments
- Martin treats most comments as a smell; Ousterhout argues good comments record what code cannot express (intent, invariants, why). Use comments for *why* and contracts, not to narrate *what*.

---

## Pillar 2: Complexity Management & Defensive Design
*Code Complete (McConnell), The Pragmatic Programmer, A Philosophy of Software Design, Software Engineering at Google.*

### 1. Barricades (McConnell)
```text
[ External world: HTTP, files, webhooks ]
                  |
         [ BARRICADE ]  <-- validate, sanitize, convert to typed values
                  |
[ Internal code: trusted data ]  <-- assertions guard "should never happen"
```
- Validate external input once, at the boundary; do not repeat the same *input* validation in every internal function.
- Inside the barricade still use **assertions** for conditions that indicate bugs (McConnell). Domain invariants (e.g., an order cannot be modified once paid) are enforced by the domain objects themselves.

### 2. Design by Contract and Crash Early
- Routines state preconditions, postconditions, and invariants.
- On a *programming error or broken invariant*, fail fast: abort the current operation/transaction with a clear error. Never swallow exceptions with an empty `catch` or substitute fake values that corrupt money or stock.
- "Crash" means stop the faulty operation, not necessarily the whole process. Long-running or offline-first systems should quarantine the failed item and keep serving other work.

### 3. Information hiding and Hyrum's Law
- "All observable behaviors of your system will be depended on by somebody." Expose the smallest API surface; hide ordering, internal IDs, and raw stack traces.

### 4. Complexity thresholds (team defaults)
- Cyclomatic complexity: investigate above ~10; McConnell's bands (0-5, 6-10, 10+) are guidance. Prefer table-driven logic or polymorphism over deep branching.
- Parameters: Martin prefers 0-2 and treats 3 as needing justification; 4+ usually becomes a parameter object. Treat "3" as a soft ceiling.

---

## Pillar 3: Disciplined Refactoring
*Refactoring (Fowler), Test-Driven Development (Beck), Modern Software Engineering (Farley).*

### 1. Two hats (Beck, popularized by Fowler)
- Adding behavior and restructuring are different activities; switch hats deliberately. Keep them in **separate commits**. Refactoring commits keep tests green.

### 2. Smell → refactoring guide (Fowler's catalog names, 2nd edition)

| Smell | Signal | Typical refactoring |
| :--- | :--- | :--- |
| Long Function / Large Class | Hard to name or summarize; several reasons to change | Extract Function / Extract Class |
| Data Clumps | Same 3+ values travel together | Introduce Parameter Object / Value Object |
| Feature Envy | Method uses another object's data more than its own | Move Function |
| Shotgun Surgery | One change touches many places | Move Function/Field to combine concerns |
| Repeated Switches | Same type switch in several places | Replace Conditional with Polymorphism |
| Primitive Obsession | Raw strings/numbers for domain concepts | Replace Primitive with Object |

### 3. Boy Scout Rule (Martin, *Clean Code*)
- Leave the code a little cleaner than you found it. Make *small*, safe improvements in the area you touch; do not turn every PR into a cleanup project, and keep cleanup commits separate from behavior changes.

---

## Pillar 4: Resilient Testing & TDD
*Unit Testing (Khorikov), Test-Driven Development (Beck), Software Engineering at Google.*

### 1. Four attributes of a good test (Khorikov)
Protection against regressions; **resistance to refactoring** (no false alarms when behavior is unchanged); fast feedback; maintainability. Resistance to refactoring is the one you cannot trade away.

### 2. Mocks vs. stubs (Khorikov)
- **Stub** = incoming data. Never assert interactions on a stub; assert the result or state.
- **Mock** = outgoing side effect on an unmanaged dependency (email, message bus, external API). Assert the interaction.
- Do not mock in-process domain collaborators. Unit tests exercise domain logic without a database; use real databases in integration tests.

### 3. Observable behavior
- Test what the code achieves for its caller, not how. Do not test private methods directly.

### 4. TDD and the Beyoncé Rule
- Beck's Red-Green-Refactor is a strong default for calculation-heavy domain logic (pricing, tax, costing). It is a tool, not a law; Ousterhout argues it can steer toward tactical thinking, so pair it with design time.
- The Beyoncé Rule (*Software Engineering at Google*): if you liked a behavior, put a test on it; infrastructure owners are not responsible for breaking untested behavior. Team policy "every fix ships with a regression test" is a good practice derived from it.

---

## Conflicts & Trade-offs Between Sources

| Topic | One side | Other side | Guidance |
| :--- | :--- | :--- | :--- |
| Function size | Martin: tiny functions, one thing | Ousterhout: too many small functions create shallow modules | Split when it improves understanding; keep related logic together behind a deep interface |
| Comments | Martin: comments often signal failure | Ousterhout: comments capture design intent | Comment the why, not the what |
| TDD | Beck: test-first drives design | Ousterhout: can promote tactical programming | Use test-first for logic-dense code; always allocate design time |
| Validation | McConnell: validate at barricade | Domain design: objects enforce their own invariants | Syntax/format at the boundary; business invariants inside the domain |
| Crash early | Pragmatic Programmer | Offline/long-running systems must stay up | Abort the operation, quarantine, keep the process alive |

---

## Reference Directory
Notes per book are in `references/` (`01-pragmatic-programmer.md` through `10-unit-testing-principles.md`); checklists in `checklists/`. Run `checklists/code-review-checklist.md` for reviews instead of duplicating its items here.
