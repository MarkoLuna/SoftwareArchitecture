# OpenID Connect (OIDC)

OAuth 2.0 is designed purely for **authorization** (delivering an access token that acts like a valet key). It does not tell the application *who* the user is. **OpenID Connect (OIDC)** extends OAuth 2.0 by introducing an identity layer.

---

## Core Concepts

### ID Token

A JWT containing structured claims about the identity of the authenticated user. Intended to be parsed and consumed by the *Client Application* to establish user sessions.

Standard claims:

| Claim | Description | Required |
|---|---|---|
| `iss` | Issuer identifier (exact URL of the IdP) | Yes |
| `sub` | Unique user identifier (never reused) | Yes |
| `aud` | Audience — the client ID that requested the token | Yes |
| `exp` | Expiration time | Yes |
| `iat` | Issued at time | Yes |
| `auth_time` | Time of authentication | No |
| `nonce` | Value passed in the auth request, included to prevent replay | No |
| `acr` | Authentication Context Class Reference | No |
| `amr` | Authentication Methods References | No |
| `azp` | Authorized party — the client that received the token | No |

### Access Token vs ID Token

| Token | Consumer | Purpose |
|---|---|---|
| **ID Token** (`id_token`) | Client App (frontend) | Authentication — establish who the user is |
| **Access Token** (`access_token`) | Resource Server (API) | Authorization — allow access to resources |

The client application should treat the access token as opaque and never parse it.

### UserInfo Endpoint

A standardized endpoint (`/userinfo`) on the Identity Provider where clients send the `access_token` to fetch additional profile claims.

```
GET /userinfo
Authorization: Bearer <access_token>

Response:
{
  "sub": "user123",
  "name": "Jane Doe",
  "email": "jane@example.com",
  "picture": "https://idp.example.com/avatars/user123.jpg"
}
```

---

## Scopes and Claims

### Standard OIDC Scopes

| Scope | Returns |
|---|---|
| `openid` | Required — signals OIDC request; returns `sub` |
| `profile` | `name`, `family_name`, `given_name`, `preferred_username`, `picture`, `updated_at` |
| `email` | `email`, `email_verified` |
| `address` | `address` (JSON object) |
| `phone` | `phone_number`, `phone_number_verified` |

### Scope-to-Claim Mapping

Scopes are authorization concepts (what the client is allowed to access). Claims are assertions about the user. The IdP maps requested scopes to the claims returned in the ID Token or UserInfo response.

```
Scope "profile" → Claims: name, family_name, picture
Scope "email"  → Claims: email, email_verified
```

### Custom Scopes and Claims

For custom business requirements, define scopes that map to custom claims in the IdP configuration:

```
Scope "employee" → Claims: department, role, employee_id
Scope "finance"  → Claims: cost_center, budget, approver_limit
```

---

## Discovery and JWKS

### OpenID Discovery (`/.well-known/openid-configuration`)

A standardized metadata endpoint that describes the IdP's capabilities.

```json
{
  "issuer": "https://idp.example.com",
  "authorization_endpoint": "https://idp.example.com/authorize",
  "token_endpoint": "https://idp.example.com/token",
  "userinfo_endpoint": "https://idp.example.com/userinfo",
  "jwks_uri": "https://idp.example.com/jwks",
  "registration_endpoint": "https://idp.example.com/register",
  "scopes_supported": ["openid", "profile", "email"],
  "response_types_supported": ["code"],
  "grant_types_supported": ["authorization_code", "client_credentials"],
  "subject_types_supported": ["public", "pairwise"],
  "id_token_signing_alg_values_supported": ["RS256", "ES256"],
  "claims_supported": ["sub", "name", "email", "picture"]
}
```

### JWKS (JSON Web Key Set)

The IdP publishes its public signing keys at the `jwks_uri` endpoint. Clients and Resource Servers fetch this to verify token signatures.

```json
{
  "keys": [
    {
      "kty": "RSA",
      "use": "sig",
      "kid": "key-id-1",
      "n": "base64url-encoded-modulus",
      "e": "AQAB",
      "alg": "RS256"
    },
    {
      "kty": "RSA",
      "use": "sig",
      "kid": "key-id-2",
      "n": "base64url-encoded-modulus",
      "e": "AQAB",
      "alg": "RS256"
    }
  ]
}
```

### JWKS Caching Strategy

- Fetch JWKS on startup and cache in-memory
- Use the `kid` (Key ID) in the JWT header to select the key
- Re-fetch only on signature validation failure (key rotation)
- Set a reasonable cache TTL (e.g., 1 hour) and re-fetch proactively

---

## OIDC Logout

### RP-Initiated Logout

The client (Relying Party) redirects the user to the IdP's end_session endpoint to terminate the session.

```
GET /logout?
  id_token_hint=<id_token>&
  post_logout_redirect_uri=https://client.example/logged-out&
  state=xyz789
```

- `id_token_hint`: The user's current ID Token (identifies the session to terminate)
- `post_logout_redirect_uri`: Where to return after logout (must be registered)
- `state`: Opaque value for CSRF protection

### Backchannel Logout (OIDC Back-Channel Logout)

The IdP sends a direct HTTP POST to a registered `backchannel_logout_uri` on the client when a user's session is terminated server-side.

```
POST /backchannel_logout
Content-Type: application/x-www-form-urlencoded

logout_token=<JWT>
```

The `logout_token` is a JWT signed by the IdP containing:
- `sub`: User whose session was terminated
- `iss`: The IdP
- `aud`: The client
- `iat`: When it was issued
- `events`: `{ "http://schemas.openid.net/event/backchannel-logout": {} }`

The client must validate the logout_token and destroy the user's local session.

---

## Session Management

| Mechanism | Direction | Description |
|---|---|---|
| **RP-Initiated Logout** | Client → IdP | Client tells IdP to log the user out |
| **Backchannel Logout** | IdP → Client | IdP tells all clients a user was logged out |
| **Session Polling** | Client → IdP | Client periodically checks `check_session_iframe` (legacy) |
| **Single Logout (SAML term)** | Cross-protocol | OIDC backchannel logout + SAML Single Logout achieve similar goals |

---

[⬅️ Back to Security Patterns](./README.md)
