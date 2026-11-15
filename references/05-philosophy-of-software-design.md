# Book 05: A Philosophy of Software Design (2nd Edition)
**Author:** John Ousterhout (Stanford University)  
**Core Domain:** Software Complexity Management, Deep Modules, Information Hiding & Interface Design  
**Key Themes:** Deep vs. Shallow Modules, Tactical vs. Strategic Programming, Pull Complexity Downwards.

---

## 1. Executive Summary & Core Thesis
Complexity is the primary enemy of software engineering. It is anything related to the structure of a software system that makes it hard to understand and modify. John Ousterhout challenges traditional dogmas (such as breaking code into hundreds of tiny classes), proposing that true modularity is achieved through **Deep Modules**—classes and functions that have a tiny, simple interface but hide a massive, powerful implementation.

---

## 2. The Three Symptoms & Two Causes of Complexity

### A. The Three Symptoms of Complexity
1. **Change Amplification:** A simple business change requires touching multiple files and modules.
2. **Cognitive Load:** A developer must hold excessive knowledge in their head to complete a simple modification safely.
3. **Unknown Unknowns (The Deadliest):** It is not obvious which pieces of code must be modified, or what side effects will occur.

### B. The Two Fundamental Causes
1. **Dependencies:** When a piece of code cannot be understood or modified in isolation from others.
2. **Obscurity:** When important information or design intent is hidden or non-obvious.

---

## 3. Deep Modules vs. Shallow Modules (الوحدات العميقة والضحلة)

```text
    Shallow Module (BAD)                  Deep Module (GOOD)
   +--------------------+               +--------------------+
   |     Interface      |               |     Interface      |
   +--------------------+               +--------------------+
   |   Implementation   |               |                    |
   +--------------------+               |                    |
                                        |   Implementation   |
                                        |  (Rich, Hidden,    |
                                        |   Complex Details) |
                                        |                    |
                                        +--------------------+
```

* **Shallow Modules (Bad):**
  - An interface that is large relative to the functionality it provides.
  - A class that does almost nothing except pass parameters to another class (Pass-Through Methods).
  - *Result:* Increases cognitive load without reducing complexity.
* **Deep Modules (Good):**
  - An interface that is simple and concise, hiding massive internal implementation complexity.
  - *Classic Example: Unix File I/O:* Just 5 system calls (`open`, `read`, `write`, `close`, `lseek`) hide disk formatting, sector allocations, caching, and physical hardware drivers.

### The Golden Rule: "Pull Complexity Downwards"
* Handle implementation details and their edge cases inside the module when callers can use a simpler interface as a result. [Ousterhout's design course summary](https://web.stanford.edu/~ouster/cs190-winter23/slides/wrapup.pdf)

---

## 4. Tactical vs. Strategic Programming
* **Tactical Programming (The Death Trap):**
  - Focusing solely on shipping a feature or quick fix as fast as possible.
  - Accumulates "tactical tornadoes" of spaghetti dependencies and technical debt.
* **Strategic Programming (The Investment Mindset):**
  - Treating working code as insufficient; the code must also facilitate future modifications.
  - Reserving time for design work as part of normal development. Ousterhout's course notes discuss a possible 10–20% investment alongside Chapter 3. [Ousterhout's Chapter 3 course notes](https://web.stanford.edu/~ouster/cgi-bin/cs190-winter18/lecture.php?topic=working)

---

## 5. Information Hiding vs. Information Leakage
* **Information Hiding:** Each module encapsulates a specific design decision so that other modules have zero knowledge of it (e.g., how orders are serialized or which database dialect is used).
* **Information Leakage:** Occurs when the same design decision is reflected in multiple places (e.g., if a change to a JSON serialization schema forces changes in 10 different business services).

---

## 6. Where Ousterhout Disagrees with Other Books in This Skill
* **Clean Code:** Ousterhout argues that very small functions often create shallow modules and that comments capture design information code cannot; Martin favors tiny functions and fewer comments.
* **TDD:** Ousterhout considers test-first development prone to tactical programming. The [trade-offs table](../SKILL.md#conflicts--trade-offs-between-sources) describes when this skill prefers test-first.

## 7. Actionable Rules for our Craftsmanship Skill
1. **Module depth (Derived).** Keep a coherent, easy-to-use public interface that hides the details callers do not need.
2. **Delegation (Derived).** Review forwarding methods for the boundary they provide. An adapter, compatibility layer, or facade can make a stable contract even when its current implementation delegates.
3. **Design investment (Derived).** Make design work part of delivery. Agree on the time based on the project's maintenance needs.
