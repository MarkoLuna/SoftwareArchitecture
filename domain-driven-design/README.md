# 🧩 Domain-Driven Design (DDD)

Domain-Driven Design is an approach to software development that centers the development on a complex domain model. It was first introduced by Eric Evans in his 2003 book.

---

## 🗺️ Table of Contents
1. [Strategic Design](#1-strategic-design)
2. [Tactical Design](#2-tactical-design)
3. [Building Blocks](#3-building-blocks)
4. [Ubiquitous Language](#4-ubiquitous-language)

---

## 1. Strategic Design
Strategic design is about large-scale structure and managing complexity across teams and systems.

- **Bounded Context**: A central pattern in DDD. It defines a boundary (a logical separator) where a particular model is defined and applicable.
- **Ubiquitous Language**: A shared language developed by the team (developers and domain experts) to be used in discussions, documentation, and code.
- **Context Mapping**: The process of defining the relationships and interactions between different Bounded Contexts.

---

## 2. Tactical Design
Tactical design focuses on the implementation details within a single Bounded Context.

- **Aggregate**: A cluster of domain objects that can be treated as a single unit (e.g., an `Order` and its `OrderItems`).
- **Aggregate Root**: The single entry point to an aggregate. All external communication must go through the root.
- **Domain Events**: Something that happened in the domain that domain experts care about (e.g., `OrderPlaced`).

---

## 3. Building Blocks

### Entities
Objects defined by their unique identity rather than their attributes (e.g., a `User` with an ID).

### Value Objects
Objects that have no identity and are defined only by their attributes. They should be immutable (e.g., an `Address` or `Money`).

### Domain Services
Logic that doesn't naturally belong to an Entity or Value Object.

### Repositories
Used to encapsulate the logic for retrieving and storing aggregates.

---

## 4. Ubiquitous Language
The goal is to eliminate translation between domain experts and technical staff.

- **Code reflects the domain**: Use names from the business domain in class names, methods, and variables.
- **Consistency**: If a business person calls it "The Billing Cycle", the code should use `BillingCycle`, not `PaymentPeriod`.

---

## ⚖️ Pro vs Contra

| Pro | Contra |
| :--- | :--- |
| Aligns software with business needs | Steep learning curve |
| Modular and maintainable | Overkill for simple applications |
| Clear boundaries between teams | Requires close collaboration with domain experts |
| Highly testable business logic | Can lead to high initial complexity |
