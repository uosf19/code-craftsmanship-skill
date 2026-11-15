---
name: code-craftsmanship
description: Use when implementing new code, changing existing behavior, reviewing code, refactoring, or designing tests. Also use for naming, function design, defensive validation, code smells, test doubles, and TDD.
---

# Code Craftsmanship Skill

A language-agnostic playbook for readable, robust, maintainable code. The [README](README.md) lists the ten books that inform these study notes.

---

## Source Notes & Attribution

Material in this skill falls into three kinds:

1. **Book ideas and named techniques**, attributed to their book (Boy Scout Rule: Martin; Two Hats: Beck via Fowler; Barricades: McConnell; Deep Modules: Ousterhout; Mocks vs. Stubs: Khorikov).
2. **Industry conventions** (CQS from Meyer, Law of Demeter, cyclomatic complexity).
3. **Derived guidance** comprises team defaults, such as reviewing functions around 30 lines, and project applications, such as matching tests to the guarantees they exercise.

Follow the project's behavior contracts and the user's scope. Treat these heuristics as defaults and explain material trade-offs in review.

---

## How to Apply

1. Use Pillars 1–2 for writing code and apply the project's [language idioms](references/11-language-idioms.md). Use Pillar 3 and the [refactoring checklist](checklists/refactoring-checklist.md) for structural changes.
2. Use Pillar 4 and the [testing checklist](checklists/unit-testing-checklist.md) for verification, or the [code review checklist](checklists/code-review-checklist.md) for reviews.
3. Open the references for the affected topic as needed and explain material trade-offs.

---

## Pillar 1: Code Hygiene & Deep Modularity
[Clean Code](references/02-clean-code.md) for naming and functions. [A Philosophy of Software Design](references/05-philosophy-of-software-design.md) for deep modules and comments.

### 1. Intention-revealing names
- A name states why it exists, what it does, and how it is used.
- Name length grows with scope (`i` in a 3-line loop; a descriptive name for a domain concept).
- No misleading type suffixes (`orderList` for a Set).

### 2. Deep modules over shallow breakups
- Prefer a small interface hiding substantial implementation; pull complexity downward instead of pushing edge cases to callers.
- Keep delegation that provides a meaningful boundary, such as an adapter with a stable public contract.
- Keep one level of abstraction per function (Martin's SLAP idea).
- Review functions around 30 lines for cohesion (derived). Extract a helper when it makes the caller easier to understand.

### 3. Command-Query Separation (Meyer)
- Keep ordinary queries side-effect free.
- When a contract needs a mutation and its result together, keep them atomic (for example, `pop`, compare-and-set, or `INSERT ... RETURNING`). Fluent builders can return the receiver.

### 4. Null policy
- Prefer empty collections for valid empty results and an idiomatic, explicit representation for absence or failure. Preserve the external contract when null, omission, and an empty value carry distinct meanings.

### 5. Comments
- Martin treats most comments as a smell; Ousterhout argues good comments record what code cannot express (intent, invariants, why). Use comments for *why* and contracts, not to narrate *what*.

---

## Pillar 2: Complexity Management & Defensive Design
[Code Complete](references/04-code-complete.md) for barricades and routine design. [The Pragmatic Programmer](references/01-pragmatic-programmer.md) for contracts and fail-fast behavior. [Software Engineering at Google](references/08-software-engineering-at-google.md) for Hyrum's Law.

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
- On a programming error or broken invariant, abort the affected operation with a clear error and preserve prior durable state. For long-running or offline work, record the failure and let independent eligible work proceed.

### 3. Information hiding and Hyrum's Law
- Assume callers may come to depend on any observable behavior. Keep the API surface small and expose only the ordering and identifiers the contract needs.

### 4. Complexity thresholds (team defaults)
- Investigate cyclomatic complexity above roughly 10. Consider a table or polymorphism when either clarifies the decisions and preserves type or exhaustiveness checks.
- Review signatures around three parameters. A parameter object helps when its fields form a meaningful concept.

---

## Pillar 3: Disciplined Refactoring
[Refactoring](references/03-refactoring.md) for two hats and smells. [Modern Software Engineering](references/06-modern-software-engineering.md) for small increments and feedback. [Head First Design Patterns](references/07-head-first-design-patterns.md) for Strategy and other pattern choices.

### 1. Two hats (Beck, popularized by Fowler)
- Keep behavior changes and structural edits in separate steps. Preserve existing edits and use targeted verification. If commits are authorized, prefer separate commits when useful for review.

### 2. Smell → refactoring guide (Fowler's catalog names, 2nd edition)

| Smell | Signal | Candidate refactoring |
| :--- | :--- | :--- |
| Long Function / Large Class | Hard to name or summarize; several reasons to change | Extract Function / Extract Class |
| Data Clumps | Related values travel together and share meaning | Introduce Parameter Object / Value Object |
| Feature Envy | Method uses another object's data more than its own | Move Function |
| Shotgun Surgery | One change touches many places | Move Function/Field to combine concerns |
| Repeated Switches | Same branches must change in several places | Consider a shared decision table or polymorphism. Keep a clear exhaustive switch when it localizes the decision |
| Primitive Obsession | Raw strings/numbers for domain concepts | Replace Primitive with Object |

### 3. Boy Scout Rule (Martin, *Clean Code*)
- Make small, safe improvements in the touched area when they fit the user's scope.

---

## Pillar 4: Resilient Testing & TDD
[Unit Testing](references/10-unit-testing-principles.md) for test quality and doubles. [Test-Driven Development](references/09-test-driven-development.md) for Red–Green–Refactor.

### 1. Four attributes of a good test (Khorikov)
Protection against regressions; **resistance to refactoring** (no false alarms when behavior is unchanged); fast feedback; maintainability. Resistance to refactoring is the one you cannot trade away.

### 2. Mocks vs. stubs (Khorikov)
- **Stub** = incoming data. Never assert interactions on a stub; assert the result or state.
- **Mock** = outgoing side effect on an unmanaged dependency (email, message bus, external API). Assert the interaction.
- Do not mock in-process domain collaborators. Unit tests exercise domain logic without a database; use real databases in integration tests.

### 3. Observable behavior
- Test what the code achieves for its caller, not how. Do not test private methods directly.

### 4. TDD and the Beyoncé Rule
- Prefer Beck's Red–Green–Refactor for calculation-heavy logic when a useful failing test can express the next behavior. For a behavior-preserving edit, start with existing coverage or add characterization tests where there is a gap.
- Google's Beyoncé Rule says to automate checks for properties you need to preserve, including failure behavior. Add a regression case when it fills a concrete coverage gap. [Software Engineering at Google, chapter 11](https://abseil.io/resources/swe-book/html/ch11.html#the-beyonc%C3%A9-rule)

### 5. Match verification to the guarantee (Derived)
- **Unit and property tests:** Check exact rounding and allocation examples with real domain objects. Generate valid inputs to verify outcomes such as allocations summing exactly to the original amount. Inject clocks and ID generators when tests need repeatable expiry, retries, or replay.
- **Shared fixtures and compatibility:** Run the same versioned exact-output vectors in each language that implements the rule. Replay fixtures from supported older clients through current endpoints and upgrade paths, including a pending queue where relevant.
- **Database integration:** Use the deployed database engines in isolated test environments. Exercise atomic commit and rollback, concurrent duplicate commands, and tenant isolation using independent connections and the runtime roles.
- **Workflow recovery:** Drive the actual command and queue paths through a lost response after commit, restart or backup restore, and retry. Assert durable state and the visible result together.

---

## Conflicts & Trade-offs Between Sources

| Topic | One side | Other side | Guidance |
| :--- | :--- | :--- | :--- |
| Function size | Martin: tiny functions, one thing | Ousterhout: too many small functions create shallow modules | Split when it improves understanding; keep related logic together behind a deep interface |
| Comments | Martin: comments often signal failure | Ousterhout: comments capture design intent | Comment the why, not the what |
| TDD | Beck: test-first drives design | Ousterhout: can promote tactical programming | Prefer test-first for logic-dense changes and evaluate module boundaries during design |
| Validation | McConnell: validate at barricade | Domain design: objects enforce their own invariants | Syntax/format at the boundary; business invariants inside the domain |
| Fail fast and idempotent retries | Invalid operation or broken invariant calls for an explicit outcome | A valid retry after a lost response may refer to an already committed operation | Return the stored result for an exact replay. Handle changed payloads and unknown external outcomes according to the contract |
