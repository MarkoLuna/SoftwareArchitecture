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

### **Backpressure**
A resilience mechanism in asynchronous systems (common in microservices) that allows a consumer to signal to a producer to slow down the rate of data transmission when overwhelmed.

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

example: 
    - Spring Cloud Consul
    - Spring Cloud Eureka
    - Spring Cloud Zookeeper

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

### **OAuth 2.0**
An industry-standard delegation framework that allows a third-party application to obtain limited access to an HTTP service on behalf of a resource owner.

### **OpenID Connect (OIDC)**
An identity layer built on top of OAuth 2.0 that adds authentication through an ID Token (JWT) containing user identity claims.

### **SAML 2.0 (Security Assertion Markup Language)**
An XML-based federated identity standard for exchanging authentication and authorization data between an Identity Provider and a Service Provider, commonly used in enterprise SSO.

### **SCIM 2.0 (System for Cross-domain Identity Management)**
A RESTful API standard for automating the exchange of user identity data between identity domains, covering the full user provisioning lifecycle.

### **JWT (JSON Web Token)**
A compact, URL-safe token format (RFC 7519) for representing claims between two parties. Used as access tokens, ID tokens, and refresh tokens.

### **JWKS (JSON Web Key Set)**
A set of public keys published by an Identity Provider, used by clients and resource servers to verify the cryptographic signature of JWTs.

### **Asymmetric Encryption**
A cryptographic system using a pair of keys — a public key for encryption and a private key for decryption (or vice versa for signing). Used in JWT signing (RS256, ES256), TLS handshake, and mTLS. Unlike symmetric encryption, the private key is never shared.

### **Symmetric Encryption**
A cryptographic system where the same secret key is used for both encryption and decryption. Used in JWT signing (HS256), TLS session encryption, and local data encryption. Requires secure key distribution between parties.

### **ECDSA (Elliptic Curve Digital Signature Algorithm)**
An asymmetric signing algorithm using elliptic curve cryptography. Provides equivalent security to RSA with smaller key sizes and faster computation. Used in JWT (ES256, ES384), TLS certificates, and blockchain. Preferred over RSA in modern systems.

### **Active Directory (AD)**
Microsoft's directory service for Windows domain networks. Stores user accounts, groups, devices, and policies. Commonly used as a user store by Identity Providers via LDAP or Kerberos federation.

### **IdP (Identity Provider)**
A system that creates, maintains, and manages identity information and provides authentication services to relying applications. Examples: Keycloak, Okta, Azure AD, Authentik, FusionAuth.

### **Kerberos**
A network authentication protocol using tickets and symmetric-key cryptography. Allows nodes to prove their identity securely over a non-secure network. Commonly used with Active Directory. Port 88.

### **LDAP (Lightweight Directory Access Protocol)**
An open, vendor-neutral protocol for accessing and maintaining distributed directory information services. Used to query and modify user records in directories like Active Directory, OpenLDAP, or 389 DS. Port 389 (LDAP) / 636 (LDAPS).

### **M2M (Machine-to-Machine)**
A communication pattern where two services interact without a human user. In OAuth2, the Client Credentials grant is designed for M2M scenarios — the client authenticates using its own credentials (not a user's).

### **SLA (Service Level Agreement)**
A formal contract defining the expected level of service between a provider and a consumer. Common metrics: uptime percentage (99.9%), latency percentiles (p99 < 200ms), error rate, throughput.

### **SLO (Service Level Objective) / Single Logout (SAML/OIDC)**
*Context-dependent*: In operations, SLO is a target reliability metric (e.g., 99.9% uptime over a quarter). In security, SLO refers to Single Logout — a SAML/OIDC profile that terminates a user's session across all Service Providers when the user logs out from one.

### **SPIFFE (Secure Production Identity Framework for Everyone)**
A standard (CNCF) for issuing cryptographic identities to workloads (services, containers, VMs). Uses a URI format `spiffe://trust-domain/workload/path`. The SPIFFE Runtime Environment (SPIRE) is a reference implementation that attests workloads and issues SVIDs (SPIFFE Verifiable Identity Documents) as X.509 or JWT tokens.

  - [SPIFFE Specification (GitHub)](https://github.com/spiffe/spiffe)
  - [SPIRE Project (CNCF)](https://spiffe.io/spire/)
  - [SPIFFE/SPIRE Quickstart](https://spiffe.io/docs/latest/try/getting-started-k8s/)
  - [SPIFFE Standards Overview](https://spiffe.io/docs/latest/spiffe-about/)

### **Caching**
The process of storing data in a cache so that future requests for that data can be served faster.

### **Idempotency**
The property of certain operations in mathematics and computer science that can be applied multiple times without changing the result beyond the initial application.

### **Rate Limiting**
Controlling the rate of requests sent or received by an application or user.

### **Throttling**
The process of limiting the number of requests a user can make within a certain time period.

### **TLS/SSL (Transport Layer Security/Secure Sockets Layer)**
Cryptographic protocols that provide communications security over a computer network. TLS (the modern standard, replacing SSL) enables encryption, authentication, and integrity for data in transit. Used in HTTPS (port 443), mTLS (mutual authentication), and secure API communication.

  - **Key Exchange**: Asymmetric cryptography (RSA, ECDHE) to establish a shared session key
  - **Session Encryption**: Symmetric cipher (AES-GCM, ChaCha20) for bulk data
  - **Handshake**: Client and server negotiate TLS version, cipher suite, and exchange certificates (X.509)

---

## 📖 Quick Reference

| Category | Key Terms | Common Use Cases |
|----------|-----------|------------------|
| **Architecture** | Microservices, Monolith, Serverless | System design decisions |
| **Data** | ACID, BASE, CAP Theorem | Database selection |
| **Operations** | CI/CD, IaC, Auto-scaling | DevOps practices |
| **Security** | OAuth2, OIDC, SAML, SCIM, JWT, JWKS, LDAP, Kerberos, AD, IdP, ECDSA, mTLS, SPIFFE, Zero Trust | AuthN/AuthZ implementation |

---

*This glossary is continuously updated. If you find missing terms or unclear definitions, please contribute to improve this resource.*
