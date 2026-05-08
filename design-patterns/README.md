# 🎨 Design Patterns

# Table of Contents
1. [Creational Patterns](#1-creational-patterns-object-creation)
2. [Structural Patterns](#2-structural-patterns-object-assembly)
3. [Behavioral Patterns](#3-behavioral-patterns-object-interaction)
4. [Modern/Web Patterns](#4-modernweb-patterns)

---

Design patterns represent the best practices used by experienced object-oriented software developers. They are solutions to general problems that software developers faced during software development.

---

## 1. Creational Patterns (Object Creation)
These patterns provide flexible object creation mechanisms:

- **Abstract Factory**: Creates families of related objects without specifying concrete classes.
- **Builder**: Constructs complex objects step-by-step.
- **Factory Method**: Defines an interface for creating an object, but lets subclasses decide which class to instantiate.
- **Prototype**: Creates new objects by cloning existing ones.
- **Singleton**: Ensures a class has only one instance and provides a global access point.

---

## 2. Structural Patterns (Object Assembly)
These patterns explain how to assemble objects and classes into larger, efficient structures.

- **Adapter**: Connects incompatible interfaces, allowing them to work together.
- **Bridge**: Separates an abstraction from its implementation so they can vary independently.
- **Composite**: Composes objects into tree structures to represent part-whole hierarchies.
- **Decorator**: Dynamically adds new behaviors to objects without changing their code.
- **Facade**: Provides a simplified, high-level interface to a complex system.
- **Flyweight**: Reduces memory usage by sharing as much data as possible with similar objects.
- **Proxy**: Provides a placeholder or surrogate for another object to control access to it.

---

## 3. Behavioral Patterns (Object Interaction)
These patterns manage algorithms and responsibilities between objects.

- **Chain of Responsibility**: Passes requests along a chain of handlers.
- **Command**: Encapsulates a request as an object, allowing parameterization of clients.
- **Iterator**: Accesses elements of a collection sequentially without exposing its underlying representation.
- **Mediator**: Simplifies communication between objects by directing them through a central hub.
- **Memento**: Captures and restores an object's internal state.
- **Observer**: Notifies multiple objects about any events happening to the object they observe.
- **State**: Allows an object to alter its behavior when its internal state changes.
- **Strategy**: Defines a family of algorithms and makes them interchangeable.
- **Template Method**: Defines the skeleton of an algorithm, leaving some steps to subclasses.
- **Visitor**: Separates an algorithm from the object structure on which it operates.

---

## 4. Modern/Web Patterns
Focused on frontend and Node.js development:

- **Module Pattern**: Splits code into smaller, reusable pieces.
- **Mixin Pattern**: Adds functionality to objects/classes without inheritance.
- **Provider Pattern**: Provides data to multiple components without manual prop-passing.
