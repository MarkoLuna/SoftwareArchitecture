# 🔒 Security Patterns

Security should be a first-class citizen in software architecture, not an afterthought.

---

## 🗺️ Table of Contents
1. [Authentication vs Authorization](#1-authentication-vs-authorization)
2. [OAuth2 and OpenID Connect](#2-oauth2-and-openid-connect)
3. [JWT (JSON Web Tokens)](#3-jwt-json-web-tokens)
4. [OWASP Top 10](#4-owasp-top-10)

---

## 1. Authentication vs Authorization

- **Authentication (AuthN)**: Verification of identity. "Who are you?" (e.g., Login, MFA).
- **Authorization (AuthZ)**: Verification of permissions. "What are you allowed to do?" (e.g., RBAC, ABAC).

---

## 2. OAuth2 and OpenID Connect

- **OAuth2**: An authorization framework that enables applications to obtain limited access to user accounts on an HTTP service.
- **OpenID Connect (OIDC)**: An identity layer on top of the OAuth 2.0 protocol. It allows Clients to verify the identity of the End-User.

---

## 3. JWT (JSON Web Tokens)
JWT is an open standard that defines a compact and self-contained way for securely transmitting information between parties as a JSON object.

- **Header**: Type and signing algorithm.
- **Payload**: Claims (user info, expiration).
- **Signature**: Used to verify that the sender of the JWT is who it says it is and to ensure that the message wasn't changed along the way.

---

## 4. OWASP Top 10
The OWASP Top 10 is a standard awareness document for developers and web application security. It represents a broad consensus about the most critical security risks to web applications.

1. **Broken Access Control**
2. **Cryptographic Failures**
3. **Injection**
4. **Insecure Design**
5. **Security Misconfiguration**
6. **Vulnerable and Outdated Components**
7. **Identification and Authentication Failures**
8. **Software and Data Integrity Failures**
9. **Security Logging and Monitoring Failures**
10. **Server-Side Request Forgery (SSRF)**
