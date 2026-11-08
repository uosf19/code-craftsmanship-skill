# Book 07: Head First Design Patterns
**Authors:** Eric Freeman & Elisabeth Robson  
**Core Domain:** Object-Oriented Design Patterns & Principles  
**Key Themes:** Gang of Four (GoF) Patterns, Composition over Inheritance, Program to Interfaces, Loose Coupling.

---

## 1. Executive Summary & Core Thesis
Design patterns are reusable solutions to recurring problems in software design. Patterns do not go directly into code; they go into your mind, giving you a proven vocabulary and structural blueprint for composing objects and classes. Freeman and Robson emphasize that design patterns represent the practical embodiment of object-oriented design principles.

---

## 2. The Core Object-Oriented Design Principles

1. **Encapsulate What Varies:** Identify the aspects of your application that change and separate them from what stays the same.
2. **Program to an Interface, Not an Implementation:** Depend on abstract interfaces or base types rather than concrete implementation classes.
3. **Favor Composition over Inheritance:** Inheritance creates rigid class hierarchies and exposes subclasses to base class changes. Composition yields dynamic, flexible behavior at runtime.
4. **Strive for Loosely Coupled Designs:** Objects that interact should have as little knowledge of each other as possible.
5. **Open-Closed Principle (OCP):** Classes should be open for extension, but closed for modification.
6. **Dependency Inversion Principle (DIP):** High-level modules should not depend on low-level modules; both should depend on abstractions.

---

## 3. High-Impact Patterns for Enterprise Software

```text
       [ Essential GoF Patterns for Enterprise Applications ]
                                 |
   +--------------------+--------------------+--------------------+
   |                    |                    |                    |
[ Creational ]       [ Structural ]       [ Behavioral ]       [ Behavioral ]
Factory / Builder    Adapter / Decorator  Strategy / State     Observer / Command
```

### A. The Strategy Pattern (نمط الاستراتيجية)
* **Definition:** Defines a family of algorithms, encapsulates each one, and makes them interchangeable. Strategy lets the algorithm vary independently from clients that use it.
* **In our ERP:**
  - `TaxCalculationStrategy`: illustrative only. Rates and rules change over time, so keep them as effective-dated data (rate tables with start/end dates, verified per jurisdiction) so historical documents can be recalculated; use a Strategy when the *algorithm* differs, not merely the rate.
  - `PaymentSettlementStrategy`: Cash settlement vs. Terminal SDK vs. Online Gateway.

### B. The Adapter Pattern (نمط المحول)
* **Definition:** Converts the interface of a class into another interface clients expect. Adapter lets classes work together that couldn't otherwise because of incompatible interfaces.
* **In our ERP:**
  - Wrapping vendor biometric timeclocks, Paymob payment webhooks, or Egyptian Tax Authority (ETA) APIs behind clean internal domain interfaces.

### C. The Observer Pattern (نمط المراقب)
* **Definition:** Defines a one-to-many dependency between objects so that when one object changes state, all its dependents are notified automatically.
* **In our ERP:**
  - In-process event notification. Across processes or devices (e.g., KDS over WebSockets) the same idea becomes publish/subscribe with delivery, ordering and retry concerns that the classic pattern does not cover.

### D. The State Pattern (نمط الحالة)
* **Definition:** Allows an object to alter its behavior when its internal state changes. The object will appear to change its class.
* **In our ERP:**
  - Managing the table lifecycle (`VacantState`, `SeatedState`, `BilledState`) and order lifecycle cleanly without massive nested `if/switch` statements.

---

## 4. Actionable Rules for our Craftsmanship Skill
1. **The Composition-First Invariant:** Never use class inheritance solely for code reuse. Use inheritance only when an undeniable "is-a" polymorphic relationship exists; otherwise, use composition.
2. **The Strategy Rule for Volatile Algorithms:** Any business calculation subject to regulatory or commercial variation (taxes, discounts, shipping fees) must be encapsulated in a Strategy class.
3. **The Adapter Isolation Rule:** No third-party SDK or external API library may be called directly from core domain services. It must be wrapped by an Adapter.
