# Code Craftsmanship & Modern Software Engineering Skill

A playbook for implementing new code, reviewing code, restructuring existing behavior, and choosing tests.

Ten books on software construction, clean code, refactoring, and testing inform the study notes. Individual references cite the chapter, excerpt, author material, or publisher contents consulted for specific claims. The [skill](SKILL.md) labels project applications and review heuristics as derived guidance.

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

The [skill](SKILL.md) links each book at the relevant topic. The [language idioms reference](references/11-language-idioms.md) applies the guidance to the project's current stack.

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

1. **Code Hygiene & Deep Modularity:** Intention-revealing naming, Deep Modules over shallow pass-through methods, Command-Query Separation (with documented exceptions), single level of abstraction, and a null-avoidance policy.
2. **Complexity & Defensive Design:** The Barricade pattern for external boundary sanitization, Design by Contract (DbC), Fail-Fast & Crash Early, Hyrum's Law defense, and complexity thresholds as team defaults.
3. **Disciplined Refactoring:** Reviewable behavior-preserving steps, context-based responses to code smells, and safe handling of existing edits.
4. **Resilient Testing & TDD:** Observable outcomes, suitable test doubles, unit and property tests, compatibility fixtures, real database tests, and workflow recovery.

---

## 📁 Repository Structure

```text
code-craftsmanship/
├── SKILL.md                 # Master skill playbook & execution rules
├── README.md                # Skill overview & catalog
├── references/              # Study notes and derived guidance
│   ├── 01-pragmatic-programmer.md
│   ├── 02-clean-code.md
│   ├── 03-refactoring.md
│   ├── 04-code-complete.md
│   ├── 05-philosophy-of-software-design.md
│   ├── 06-modern-software-engineering.md
│   ├── 07-head-first-design-patterns.md
│   ├── 08-software-engineering-at-google.md
│   ├── 09-test-driven-development.md
│   ├── 10-unit-testing-principles.md
│   └── 11-language-idioms.md
└── checklists/              # Operational checklists for code writing & review
    ├── code-review-checklist.md
    ├── refactoring-checklist.md
    └── unit-testing-checklist.md
```

---

## Installation

Use the complete `code-craftsmanship` folder from the chosen release. For direct use, point the agent to [SKILL.md](SKILL.md). For Codex project discovery, place or link the folder under `.agents/skills/` and check the skill selector in a fresh session. Use one discoverable copy per name. [Codex skill discovery](https://learn.chatgpt.com/docs/build-skills#where-codex-loads-local-skills)

---

## 📄 License
MIT License. Created by Youssef Osman.
