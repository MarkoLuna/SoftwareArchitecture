# 📚 Software Architecture Glossary

A comprehensive glossary of architectural terms, patterns, and concepts used throughout this documentation.

---

## 🗺️ Table of Contents
1. [Architectural Patterns](#1-architectural-patterns)
2. [Design Patterns](#2-design-patterns)
3. [Data & Storage](#3-data--storage)
4. [Infrastructure & Operations](#4-infrastructure--operations)
5. [Cloud-Native & DevOps](#5-cloud-native--devops)
6. [Cross-Cutting Concerns](#6-cross-cutting-concerns)

---

## 1. Architectural Patterns

### **API Gateway**
A server that acts as a single entry point into a microservices architecture, handling routing, authentication, rate limiting, and protocol translation.

### **Circuit Breaker**
A design pattern that detects failures and prevents cascading failures by stopping requests to failing services after a threshold is reached.

### **CQRS (Command Query Responsibility Segregation)**
An architectural pattern that separates read and update operations for a data store to maximize performance, scalability, and security.

### **Event-Driven Architecture**
A software architecture paradigm promoting the production, detection, consumption of, and reaction to events.

### **Event Sourcing**
Instead of storing just the current state of data, use an append-only store to record the full series of actions taken on that data.

### **Hexagonal Architecture (Ports and Adapters)**
An architectural pattern that creates loosely coupled application components that can be easily connected to their software environment by means of ports and adapters.

### **Microservices**
An architectural style that structures an application as a collection of services that are highly maintainable, testable, loosely coupled, and independently deployable.

### **Monolithic Architecture**
A single-tiered software application in which the user interface and data access code are combined into a single program from a single platform.

### **Saga Pattern**
A failure management pattern that helps establish consistency in distributed applications by coordinating transactions between multiple microservices.

### **Serverless**
A cloud-computing execution model in which the cloud provider allocates machine resources on demand, taking care of the servers on behalf of their customers.

### **Service Discovery**
The process of automatically detecting devices and services on a computer network, allowing services to find each other dynamically without hardcoded IP addresses.

### **Service Registry**
A database containing the network locations of service instances that must be highly available and up-to-date.

---

## 2. Design Patterns

### **Adapter Pattern**
Allows the interface of an existing class to be used as another interface.

### **Facade Pattern**
Provides a simplified interface to a larger body of code, such as a class library.

### **Observer Pattern**
Defines a one-to-many dependency between objects so that when one object changes state, all its dependents are notified and updated automatically.

### **Strategy Pattern**
Defines a family of algorithms, encapsulates each one, and makes them interchangeable.

### **Repository Pattern**
Mediates between the domain and data mapping layers using a collection-like interface for accessing domain objects.

---

## 3. Data & Storage

### **ACID Properties**
Atomicity, Consistency, Isolation, Durability - a set of properties that guarantee database transactions are processed reliably.

### **BASE Properties**
Basically Available, Soft state, Eventual consistency - an alternative to ACID for distributed systems.

### **CAP Theorem**
States that a distributed data store can only provide two out of three guarantees: Consistency, Availability, and Partition Tolerance.

### **Database Sharding**
The process of splitting a large database into smaller, faster, more easily managed parts called shards.

### **Eventual Consistency**
A consistency model used in distributed computing to achieve high availability that informally guarantees that, if no new updates are made to a given data item, all accesses to that item will eventually return the last updated value.

### **Normalization**
The process of organizing the columns (attributes) and tables (relations) of a relational database to minimize data redundancy.

### **Partitioning**
Dividing a database into smaller, more manageable pieces while maintaining the integrity of the data.

### **Replication**
The process of sharing information so as to ensure consistency between redundant resources, such as software or hardware components.

---

## 4. Infrastructure & Operations

### **Auto-scaling**
Automatically adjusting the number of computational resources in a server farm based on load.

### **Blue-Green Deployment**
A technique that reduces downtime and risk by running two identical production environments.

### **Canary Deployment**
A technique to reduce the risk of introducing a new software version in production by slowly rolling it out to a small subset of users before making it available to everybody.

### **Containerization**
Encapsulating an application in a container with its own operating environment.

### **Infrastructure as Code (IaC)**
The process of managing and provisioning infrastructure through machine-readable definition files, rather than physical hardware configuration or interactive configuration tools.

### **Load Balancing**
The process of distributing workloads across multiple computing resources.

### **Orchestration**
Automated configuration, coordination, and management of computer systems and software.

---

## 5. Cloud-Native & DevOps

### **CI/CD (Continuous Integration/Continuous Deployment)**
The practice of merging all developers' working copies to a shared mainline several times a day and automatically deploying code changes to testing or production environments.

### **Container Orchestration**
Automated deployment, management, scaling, and networking of containerized applications.

### **Distributed Tracing**
A method used to profile and monitor applications, especially those built using a microservices architecture.

### **Immutable Infrastructure**
An infrastructure paradigm where servers are never modified after deployment, but replaced entirely with new instances.

### **Observability**
The ability to measure a system's current state based on the data it generates, such as logs, metrics, and traces.

### **Sidecar Pattern**
A single-node pattern that has the sidecar attached to a parent application to provide supporting features.

### **Twelve-Factor App**
A methodology for building software-as-a-service apps that provides a set of best practices for cloud-native applications.

---

## 6. Cross-Cutting Concerns

### **Authentication**
The process of verifying the identity of a user, process, or device.

### **Authorization**
The process of determining what permissions an authenticated entity has.

### **Caching**
The process of storing data in a cache so that future requests for that data can be served faster.

### **Idempotency**
The property of certain operations in mathematics and computer science that can be applied multiple times without changing the result beyond the initial application.

### **Rate Limiting**
Controlling the rate of requests sent or received by an application or user.

### **Throttling**
The process of limiting the number of requests a user can make within a certain time period.

### **TLS/SSL (Transport Layer Security/Secure Sockets Layer)**
Cryptographic protocols that provide communications security over a computer network.

---

## 📖 Quick Reference

| Category | Key Terms | Common Use Cases |
|----------|-----------|------------------|
| **Architecture** | Microservices, Monolith, Serverless | System design decisions |
| **Data** | ACID, BASE, CAP Theorem | Database selection |
| **Operations** | CI/CD, IaC, Auto-scaling | DevOps practices |
| **Security** | Authentication, Authorization, TLS | Security implementation |

---

*This glossary is continuously updated. If you find missing terms or unclear definitions, please contribute to improve this resource.*
