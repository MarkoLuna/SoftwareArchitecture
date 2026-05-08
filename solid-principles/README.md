# 🧱 SOLID Principles

# Table of Contents
1. [Single Responsibility Principle](#1-single-responsibility-principle)
2. [Open/Closed Principle](#2-openclosed-principle)
3. [Liskov Substitution Principle](#3-liskov-substitution-principle)
4. [Interface Segregation Principle](#4-interface-segregation-principle)
5. [Dependency Inversion Principle](#5-dependency-inversion-principle)

---

The foundation of object-oriented design and dependency management.

---

## 1. Single Responsibility Principle (SRP)
> A class should have one, and only one, reason to change.

### ❌ Bad Example
A single class handling user data, database persistence, and email notifications.
```javascript
class User {
  constructor(name) { this.name = name; }
  saveToDatabase() { /* ... */ }
  sendWelcomeEmail() { /* ... */ }
}
```

### ✅ Good Example
Responsibilities are split into specialized classes.
```javascript
class User {
  constructor(name) { this.name = name; }
}

class UserRepository {
  save(user) { /* ... */ }
}

class EmailService {
  sendWelcome(user) { /* ... */ }
}
```

---

## 2. Open/Closed Principle (OCP)
> Software entities should be open for extension, but closed for modification.

### ❌ Bad Example
Adding a new shape requires modifying the `AreaCalculator` class.
```javascript
class AreaCalculator {
  calculate(shape) {
    if (shape.type === 'square') return shape.size ** 2;
    if (shape.type === 'circle') return Math.PI * (shape.radius ** 2);
  }
}
```

### ✅ Good Example
New shapes can be added without changing existing logic.
```javascript
class Square {
  calculateArea() { return this.size ** 2; }
}

class Circle {
  calculateArea() { return Math.PI * (this.radius ** 2); }
}

class AreaCalculator {
  calculate(shape) { return shape.calculateArea(); }
}
```

---

## 3. Liskov Substitution Principle (LSP)
> Derived classes must be substitutable for their base classes.

### ❌ Bad Example
`Ostrich` breaks the expectation that all birds can fly.
```javascript
class Bird {
  fly() { /* ... */ }
}

class Ostrich extends Bird {
  fly() { throw new Error("I can't fly!"); }
}
```

### ✅ Good Example
The hierarchy correctly reflects capabilities.
```javascript
class Bird { /* ... */ }
class FlyingBird extends Bird {
  fly() { /* ... */ }
}

class Ostrich extends Bird { /* ... */ }
```

---

## 4. Interface Segregation Principle (ISP)
> Make fine-grained interfaces that are client-specific.

### ❌ Bad Example
A simple printer is forced to implement `scan` and `fax` even if it doesn't support them.
```javascript
class MultiFunctionDevice {
  print() {}
  scan() {}
  fax() {}
}
```

### ✅ Good Example
Interfaces are split so clients only implement what they need.
```javascript
class Printer { print() {} }
class Scanner { scan() {} }

class SimplePrinter extends Printer {
  print() { /* logic */ }
}
```

---

## 5. Dependency Inversion Principle (DIP)
> Depend on abstractions, not on concretions.

### ❌ Bad Example
`UserLogic` is tightly coupled to a specific database implementation.
```javascript
class MySqlDatabase {
  save(data) { /* ... */ }
}

class UserLogic {
  constructor() {
    this.db = new MySqlDatabase();
  }
}
```

### ✅ Good Example
`UserLogic` depends on an abstraction (the database interface).
```javascript
class UserLogic {
  constructor(database) {
    this.db = database; // Can be MySQL, MongoDB, etc.
  }
}
```
