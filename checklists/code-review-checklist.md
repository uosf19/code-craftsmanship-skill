# Master Code Review Checklist

Use this checklist during peer reviews, automated PR gates, and self-reviews to guarantee high craftsmanship standards before merging.

---

## 1. Naming & Readability (Uncle Bob & Ousterhout)
- [ ] **Intention-Revealing:** Can a junior engineer understand what each variable, function, and class does without reading the implementation?
- [ ] **No Disinformation:** Are names free from misleading type encodings (e.g., no `userList` for a Set, no `orderObj` for an entity)?
- [ ] **Boring Code Over Clever Code:** Is the implementation simple and predictable? Are obscure language tricks or premature optimizations avoided?
- [ ] **Single Level of Abstraction (SLAP):** Does each function operate at a uniform abstraction level without mixing high-level orchestration with low-level string/math plumbing?

---

## 2. Function & Method Design (McConnell & Martin)
- [ ] **Single Responsibility:** Does the function perform exactly one logical task?
- [ ] **Parameter Capping:** Does the function have 3 or fewer arguments? (If 4+, are they wrapped in a typed Parameter Object)?
- [ ] **Zero Flag Arguments:** Is the routine free from boolean control flags that fork execution into two completely different paths?
- [ ] **Command-Query Separation (CQS):** Does the function strictly either modify state or return a calculated value without mixing both?
- [ ] **Zero-Null Policy:** Does the method avoid returning `null` or accepting `null` parameters? Are empty arrays, Optionals, or Result types used instead?

---

## 3. Modularity & Complexity (Ousterhout & McConnell)
- [ ] **Deep Module Check:** Does this class/service provide a simple, clean interface that hides a substantial internal implementation?
- [ ] **Zero Pass-Through Methods:** Are there any useless methods that do nothing but pass arguments to an underlying service? (If so, eliminate them).
- [ ] **Cyclomatic Complexity $\le 10$:** Does any routine have more than 10 independent execution branches?
- [ ] **Pull Complexity Downwards:** Does the module resolve internal quirks and edge cases itself, rather than pushing errors and configurations up to the caller?

---

## 4. Defensive Perimeter & Hygiene (Hunt, Thomas & McConnell)
- [ ] **Barricade Pattern:** Are external inputs (HTTP bodies, query params, webhooks, files) strictly validated and sanitized before reaching the domain?
- [ ] **Crash Early:** If an internal business invariant or contract is breached, does the routine fail immediately with an explicit domain error rather than swallowing exceptions?
- [ ] **Boy Scout Rule:** Did the author leave the touched file cleaner than they found it (e.g., deleted dead code, renamed an ambiguous identifier)?
