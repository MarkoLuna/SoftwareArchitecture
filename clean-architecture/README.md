# 🧼 Clean Architecture

Clean Architecture is a software design philosophy introduced by Robert C. Martin ("Uncle Bob"). Its objective is the separation of concerns, allowing developers to build applications that are independent of frameworks, UI, databases, or any external agency.

---

## 🏗️ The Concentric Circles (The Dependency Rule)

The core principle of Clean Architecture is the **Dependency Rule**: *Dependencies must only point inwards.*

1. **Entities (Inmost Circle)**: These represent the business objects of the application and encapsulate the most general and high-level rules.
2. **Use Cases**: These contain the application-specific business rules. They orchestrate the flow of data to and from the entities.
3. **Interface Adapters**: This layer converts data from the format most convenient for use cases and entities to the format most convenient for external agencies (e.g., Controllers, Presenters, Gateways).
4. **Frameworks & Drivers (Outermost Circle)**: This layer is where all the details go (Database, Web Framework, UI, External Devices).

---

## 🔑 Key Characteristics

- **Independent of Frameworks**: The architecture does not depend on the existence of some library of feature-laden software. This allows you to use such frameworks as tools, rather than having to cram your system into their limited constraints.
- **Testable**: The business rules can be tested without the UI, Database, Web Server, or any other external element.
- **Independent of UI**: The UI can change easily, without changing the rest of the system. A Web UI could be replaced with a console UI, for example, without changing the business rules.
- **Independent of Database**: You can swap out Oracle or SQL Server, for Mongo, BigTable, CouchDB, or something else. Your business rules are not bound to the database.
- **Independent of any external agency**: In fact your business rules simply don’t know anything at all about the outside world.

---

## 📐 Implementation Guidelines

### 1. Entities
- Should not be affected by changes to external layers.
- Example: `User`, `Account`, `Transaction`.

### 2. Use Cases
- Should not be affected by changes to the database or UI.
- Example: `CreateAccount`, `TransferFunds`.

### 3. Interface Adapters
- Includes the MVC architecture of a GUI.
- Converts data from Use Cases into a format ready for the UI or DB.

### 4. Frameworks and Drivers
- Generally, you don’t write much code in this layer, other than glue code that communicates to the next circle inwards.

---

## ⚖️ Pro vs Contra

| Pro | Contra |
| :--- | :--- |
| High testability | Increased boilerplate |
| Easy to maintain | Steeper learning curve |
| Decoupled code | Can be over-engineering for simple apps |
| Future-proof | More files and abstractions |
