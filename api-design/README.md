# 🔌 API Design

API (Application Programming Interface) design is a crucial part of software architecture, defining how different software components interact with each other.

---

## 🗺️ Table of Contents
1. [REST Architecture](#1-rest-architecture)
2. [GraphQL](#2-graphql)
3. [gRPC](#3-grpc)
4. [API Design Principles](#4-api-design-principles)

---

## 1. REST Architecture
Representational State Transfer (REST) is an architectural style that defines a set of constraints to be used for creating web services.

- **Stateless**: Each request from client to server must contain all the information necessary to understand and complete the request.
- **Resources**: Everything is a resource identified by a URI (Uniform Resource Identifier).
- **HTTP Methods**:
    - `GET`: Retrieve a resource.
    - `POST`: Create a new resource.
    - `PUT`: Update/Replace an existing resource.
    - `PATCH`: Partially update a resource.
    - `DELETE`: Remove a resource.

---

## 2. GraphQL
GraphQL is a query language for APIs and a runtime for fulfilling those queries with your existing data.

- **Ask for what you need**: Clients can request exactly the fields they need, nothing more.
- **Single Endpoint**: Usually uses a single `/graphql` endpoint.
- **Typed Schema**: Uses a strong type system to define the capabilities of the API.

---

## 3. gRPC
gRPC is a modern open-source high-performance Remote Procedure Call (RPC) framework that can run in any environment.

- **Protocol Buffers**: Uses Protobuf as the interface description language and message interchange format.
- **HTTP/2**: Leverages HTTP/2 for transport, enabling features like multiplexing and streaming.
- **Strong Typing**: Generation of client/server stubs in multiple languages.

---

## 4. API Design Principles

### Versioning
- **URL Versioning**: `/v1/users`
- **Header Versioning**: `Accept: application/vnd.myapi.v1+json`

### Error Handling
- Use standard HTTP Status Codes (`200 OK`, `201 Created`, `400 Bad Request`, `401 Unauthorized`, `404 Not Found`, `500 Internal Server Error`).
- Provide meaningful error messages in the response body.

### Idempotency
An operation is idempotent if it can be performed multiple times without changing the result beyond the initial application.
> [!TIP]
> [Read the full guide on API Idempotency](./idempotency.md)
