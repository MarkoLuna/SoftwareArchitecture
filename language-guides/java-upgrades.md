# ☕ Java Upgrades: JDK 8 to JDK 21

A guide to the most significant features and changes in the Java ecosystem since the landmark JDK 8 release.

---

## 🗺️ Table of Contents
1. [JDK 9-11: Foundations](#1-jdk-9-11-foundations)
2. [JDK 12-17: Modernization](#2-jdk-12-17-modernization)
3. [JDK 21: The Next Leap](#3-jdk-21-the-next-leap)
4. [Migration Tips](#4-migration-tips)

---

## 1. JDK 9-11: Foundations

### JDK 9: The Module System (Project Jigsaw)
Introduced `module-info.java` to define modules and their dependencies, improving encapsulation and reducing the runtime footprint.

### JDK 10: Local Variable Type Inference
Introduced the `var` keyword for local variables.
```java
var list = new ArrayList<String>(); // Compiler infers ArrayList<String>
```

### JDK 11 (LTS): New HTTP Client
A modern, non-blocking HTTP client that supports HTTP/2 and WebSockets (replaces `HttpURLConnection`).

---

## 2. JDK 12-17: Modernization

### Switch Expressions (JDK 14)
Improved switch syntax that can be used as an expression and returns a value.
```java
int numLetters = switch (day) {
    case MONDAY, FRIDAY, SUNDAY -> 6;
    case TUESDAY -> 7;
    default -> 0;
};
```

### Records (JDK 16)
Immutable data classes that reduce boilerplate code for getters, constructors, `equals()`, `hashCode()`, and `toString()`.
```java
public record User(String name, int age) {}
```

### Sealed Classes (JDK 17 LTS)
Restrict which other classes or interfaces may extend or implement them.
```java
public sealed class Shape permits Circle, Square {}
```

### Text Blocks (JDK 15)
Multiline string literals that avoid the need for most escape sequences and automatically format the string in a predictable way.
```java
String html = """
              <html>
                  <body>
                      <p>Hello, World</p>
                  </body>
              </html>
              """;
```

### Stream API Enhancements
- **JDK 9**: `takeWhile`, `dropWhile`, `ofNullable`.
- **JDK 16**: `.toList()` (convenient alternative to `.collect(Collectors.toList())`).

---

## 3. JDK 21: The Next Leap

### Virtual Threads (Project Loom)
Lightweight threads that dramatically reduce the effort of writing, maintaining, and observing high-throughput concurrent applications. They allow a single JVM to handle millions of concurrent tasks.

### Sequenced Collections
New interfaces (`SequencedCollection`, `SequencedSet`, `SequencedMap`) to represent collections with a defined encounter order.
```java
list.getFirst();
list.getLast();
list.reversed();
```

if (obj instanceof User(String name, int age)) {
    System.out.println(name + " is " + age);
}
```

### Scoped Values (Preview in JDK 21)
A way to share immutable data within and across threads. They are preferred over `ThreadLocal` variables, especially when using millions of virtual threads.

### Structured Concurrency (Preview in JDK 21)
Simplifies multithreaded programming by treating multiple tasks running in different threads as a single unit of work, streamlining error handling and cancellation.
```

---

## 4. Migration Tips

| From | To | Major Hurdles |
| :--- | :--- | :--- |
| **8** | **11** | Removal of Java EE modules (JAXB, etc.), classloader changes. |
| **11** | **17** | Strong encapsulation of JDK internals (use `--add-opens` sparingly). |
| **17** | **21** | Generally smooth, focus on adopting Virtual Threads for I/O bound tasks. |

> [!IMPORTANT]
> Always check for library compatibility (Spring Boot, Hibernate, etc.) before upgrading the JDK version.
