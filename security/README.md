# 🔒 Security Patterns

Security must be a first-class citizen in software architecture, integrated from day one rather than treated as a peripheral layer. A secure system relies on rigorous identity verification, robust token management, and defensive architectural principles.

---

## Authentication vs Authorization

The distinction between verification of identity and validation of permissions is fundamental to all secure architectures:

- **Authentication (AuthN — *Identity*)** answers **"Who are you?"** — username/password, MFA (TOTP, WebAuthn), federated logins (SAML 2.0, OIDC).
- **Authorization (AuthZ — *Permissions*)** answers **"What are you allowed to do?"** — RBAC (Role-Based Access Control), ABAC (Attribute-Based Access Control).

---

## 🗺️ Table of Contents

| # | Article | Content |
|---|---------|---------|
| 1 | [OAuth 2.0](./oauth2.md) | Grant types, PKCE, Client Credentials, Device Grant, DPoP, PAR, Revocation, Introspection, Token Exchange, Consent |
| 2 | [OpenID Connect (OIDC)](./oidc.md) | ID Token, scopes and claims, UserInfo, Discovery/JWKS, logout (RP + backchannel), session management |
| 3 | [SAML 2.0](./saml.md) | Architecture, assertions, bindings, metadata, profiles, SAML vs OIDC comparison |
| 4 | [SCIM 2.0](./scim.md) | User/Group schema, provisioning lifecycle, REST endpoints, filtering, SCIM vs manual |
| 5 | [JWT Best Practices](./jwt.md) | Anatomy, JWS vs JWE, token storage, claims verification, JWKS rotation, algorithm confusion, RTR |
| 6 | [Zero Trust Architecture](./zero-trust.md) | Core principles, PDP/PEP, mTLS & microsegmentation, access flow |
| 7 | [Authentication Providers](./providers.md) | Keycloak, Authentik, FusionAuth, Okta — features, architecture, comparison |
| 8 | [OWASP Top 10](./owasp.md) | Risk catalog with mitigation strategies |
| 9 | [Implementation Checklist](./implementation-checklist.md) | Go-live reference: endpoints, config, pitfalls, testing |

---

[⬅️ Back to Home](../README.md)
