# 🏗️ Architectural Patterns

# Table of Contents
1. [Monolithic Architecture](#monolithic-architecture)
2. [Microservices](#microservices)
3. [Hexagonal Architecture](./hexagonal.md)
4. [Event-Driven Architecture](./event-driven.md)
5. [Serverless](#serverless)
6. [CQRS](#cqrs)
7. [Saga Pattern](#saga-pattern)
8. [Circuit Breaker](#circuit-breaker)
9. [CAP Theorem](#cap-theorem)
10. [Service Discovery](#service-discovery)
11. [API Gateway](#api-gateway)
12. [Data Mesh](./data-mesh.md)

---

Exploration of system-level architectures:

## Monolithic Architecture
A single-tiered software application in which the user interface and data access code are combined into a single program from a single platform.

### Monolithic Architecture Diagram
```mermaid
graph TD
    Client[Client Applications]
    
    subgraph Monolith[Single Application]
        UI[User Interface Layer]
        Business[Business Logic Layer]
        Data[Data Access Layer]
        
        UI --> Business
        Business --> Data
    end
    
    subgraph Database [Database]
        DB[(Single Database)]
    end
    
    Client --> UI
    Data --> DB
    
    style Monolith fill:#ffebee
    style Database fill:#e8f5e8
```

## Microservices
An architectural style that structures an application as a collection of services that are highly maintainable and testable, loosely coupled, independently deployable, and organized around business capabilities.

### Microservices Architecture Diagram
```mermaid
graph TD
    Client[Client Applications]
    
    subgraph APIGateway[API Gateway]
        Gateway[Gateway Layer]
    end
    
    subgraph Services[Microservices]
        UserService[User Service]
        OrderService[Order Service]
        PaymentService[Payment Service]
        InventoryService[Inventory Service]
        NotificationService[Notification Service]
    end
    
    subgraph Databases [Databases]
        UserDB[(User DB)]
        OrderDB[(Order DB)]
        PaymentDB[(Payment DB)]
        InventoryDB[(Inventory DB)]
        NotificationDB[(Notification DB)]
    end
    
    subgraph Messaging[Message Broker]
        Broker[(Event Broker)]
    end
    
    Client --> Gateway
    Gateway --> UserService
    Gateway --> OrderService
    Gateway --> PaymentService
    Gateway --> InventoryService
    Gateway --> NotificationService
    
    UserService --> UserDB
    OrderService --> OrderDB
    PaymentService --> PaymentDB
    InventoryService --> InventoryDB
    NotificationService --> NotificationDB
    
    UserService -.-> Broker
    OrderService -.-> Broker
    PaymentService -.-> Broker
    InventoryService -.-> Broker
    NotificationService -.-> Broker
    
    style Services fill:#e3f2fd
    style Databases fill:#e8f5e8
    style Messaging fill:#fff3e0
```

## Hexagonal Architecture (Ports and Adapters)
An architectural pattern used in software design. It aims at creating loosely coupled application components that can be easily connected to their software environment by means of ports and adapters.
> [!TIP]
> [Read the full guide on Hexagonal Architecture](./hexagonal.md)

## Event-Driven Architecture
A software architecture paradigm promoting the production, detection, consumption of, and reaction to events.
> [!TIP]
> [Read the full guide on Event-Driven Architecture](./event-driven.md)

## Serverless
A cloud-computing execution model in which the cloud provider allocates machine resources on demand, taking care of the servers on behalf of their customers.

### Serverless Architecture Diagram
```mermaid
graph TD
    Client[Client Applications]
    
    subgraph CloudProvider[Cloud Provider]
        subgraph APIGateway[API Gateway]
            Gateway[Gateway Service]
        end
        
        subgraph Functions[Serverless Functions]
            AuthFunction[Auth Function]
            ProcessFunction[Process Function]
            NotifyFunction[Notification Function]
        end
        
        subgraph Services[Managed Services]
            Database[(Managed Database)]
            Storage[(Object Storage)]
            Queue[(Message Queue)]
        end
    end
    
    Client --> Gateway
    Gateway --> AuthFunction
    Gateway --> ProcessFunction
    Gateway --> NotifyFunction
    
    AuthFunction --> Database
    ProcessFunction --> Database
    ProcessFunction --> Queue
    NotifyFunction --> Queue
    NotifyFunction --> Storage
    
    style CloudProvider fill:#f3e5f5
    style Functions fill:#e8f5e8
    style Services fill:#fff3e0
```

## CQRS (Command Query Responsibility Segregation)
An architectural pattern that separates read and update operations for a data store to maximize performance, scalability, and security.
> [!TIP]
> [Read the full guide on CQRS](./cqrs.md)

## Saga Pattern
A failure management pattern that helps establish consistency in distributed applications and coordinates transactions between multiple microservices to maintain data integrity. It uses a sequence of local transactions and compensating transactions for rollback.

### Saga Pattern Diagram
```mermaid
sequenceDiagram
    participant Client
    participant Orchestrator as Saga Orchestrator
    participant OrderService as Order Service
    participant PaymentService as Payment Service
    participant InventoryService as Inventory Service
    participant NotificationService as Notification Service
    
    Client->>Orchestrator: Create Order
    Orchestrator->>OrderService: Create Order
    OrderService-->>Orchestrator: Order Created
    
    Orchestrator->>PaymentService: Process Payment
    PaymentService-->>Orchestrator: Payment Successful
    
    Orchestrator->>InventoryService: Reserve Items
    InventoryService-->>Orchestrator: Items Reserved
    
    Orchestrator->>NotificationService: Send Confirmation
    NotificationService-->>Orchestrator: Notification Sent
    Orchestrator-->>Client: Order Completed
    
    Note over Orchestrator,InventoryService: If any step fails:
    Orchestrator->>NotificationService: Send Cancellation
    Orchestrator->>InventoryService: Release Reservation
    Orchestrator->>PaymentService: Refund Payment
    Orchestrator->>OrderService: Cancel Order
```

## Circuit Breaker
A design pattern used to detect failures and encapsulate the logic of preventing a failure from constantly recurring.
> [!TIP]
> [Read the full guide on Resilience Patterns (including Circuit Breaker)](../resilience-patterns/README.md)

### Circuit Breaker Pattern Diagram
```mermaid
stateDiagram-v2
    [*] --> Closed
    Closed --> Open: Failure Threshold Reached
    Open --> Half_Open: Timeout Expires
    Half_Open --> Closed: Success
    Half_Open --> Open: Failure
    Open --> Closed: Manual Reset
    
    state Closed {
        [*] --> Normal
        Normal --> Normal: Request Success
        Normal --> Monitoring: Request Failure
        Monitoring --> Normal: Request Success
        Monitoring --> Monitoring: Request Failure
    }
    
    state Open {
        [*] --> FailingFast
        FailingFast --> FailingFast: All Requests Fail
    }
    
    state Half_Open {
        [*] --> Testing
        Testing --> Testing: Limited Requests
    }
```

## CAP Theorem
Also known as Brewer's theorem, it states that a distributed data store can only provide two out of three guarantees:
- **Consistency**: Every read receives the most recent write or an error.
- **Availability**: Every request receives a (non-error) response, without the guarantee that it contains the most recent write.
- **Partition Tolerance**: The system continues to operate despite an arbitrary number of messages being dropped (or delayed) by the network between nodes.

### CAP Theorem Visualization
```mermaid
graph TD
    subgraph CAP[CAP Theorem Triangle]
        C[Consistency]
        A[Availability]
        P[Partition Tolerance]
        
        C --- A
        A --- P
        P --- C
    end
    
    subgraph Systems[System Examples]
        CA[CA Systems]
        CP[CP Systems]
        AP[AP Systems]
    end
    
    subgraph Examples[Examples]
        RDBMS[(Traditional RDBMS)]
        MongoDB[(MongoDB)]
        Cassandra[(Cassandra)]
        DNS[(DNS)]
    end
    
    C --> CA
    A --> CA
    CA --> RDBMS
    
    C --> CP
    P --> CP
    CP --> MongoDB
    
    A --> AP
    P --> AP
    AP --> Cassandra
    AP --> DNS
    
    style CAP fill:#f3e5f5
    style Systems fill:#e8f5e8
    style Examples fill:#fff3e0
```

## Service Discovery
Automatically detecting devices and services on a computer network.
> [!TIP]
> [Read the full guide on Service Discovery](./service-discovery.md)

## API Gateway
A server that acts as the single entry point into a microservices architecture.
> [!TIP]
> [Read the full guide on API Gateway](./api-gateway.md)

## Data Mesh
A decentralized sociotechnical paradigm for data architecture that shifts from a centralized, monolithic data platform to a distributed, domain-oriented approach. It treats data as a product and enables organizations to scale their data initiatives while maintaining agility and ownership.
> [!TIP]
> [Read the full guide on Data Mesh](./data-mesh.md)
