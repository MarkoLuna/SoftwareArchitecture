# 🔄 Idempotency in APIs

Idempotency is the property of certain operations in mathematics and computer science whereby they can be applied multiple times without changing the result beyond the initial application.

---

## 🗺️ Table of Contents
1. [Why Idempotency Matters](#1-why-idempotency-matters)
2. [Idempotency vs Safety](#2-idempotency-vs-safety)
3. [HTTP Methods and Idempotency](#3-http-methods-and-idempotency)
4. [Implementation Strategies](#4-implementation-strategies)

---

## 1. Why Idempotency Matters
In distributed systems, network failures are inevitable. A client might send a request, but the network connection fails before the server's response reaches the client. The client doesn't know if the request was processed or not.
- **Retries**: Idempotency allows the client to safely retry the request without worrying about side effects (like double-charging a customer).
- **Consistency**: It ensures the system reaches the same state regardless of how many times a request is processed.

---

## 2. Idempotency vs Safety
- **Safe Methods**: Operations that do not modify resources (Read-only). They are always idempotent.
- **Idempotent Methods**: Operations that *can* modify resources but do so in a way that multiple identical requests have the same effect as a single request.

| Method | Safe | Idempotent |
| :--- | :--- | :--- |
| `GET` | ✅ | ✅ |
| `HEAD` | ✅ | ✅ |
| `OPTIONS` | ✅ | ✅ |
| `PUT` | ❌ | ✅ |
| `DELETE` | ❌ | ✅ |
| `POST` | ❌ | ❌ |
| `PATCH` | ❌ | ❌ (Usually) |

---

## 3. HTTP Methods and Idempotency

### PUT (Idempotent)
`PUT` is used to replace a resource. If you `PUT` the same data multiple times, the final state of the resource is exactly the same as after the first time.

### DELETE (Idempotent)
`DELETE` is used to remove a resource. Deleting an already deleted resource still results in the resource being gone (even if the response code changes from `204` to `404`).

### POST (Non-Idempotent)
`POST` is used to create a new resource or trigger an action. Sending the same `POST` request twice will typically create two resources (e.g., two identical orders).

---

## 4. Implementation Strategies

### Idempotency Keys
The most common way to implement idempotency for `POST` requests is using an **Idempotency Key** (usually a UUID).

1. The client generates a unique key for the operation.
2. The client sends the key in an HTTP header (e.g., `Idempotency-Key: <UUID>`).
3. The server checks if it has already processed a request with that key.
4. If yes, it returns the cached response from the first request.
5. If no, it processes the request and stores the response associated with that key.

### Database Constraints
Using unique constraints (e.g., `UNIQUE` index on a natural key) can prevent duplicate records from being created by retried requests.

---
[⬅️ Back to API Design](./README.md)
