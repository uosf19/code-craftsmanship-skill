# Code Craftsmanship & Modern Software Engineering Skill

A universal, production-grade engineering playbook for writing clean, robust, deeply modular, and maintainable software. 

Synthesized from **10 foundational classics** in software construction, clean code, refactoring patterns, and test-driven development curated by Dr. Milan Milanović (*Tech World With Milan*).

---

## 📚 The 10 Foundational References

This skill distills the engineering principles, heuristics, and invariants from:

1. **The Pragmatic Programmer: Your Journey to Mastery (20th Anniversary Edition)** — *David Thomas & Andrew Hunt*
2. **Clean Code: A Handbook of Agile Software Craftsmanship** — *Robert C. Martin ("Uncle Bob")*
3. **Refactoring: Improving the Design of Existing Code** — *Martin Fowler*
4. **Code Complete: A Practical Handbook of Software Construction (2nd Edition)** — *Steve McConnell*
5. **A Philosophy of Software Design (2nd Edition)** — *John Ousterhout (Stanford University)*
6. **Modern Software Engineering: Doing What Works to Build Better Software Faster** — *David Farley*
7. **Head First Design Patterns (2nd Edition)** — *Eric Freeman & Elisabeth Robson*
8. **Software Engineering at Google: Lessons Learned from Programming Over Time** — *Titus Winters, Tom Manshreck, Hyrum Wright*
9. **Test-Driven Development: By Example** — *Kent Beck*
10. **Unit Testing: Principles, Practices, and Patterns** — *Vladimir Khorikov*

Detailed chapter-by-chapter summaries and actionable rules are available in the [`references/`](references/) directory.

---

## 🏛️ The Four Craftsmanship Pillars

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

1. **Code Hygiene & Deep Modularity:** Intention-revealing naming, Deep Modules over shallow pass-through methods, Command-Query Separation (CQS), Single Level of Abstraction (SLAP), and Zero-Null policies.
2. **Complexity & Defensive Design:** The Barricade pattern for external boundary sanitization, Design by Contract (DbC), Fail-Fast & Crash Early, Hyrum's Law defense, and Cyclomatic Complexity $\le 10$.
3. **Disciplined Refactoring:** Two-Hat discipline (feature vs. refactoring), Code Smells taxonomy, Boy Scout rule, and atomic micro-step execution under green test suites.
4. **Resilient Testing & TDD:** The Four Pillars of good tests, strict Mock vs. Stub taxonomy (never assert interactions on stubs), testing observable behavior over private details, and the Beyoncé Rule.

---

## 📁 Repository Structure

```text
code-craftsmanship/
├── SKILL.md                 # Master skill playbook & execution rules
├── README.md                # Skill overview & catalog
├── references/              # Detailed extractions from all 10 books
│   ├── 01-pragmatic-programmer.md
│   ├── 02-clean-code.md
│   ├── 03-refactoring.md
│   ├── 04-code-complete.md
│   ├── 05-philosophy-of-software-design.md
│   ├── 06-modern-software-engineering.md
│   ├── 07-head-first-design-patterns.md
│   ├── 08-software-engineering-at-google.md
│   ├── 09-test-driven-development.md
│   └── 10-unit-testing-principles.md
└── checklists/              # Operational checklists for code writing & review
    ├── code-review-checklist.md
    ├── refactoring-checklist.md
    └── unit-testing-checklist.md
```

---

## 🚀 How to Install & Use in Antigravity or AI Agent Workspaces

### Option A: Install as a Global Antigravity Skill
Clone or copy this repository into your global or project skills directory:
```bash
git clone https://github.com/uosf19/code-craftsmanship-skill.git code-craftsmanship
```

### Option B: Use in Any Project
Reference `SKILL.md` in your project's coding standards or system prompts. AI agents will automatically activate this skill when writing, reviewing, refactoring, or testing code.

---

## 📄 License
MIT License. Created by Youssef Osman.
