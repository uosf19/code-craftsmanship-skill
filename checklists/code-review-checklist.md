# Master Code Review Checklist

Use the applicable checks during peer review or self-review. These are derived review defaults. Follow the package's conventions and the project's behavior contracts.

---

## 1. Naming & Readability (Uncle Bob & Ousterhout)
- [ ] **Intention-Revealing:** Can a junior engineer understand what each variable, function, and class does without reading the implementation?
- [ ] **No Disinformation:** Are names free from misleading type encodings (e.g., no `userList` for a Set, no `orderObj` for an entity)?
- [ ] **Boring Code Over Clever Code:** Is the implementation simple and predictable? Are obscure language tricks or premature optimizations avoided?
- [ ] **Single Level of Abstraction (SLAP):** Does each function operate at a uniform abstraction level without mixing high-level orchestration with low-level string/math plumbing?

---

## 2. Function & Method Design (McConnell & Martin)
- [ ] **Cohesion.** Does the function have a clear purpose? Can a caller understand a flag value at the call site?
- [ ] **Parameter count.** When a signature starts to obscure intent, would a typed parameter object capture a real concept?
- [ ] **Command–query separation.** Are ordinary queries side-effect free? Where a command changes state and returns its result, does it preserve the operation's atomicity, as in compare-and-set or `INSERT ... RETURNING`? Fluent builders can return the receiver.
- [ ] **Absence and failure.** Does the method use empty collections for empty results and follow the language's conventions for absence and errors? Does it preserve the wire contract for null, omission, and empty values?

---

## 3. Modularity & Complexity (Ousterhout & McConnell)
- [ ] **Deep Module Check:** Does this class/service provide a simple, clean interface that hides a substantial internal implementation?
- [ ] **Delegation.** Does a wrapper provide a meaningful boundary, such as an adapter or a stable API?
- [ ] **Complexity.** When branching makes behavior hard to follow or test, would a local exhaustive match, decision table, or shared implementation make it clearer?
- [ ] **Pull Complexity Downwards:** Does the module resolve internal quirks and edge cases itself, rather than pushing errors and configurations up to the caller?

---

## 4. Defensive Perimeter & Hygiene (Hunt, Thomas, McConnell & Martin)
- [ ] **Barricade Pattern:** Are external inputs (HTTP bodies, query params, webhooks, files) strictly validated and sanitized before reaching the domain?
- [ ] **Failure handling.** Does an invariant failure abort the affected operation with a visible outcome while preserving durable state? Are expected replays and recoverable failures handled according to the contract?
- [ ] **Refactoring scope.** Are structural edits and behavior changes distinguishable, with existing work preserved? Consider separate commits when authorized and useful for review.
- [ ] **Verification.** Does the [testing checklist](unit-testing-checklist.md) cover the changed guarantees at the right level?
