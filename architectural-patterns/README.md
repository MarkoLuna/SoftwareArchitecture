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
13. [Fan-Out Pattern](#fan-out-pattern)
14. [Fan-In Pattern](#fan-in-pattern)

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

## Fan-Out Pattern
A messaging topology where **one publisher sends a single message to an exchange or topic, and that message is delivered to multiple downstream consumers simultaneously** — each receiving their own independent copy. It decouples the producer from the consumers: neither side needs to know about the other.

- **Exchange type used**: Fanout Exchange (RabbitMQ) or a topic with multiple subscriber groups (Kafka).
- **Key property**: Every bound queue/subscriber gets the full message regardless of content.
- **Classic use-cases**:
  - An `order.placed` event triggers Email, SMS, Push Notification, and Analytics pipelines at the same time.
  - A cache invalidation event must reach every application node.
  - A configuration-change event must notify all running service instances.

### Fan-Out Pattern Diagram
```mermaid
flowchart LR
    P(["📤 Publisher\norder.placed"])

    subgraph BROKER["Message Broker"]
        EX{{"Fanout Exchange"}}
    end

    subgraph CONSUMERS["Independent Consumers"]
        C1["📧 Email Service"]
        C2["📱 SMS Service"]
        C3["🔔 Push Notification Service"]
        C4["📊 Analytics Pipeline"]
    end

    P -->|"publish once"| EX
    EX -->|"copy"| C1
    EX -->|"copy"| C2
    EX -->|"copy"| C3
    EX -->|"copy"| C4
```

### Fan-Out Trade-offs

| Aspect | Detail |
| :--- | :--- |
| ✅ **Decoupling** | Producer is unaware of consumers; adding a new consumer requires zero producer changes. |
| ✅ **Parallelism** | All consumers process the event concurrently, minimizing total latency. |
| ⚠️ **Message amplification** | One message becomes N messages on the broker; monitor queue depth per consumer. |
| ⚠️ **Slow consumers** | A backed-up consumer queue does not affect other queues, but its own processing delay accumulates. |
| ❌ **No selective routing** | Every subscriber gets every message. Use a **Topic Exchange** with routing keys if filtering is needed. |

---

## Fan-In Pattern
A messaging topology where **multiple independent producers all publish to the same destination** (queue, topic, or channel) and **a single consumer aggregates and processes all of them**. It is the logical complement of Fan-Out.

- **Exchange type used**: Direct Exchange or a single shared topic (Kafka).
- **Key property**: All messages converge on one processing point, regardless of their source.
- **Classic use-cases**:
  - Multiple microservices (Orders, Payments, Inventory) all emit audit events that a single Audit Service consumes.
  - Multiple IoT sensors publish telemetry to one stream processor.
  - Several upstream services feed results into one aggregation / reporting pipeline.

### Fan-In Pattern Diagram
```mermaid
flowchart LR
    subgraph PRODUCERS["Independent Producers"]
        P1["🛒 Order Service"]
        P2["💳 Payment Service"]
        P3["📦 Inventory Service"]
        P4["📡 IoT Sensor"]
    end

    subgraph BROKER["Message Broker"]
        Q[/"aggregator.queue"/]
    end

    C(["🗂️ Audit / Aggregator Consumer"])

    P1 -->|"audit.event"| Q
    P2 -->|"audit.event"| Q
    P3 -->|"audit.event"| Q
    P4 -->|"telemetry.event"| Q
    Q -->|"consume"| C
```

### Fan-In Trade-offs

| Aspect | Detail |
| :--- | :--- |
| ✅ **Centralised processing** | One place to apply aggregation, deduplication, or enrichment logic. |
| ✅ **Producer independence** | Producers are fully decoupled from each other and from the consumer. |
| ⚠️ **Single consumer bottleneck** | If the consumer is slow, the queue grows. Scale with competing consumers in the same group. |
| ⚠️ **Message ordering** | Messages from different producers are interleaved; ordering guarantees require partition keys (Kafka) or sequencing logic in the consumer. |
| ❌ **Source attribution** | The consumer must inspect message metadata/headers to determine which producer sent a given message. |
