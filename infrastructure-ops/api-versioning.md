# 🔧 API Versioning & Evolution

API versioning is a critical component of API design that allows developers to improve and evolve an API without breaking existing client integrations. 

---

## 🗺️ Table of Contents
1. [When to Version?](#1-when-to-version)
2. [Versioning Strategies](#2-versioning-strategies)
3. [Backward Compatibility](#3-backward-compatibility)
4. [Deprecation & Sunsetting](#4-deprecation--sunsetting)

---

## 1. When to Version?
You should introduce a new API version only when making **breaking changes**.

### What constitutes a breaking change?
- Removing or renaming an endpoint.
- Removing or renaming a field in the request/response payload.
- Changing the data type of a field (e.g., changing an integer to a string).
- Adding a new **required** field to a request payload.
- Changing authentication mechanisms or required permissions.

### Non-breaking changes (No new version needed)
- Adding new endpoints.
- Adding new optional fields to a request.
- Adding new fields to a response.
- Fixing a bug (unless clients relied on the buggy behavior).

---

## 2. Versioning Strategies

### 1. URI Path Versioning (Most Common)
The version is explicitly included in the URL path.
- **Example**: `GET /api/v1/users`
- **Pros**: Very visible, easy to test in a browser, easy to route at the API Gateway level.
- **Cons**: Can lead to URL bloat, doesn't adhere strictly to REST principles (a URI should represent a resource, not a version).

### 2. Header Versioning (Custom Header)
A custom HTTP header specifies the version.
- **Example**: 
  ```http
  GET /api/users
  X-API-Version: 1
  ```
- **Pros**: Keeps URLs clean, adheres to REST principles.
- **Cons**: Harder to test manually (requires tools like cURL or Postman), caching can be tricky (requires `Vary: X-API-Version`).

### 3. Media Type / Accept Header Versioning (Content Negotiation)
Clients use the `Accept` header to request a specific version of the resource representation.
- **Example**: 
  ```http
  GET /api/users
  Accept: application/vnd.mycompany.v1+json
  ```
- **Pros**: Architecturally the most "pure" REST approach.
- **Cons**: Most complex to implement and test.

### 4. Query Parameter Versioning
The version is passed as a query parameter.
- **Example**: `GET /api/users?version=1`
- **Pros**: Easy to implement and test.
- **Cons**: Can interfere with query caching and is generally considered bad practice for REST APIs.

---

## 3. Backward Compatibility Strategies
The goal of API evolution is to avoid versioning as much as possible by designing for backward compatibility.

- **Additive Changes Only**: Always prefer adding fields rather than modifying or deleting existing ones.
- **Tolerant Reader Pattern**: Clients should be designed to ignore unknown fields in JSON responses.
- **Default Values**: If adding a new field to a request, make it optional with a sensible default value so older clients aren't forced to send it.
- **GraphQL**: If you frequently struggle with REST API versioning, consider GraphQL. It avoids versioning by allowing clients to query exactly what they need, and fields can be easily marked as `@deprecated`.

---

## 4. Deprecation & Sunsetting
When you *must* release a new version, you need a clear strategy to migrate users off the old one.

1. **Announce**: Communicate the change well in advance (e.g., 6 months).
2. **Deprecate**: Mark the old endpoints as deprecated.
   - Use the `Deprecation` HTTP Header: `Deprecation: @1609459200` (Unix timestamp).
   - Use the `Link` Header to point to documentation: `Link: <https://api.example.com/docs/v2>; rel="deprecation"`.
3. **Sunset**: The date the API will stop working.
   - Use the `Sunset` HTTP Header to inform clients of the exact shutdown date: `Sunset: Sat, 31 Dec 2022 23:59:59 GMT`.
4. **Shutdown**: Return a `410 Gone` HTTP status code when clients try to access the retired version.

---

[⬅️ Back to Infrastructure & Ops](./README.md)
