# Book 02: Clean Code: A Handbook of Agile Software Craftsmanship
**Author:** Robert C. Martin ("Uncle Bob")  
**Core Domain:** Code Readability, Function Design, Naming Conventions & Error Handling  
**Key Themes:** Meaningful Names, Single Responsibility Functions, Command-Query Separation, Clean Exceptions.

---

## 1. Executive Summary & Core Philosophy
The ratio of time spent reading code versus writing new code is well over 10 to 1. Making code readable makes the entire development lifecycle faster. Clean code reads like well-written prose: it is focused, direct, reveals its intent unambiguously, and does not obscure the designer's intent with speculative complexity.

---

## 2. Core Methodologies & Coding Standards

### A. Meaningful & Intention-Revealing Names (التسمية المعبرة)
* **Use Intention-Revealing Names:** A variable, function, or class name should answer three questions: *Why does it exist? What does it do? How is it used?*
  ```typescript
  // Bad: Obscure, requires mental translation
  const d: number = 86400; // elapsed time in seconds
  const list1: Order[] = getThem();

  // Good: Self-documenting, explicit business intent
  const elapsedTimeInSeconds: number = 86400;
  const flaggedUnpaidOrders: Order[] = getUnpaidOrders();
  ```
* **Avoid Disinformation:** Avoid naming an array `orderList` unless it is an actual List data structure. Prefer `orders`.
* **Make Meaningful Distinctions:** Never use arbitrary suffixes like `ProductData` vs. `ProductInfo` vs. `Product`—they provide zero distinct semantic value.
* **Pronounceable & Searchable Names:** The length of a variable name should be proportional to the scope of its usage. Single-letter variables are only acceptable as short loop indices (`i`, `j`) spanning 3–5 lines.

### B. Functions: The Rules of Smallness & Cohesion (هندسة الدوال)
1. **Rule 1: Small!** Functions should rarely exceed 20 lines of code.
2. **Rule 2: Do Exactly One Thing (Single Responsibility):** A function should do one thing, do it well, and do it only.
   - *Test:* A function does one thing if you cannot extract another meaningful function from it without restating its primary purpose.
3. **One Level of Abstraction per Function (SLAP):**
   - Statements within a function should all reside at the same conceptual level of abstraction (e.g., don't mix high-level business workflow logic with low-level string buffer manipulations).
4. **The Stepdown Rule:** Code should read like a top-down narrative, where each function is immediately followed by the functions at the next level of abstraction down.
5. **Function Arguments:**
   - Ideal: **0 (Niladic)**.
   - Acceptable: **1 (Monadic)** or **2 (Dyadic)**.
   - Requires justification: **3 (Triadic)**.
   - Strictly forbidden: **4+ (Polyadic)** $\rightarrow$ Wrap into a typed Parameter Object or Value Object.
   - **No Flag Arguments:** Passing a boolean flag (`render(isSuite: boolean)`) proves the function does two things. Split it into `renderForSuite()` and `renderSingle()`.

### C. Command-Query Separation (CQS)
* A function should either **do something** (mutate state) or **answer something** (return state), but never both:
  ```typescript
  // Bad: Mutates state AND returns boolean success check
  if (set("username", "unclebob")) { ... }

  // Good: Explicit separation
  if (attributeExists("username")) {
    setAttribute("username", "unclebob");
  }
  ```

### D. Error Handling as a First-Class Citizen
* **Use Exceptions Rather than Return Codes:** Error return codes force callers to write deeply nested `if/else` checks, polluting the happy path.
* **Write Try-Catch-Finally First:** A `try` block defines a scope boundary. Extract the body of the `try` and `catch` blocks into separate named functions to preserve single responsibility.
* **Don't Return Null:** Returning `null` creates rampant `if (obj !== null)` clutter and inevitable runtime crashes. Return empty collections, Optional/Maybe types, or Null Objects.
* **Don't Pass Null:** Passing `null` into method parameters should be forbidden by type systems and static linters.

---

## 3. Actionable Rules for our Craftsmanship Skill
1. **The Single Abstraction Rule:** Never mix high-level business orchestration with low-level technical plumbing in the same function.
2. **The Zero-Null Invariant:** Production APIs and domain routines must never return `null` for collections or aggregate queries; return empty sets or dedicated domain result types.
3. **The Parameter Capping Rule:** Functions with more than 3 parameters must be refactored to accept a strongly typed parameter interface/object.
