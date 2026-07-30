# OAuth2 / OIDC Implementation Checklist

A practical go-live reference covering endpoints, configuration, common pitfalls, and testing strategy.

---

## Endpoint Checklist

Ensure your Authorization Server exposes these endpoints before going live:

| Endpoint | Path Convention | Required | Purpose |
|---|---|---|---|
| Authorization | `/authorize` | ✅ Yes | Browser redirect for user authentication |
| Token | `/token` | ✅ Yes | Exchange auth code, refresh token, or client credentials for tokens |
| JWKS | `/jwks` | ✅ Yes | Publish public signing keys for token verification |
| OpenID Discovery | `/.well-known/openid-configuration` | ✅ Yes (OIDC) | Metadata about IdP capabilities |
| UserInfo | `/userinfo` | ✅ Yes (OIDC) | Fetch additional user claims with access token |
| Revocation | `/revoke` | ⚠️ Recommended | Allow clients to revoke tokens on logout |
| Introspection | `/introspect` | ⚠️ Recommended | Validate opaque tokens for resource servers |
| Registration | `/register` | Optional | Dynamic client registration |

---

## Configuration Checklist

### Token Lifetimes

| Token | Recommended | Rationale |
|---|---|---|
| Access Token | 5-15 minutes | Short window for stolen tokens |
| Refresh Token | 7-30 days | Long-lived but revocable; rotate on use |
| ID Token | Hours (session-bound) | Matches user session length |
| Authorization Code | 1-5 minutes | Ephemeral; single-use |

### Signing Algorithms

- Use `ES256` (ECDSA) or `RS256` (RSA) for signing
- Reject `none` algorithm
- Reject symmetric algorithms (`HS256`) for multi-service verification
- Rotate signing keys quarterly

### Cookie Settings (for browser-based apps)

- `HttpOnly` — prevent JavaScript access
- `Secure` — only send over HTTPS
- `SameSite=Strict` (or `Lax`) — CSRF protection
- `Path` — restrict to the minimum necessary path
- `__Host-` prefix when applicable

### CORS

- Explicit allowlist of origins (never `*`)
- Vary allowed methods per endpoint
- Block credentials on wildcard origins

---

## Common Pitfalls

### 1. Algorithm Confusion Attack

The attacker changes the JWT header `alg` from `RS256` to `HS256`, tricking the server into using the public key as an HMAC secret.

**Fix**: Enforce an algorithm allowlist server-side. Never trust the `alg` header from the token.

### 2. Clock Skew

If the Resource Server's clock is significantly different from the IdP's, valid tokens may be rejected (exp claim) or expired tokens may be accepted.

**Fix**: Allow a small leeway (e.g., 30 seconds) but no more than 5 minutes. Use NTP on all servers.

### 3. Token Leakage in URLs

Authorization codes and tokens in the URL fragment or query string can be leaked via browser history, referrer headers, or server logs.

**Fix**: Use POST bindings where possible; strip fragments on the client after extraction; use PAR (RFC 9126) to move payloads to the back channel.

### 4. Missing Audience Validation

A token issued for one API is accepted by another API, because the server only verified the signature.

**Fix**: Always validate the `aud` claim matches your specific API identifier.

### 5. Refresh Token Not Rotated

A stolen refresh token can be used indefinitely.

**Fix**: Implement Refresh Token Rotation (RTR) with breach detection.

### 6. Overly Permissive CORS

`Access-Control-Allow-Origin: *` with credentials enabled.

**Fix**: Never use wildcard origins with credentials. Set explicit, validated origins.

---

## Testing Strategy

### Integration Tests

| Test | What to Verify |
|---|---|
| Full auth flow | User authenticates, receives code, exchanges for tokens, calls API |
| Token refresh | Refresh token produces a new valid access token and a new refresh token |
| Token revocation | After revoke, the token is rejected by the resource server and token endpoint |
| Invalid credentials | Wrong password triggers 401, not a token |
| Expired token | Token past `exp` is rejected with 401 |

### Negative Tests

| Test | Expected Result |
|---|---|
| Missing `code_verifier` | Token exchange fails |
| Wrong `code_verifier` | Token exchange fails |
| Replayed auth code | Second exchange fails |
| Replayed refresh token (after rotation) | Entire token family revoked |
| Token with `alg: none` | Rejected (if validation is correct) |
| Token signed with wrong key | Signature validation fails |
| Missing required claim (`aud`, `iss`) | Token rejected |

### Rotation Tests

| Test | Expected Result |
|---|---|
| Old key removed from JWKS | Existing tokens remain valid until `exp` |
| Token signed with new key | Verified successfully against new JWKS entry |
| Client cache re-fetch | Cache miss triggers JWKS re-fetch, new key is accepted |

---

[⬅️ Back to Security Patterns](./README.md)
