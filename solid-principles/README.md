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

<details>
<summary>☕ Java</summary>

```java
public class User {
    private String name;
    public User(String name) { this.name = name; }
    public void saveToDatabase() { /* ... */ }
    public void sendWelcomeEmail() { /* ... */ }
}
```
</details>

<details>
<summary>🐹 Go</summary>

```go
type User struct {
    Name string
}

func (u *User) SaveToDatabase() { /* ... */ }
func (u *User) SendWelcomeEmail() { /* ... */ }
```
</details>

<details>
<summary>🐍 Python</summary>

```python
class User:
    def __init__(self, name):
        self.name = name
    
    def save_to_database(self):
        # Logic to save to DB
        pass
    
    def send_welcome_email(self):
        # Logic to send email
        pass
```
</details>

### ✅ Good Example
Responsibilities are split into specialized components.

<details>
<summary>☕ Java</summary>

```java
public class User {
    private String name;
    public User(String name) { this.name = name; }
}

public class UserRepository {
    public void save(User user) { /* ... */ }
}

public class EmailService {
    public void sendWelcome(User user) { /* ... */ }
}
```
</details>

<details>
<summary>🐹 Go</summary>

```go
type User struct {
    Name string
}

type UserRepository struct {}
func (r *UserRepository) Save(u User) { /* ... */ }

type EmailService struct {}
func (e *EmailService) SendWelcome(u User) { /* ... */ }
```
</details>

<details>
<summary>🐍 Python</summary>

```python
class User:
    def __init__(self, name):
        self.name = name

class UserRepository:
    def save(self, user):
        # Logic to save to DB
        pass

class EmailService:
    def send_welcome(self, user):
        # Logic to send email
        pass
```
</details>

---

## 2. Open/Closed Principle (OCP)
> Software entities should be open for extension, but closed for modification.

### ❌ Bad Example
Adding a new shape requires modifying the calculator.

<details>
<summary>☕ Java</summary>

```java
public class AreaCalculator {
    public double calculate(Object shape) {
        if (shape instanceof Square) {
            return ((Square)shape).size * ((Square)shape).size;
        } else if (shape instanceof Circle) {
            return Math.PI * ((Circle)shape).radius * ((Circle)shape).radius;
        }
        return 0;
    }
}
```
</details>

<details>
<summary>🐹 Go</summary>

```go
func CalculateArea(shape interface{}) float64 {
    switch s := shape.(type) {
    case Square:
        return s.Size * s.Size
    case Circle:
        return math.Pi * s.Radius * s.Radius
    }
    return 0
}
```
</details>

<details>
<summary>🐍 Python</summary>

```python
class AreaCalculator:
    def calculate(self, shape):
        if isinstance(shape, Square):
            return shape.size ** 2
        elif isinstance(shape, Circle):
            return math.pi * (shape.radius ** 2)
```
</details>

### ✅ Good Example
New shapes can be added without changing existing logic.

<details>
<summary>☕ Java</summary>

```java
public interface Shape {
    double calculateArea();
}

public class Square implements Shape {
    public double size;
    public double calculateArea() { return size * size; }
}

public class AreaCalculator {
    public double calculate(Shape shape) { return shape.calculateArea(); }
}
```
</details>

<details>
<summary>🐹 Go</summary>

```go
type Shape interface {
    CalculateArea() float64
}

type Square struct { Size float64 }
func (s Square) CalculateArea() float64 { return s.Size * s.Size }

type AreaCalculator struct{}
func (a AreaCalculator) Calculate(s Shape) float64 {
    return s.CalculateArea()
}
```
</details>

<details>
<summary>🐍 Python</summary>

```python
from abc import ABC, abstractmethod

class Shape(ABC):
    @abstractmethod
    def calculate_area(self):
        pass

class Square(Shape):
    def __init__(self, size):
        self.size = size
    def calculate_area(self):
        return self.size ** 2

class AreaCalculator:
    def calculate(self, shape: Shape):
        return shape.calculate_area()
```
</details>

---

## 3. Liskov Substitution Principle (LSP)
> Derived classes must be substitutable for their base classes.

### ❌ Bad Example
Breaking expectations of the base type.

<details>
<summary>☕ Java</summary>

```java
public class Bird {
    public void fly() { /* ... */ }
}

public class Ostrich extends Bird {
    @Override
    public void fly() { throw new UnsupportedOperationException(); }
}
```
</details>

<details>
<summary>🐹 Go</summary>

```go
type Bird struct{}
func (b Bird) Fly() { /* ... */ }

type Ostrich struct {
    Bird // Embedding
}
func (o Ostrich) Fly() { panic("can't fly") }
```
</details>

<details>
<summary>🐍 Python</summary>

```python
class Bird:
    def fly(self):
        pass

class Ostrich(Bird):
    def fly(self):
        raise Exception("Can't fly")
```
</details>

### ✅ Good Example
The hierarchy correctly reflects capabilities.

<details>
<summary>☕ Java</summary>

```java
public class Bird { /* ... */ }
public abstract class FlyingBird extends Bird {
    public abstract void fly();
}
public class Ostrich extends Bird { /* ... */ }
```
</details>

<details>
<summary>🐹 Go</summary>

```go
type Bird interface {
    Eat()
}

type FlyingBird interface {
    Bird
    Fly()
}

type Ostrich struct{}
func (o Ostrich) Eat() { /* ... */ }
```
</details>

<details>
<summary>🐍 Python</summary>

```python
class Bird:
    def eat(self):
        pass

class FlyingBird(Bird):
    def fly(self):
        pass

class Ostrich(Bird):
    pass
```
</details>

---

## 4. Interface Segregation Principle (ISP)
> Make fine-grained interfaces that are client-specific.

### ❌ Bad Example
Forcing implementation of unused methods.

<details>
<summary>☕ Java</summary>

```java
public interface MultiFunctionDevice {
    void print();
    void scan();
}

public class SimplePrinter implements MultiFunctionDevice {
    public void print() { /* ... */ }
    public void scan() { /* Unsupported */ }
}
```
</details>

<details>
<summary>🐹 Go</summary>

```go
type MultiFunctionDevice interface {
    Print()
    Scan()
}

type SimplePrinter struct{}
func (s SimplePrinter) Print() { /* ... */ }
func (s SimplePrinter) Scan() { panic("not supported") }
```
</details>

<details>
<summary>🐍 Python</summary>

```python
class MultiFunctionDevice:
    def print(self): pass
    def scan(self): pass

class SimplePrinter(MultiFunctionDevice):
    def print(self):
        # logic
        pass
    def scan(self):
        raise NotImplementedError()
```
</details>

### ✅ Good Example
Clients only implement what they need.

<details>
<summary>☕ Java</summary>

```java
public interface Printer { void print(); }
public interface Scanner { void scan(); }

public class SimplePrinter implements Printer {
    public void print() { /* ... */ }
}
```
</details>

<details>
<summary>🐹 Go</summary>

```go
type Printer interface { Print() }
type Scanner interface { Scan() }

type SimplePrinter struct{}
func (s SimplePrinter) Print() { /* ... */ }
```
</details>

<details>
<summary>🐍 Python</summary>

```python
class Printer:
    def print(self): pass

class Scanner:
    def scan(self): pass

class SimplePrinter(Printer):
    def print(self):
        # logic
        pass
```
</details>

---

## 5. Dependency Inversion Principle (DIP)
> Depend on abstractions, not on concretions.

### ❌ Bad Example
Tight coupling to concrete implementations.

<details>
<summary>☕ Java</summary>

```java
public class UserLogic {
    private MySqlDatabase db = new MySqlDatabase();
}
```
</details>

<details>
<summary>🐹 Go</summary>

```go
type UserLogic struct {
    db MySqlDatabase // Concrete type
}
```
</details>

<details>
<summary>🐍 Python</summary>

```python
class UserLogic:
    def __init__(self):
        self.db = MySqlDatabase()
```
</details>

### ✅ Good Example
Depending on an abstraction.

<details>
<summary>☕ Java</summary>

```java
public interface Database { void save(String data); }

public class UserLogic {
    private Database db;
    public UserLogic(Database db) { this.db = db; }
}
```
</details>

<details>
<summary>🐹 Go</summary>

```go
type Database interface {
    Save(data string)
}

type UserLogic struct {
    db Database // Interface
}

func NewUserLogic(db Database) *UserLogic {
    return &UserLogic{db: db}
}
```
</details>

<details>
<summary>🐍 Python</summary>

```python
class Database(ABC):
    @abstractmethod
    def save(self, data): pass

class UserLogic:
    def __init__(self, db: Database):
        self.db = db
```
</details>
