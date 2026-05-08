# 🏗️ Architectural Patterns

# Table of Contents
1. [Monolithic Architecture](#monolithic-architecture)
2. [Microservices](#microservices)
3. [Hexagonal Architecture](#hexagonal-architecture-ports-and-adapters)
4. [Event-Driven Architecture](#event-driven-architecture)
5. [Serverless](#serverless)
6. [CQRS](#cqrs)
7. [Saga Pattern](#saga-pattern)
8. [Circuit Breaker](#circuit-breaker)
9. [CAP Theorem](#cap-theorem)

---

Exploration of system-level architectures:

## Monolithic Architecture
A single-tiered software application in which the user interface and data access code are combined into a single program from a single platform.

## Microservices
An architectural style that structures an application as a collection of services that are highly maintainable and testable, loosely coupled, independently deployable, and organized around business capabilities.

## Hexagonal Architecture (Ports and Adapters)
An architectural pattern used in software design. It aims at creating loosely coupled application components that can be easily connected to their software environment by means of ports and adapters.

## Event-Driven Architecture
A software architecture paradigm promoting the production, detection, consumption of, and reaction to events.

## Serverless
A cloud-computing execution model in which the cloud provider allocates machine resources on demand, taking care of the servers on behalf of their customers.

## CQRS (Command Query Responsibility Segregation)
An architectural pattern that separates read and update operations for a data store. Implementing CQRS in your application can maximize its performance, scalability, and security.

## Saga Pattern
A failure management pattern that helps establish consistency in distributed applications and coordinates transactions between multiple microservices to maintain data integrity. It uses a sequence of local transactions and compensating transactions for rollback.

## Circuit Breaker
A design pattern used to detect failures and encapsulate the logic of preventing a failure from constantly recurring. It prevents an application from repeatedly trying to execute an operation that's likely to fail, allowing it to continue without waiting for the fault to be fixed or wasting CPU cycles.

## CAP Theorem
Also known as Brewer's theorem, it states that a distributed data store can only provide two out of three guarantees:
- **Consistency**: Every read receives the most recent write or an error.
- **Availability**: Every request receives a (non-error) response, without the guarantee that it contains the most recent write.
- **Partition Tolerance**: The system continues to operate despite an arbitrary number of messages being dropped (or delayed) by the network between nodes.
