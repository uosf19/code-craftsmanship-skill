# Book 04: Code Complete (2nd Edition)
**Author:** Steve McConnell  
**Core Domain:** Software Construction, Defensive Programming, Routine Design & Variable Lifecycle  
**Key Themes:** Managing Cognitive Complexity, Variable Scoping, Barricades, Table-Driven Methods.

---

## 1. Executive Summary & Core Philosophy
Software construction is the central activity of software development. McConnell establishes that the primary technical imperative in software construction is **managing complexity**. Because human working memory is severely limited (Miller's Law: $7 \pm 2$ chunks of information), code must be structured so that a programmer never has to keep more than a few details in their head at any single moment.

---

## 2. Core Methodologies & Construction Standards

### A. Routine Design & Logical Cohesion (تصميم الإجراءات والتماسك المنطقي)
* **Functional Cohesion (The Gold Standard):** A routine possesses functional cohesion when it performs one and only one operation (e.g., `calculateSalesTax()`, `formatFiscalReceipt()`).
* **Cohesion Anti-Patterns:**
  - *Coincidental Cohesion:* Stuffing unrelated routines into a generic `Utils` or `Helper` class.
  - *Logical Cohesion:* Passing a control flag to execute one of 5 different operations inside a big `switch`.
* **Routine length.** The claim about routines of roughly 200 lines is unverified. The preview identifies §7.4 as the discussion of routine length. **Derived guidance.** Review cohesion and readability when a routine is hard to summarize or includes unrelated work. [Code Complete, 2nd edition, §7.4 preview](https://www.oreilly.com/library/view/code-complete-2nd/0735619670/ch07s04.html)

### B. Working with Variables: Lifecycle & Scoping (إدارة دورة حياة المتغيرات)
1. **Minimize "Live Time" & "Span":**
   - **Span:** The number of lines between successive references to a variable.
   - **Live Time:** Total lines between its first reference and its final reference.
   - *Rule:* Declare and initialize variables immediately before their first use. Keeping span and live time short dramatically reduces mental context tracking and bugs.
2. **Principle of Least Privilege:** Keep scope as narrow as possible. Global variables and class-level mutable state should be aggressively avoided.

### C. Defensive Programming & Barricades (البرمجة الدفاعية وحواجز الحماية)
* **The Philosophy:** Software will be subjected to bad data and rogue states. A robust routine never allows corrupt input to compromise downstream execution.
* **The Barricade Pattern (حواجز الحماية المعمارية):**
  - Partition the application into public "untrusted" zones and internal "trusted" zones.
  - Data crossing the public boundary (HTTP requests, files, user inputs, external webhooks) must be rigorously validated, sanitized, and transformed at the **Barricade Gate**.
  - Internal routines behind the barricade can operate with high performance, trusting that data passed to them is already valid.

```text
[ External World ] ---> ( Untrusted Data )
                               |
                   [ BARRICADE GATEWAY ]  <-- Sanitize, Validate, Enforce Schemas
                               |
[ Core Domain ]    <--- ( Sanitized, Trusted Data Only )
```

### D. Table-Driven Methods (البرمجة الموجهة بالجداول)
* A table lookup can clarify a conditional that maps inputs to data or uniform actions. Keep explicit branches when they display a closed set of cases or enforce exhaustiveness more clearly.

---

## 3. Managing Cyclomatic Complexity
* **Cyclomatic Complexity:** The number of independent execution paths through a routine (driven by `if`, `while`, `for`, `case`, `&&`, `||`).
* **Derived review threshold.** Around 10 independent paths is a prompt to inspect branching and test coverage. A parser, generated code, or an exhaustive mapping may justify retaining the structure.

---

## 4. Actionable Rules for our Craftsmanship Skill
1. **The Barricade Rule:** Every boundary accepting external data validates its inputs before forwarding to domain services; internal code need not repeat the same input validation, but should use assertions for conditions that indicate bugs, and domain objects still enforce their own business invariants.
2. **The Variable Locality Rule:** Never declare a variable at the top of a method if it is not used until line 30. Declare it on line 29.
3. **Complexity (Derived).** Examine the decisions together before extracting helpers or introducing a data table. Explain material trade-offs during review.
