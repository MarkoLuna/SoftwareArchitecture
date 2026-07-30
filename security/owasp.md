# OWASP Top 10

The Open Web Application Security Project (OWASP) Top 10 lists the most critical security risks facing web applications. Architects must understand these risks and integrate countermeasures directly into their designs.

---

| # | Risk | Description | Key Mitigations |
|---|---|---|---|
| 1 | **Broken Access Control** | Failure to enforce authorization rules — users accessing resources outside their permissions. | RBAC/ABAC, deny-by-default, verify every request server-side, CORS whitelist |
| 2 | **Cryptographic Failures** | Sensitive data exposure: plain-text secrets, weak hashing, missing encryption in transit. | Use TLS 1.3, encrypt PII at rest, hash passwords with bcrypt/argon2, never roll your own crypto |
| 3 | **Injection** | SQL, NoSQL, OS, LDAP injection — untrusted data sent to an interpreter. | Parameterized queries / prepared statements, input validation, least privilege for DB accounts |
| 4 | **Insecure Design** | Architecture-level flaws that cannot be fixed by implementation alone. | Threat modeling (STRIDE), secure design reviews, rate limiting, quota enforcement |
| 5 | **Security Misconfiguration** | Default credentials, verbose error messages, unnecessary open ports, missing security headers. | Automated hardening (CIS benchmarks), security header audit (`Content-Security-Policy`, `X-Frame-Options`), disable directory listing |
| 6 | **Vulnerable and Outdated Components** | Libraries with known, unpatched CVEs. | SBOM (Software Bill of Materials), dependency scanning (Dependabot, Snyk), regular updates |
| 7 | **Identification and Authentication Failures** | Weak passwords, missing MFA, improper session timeout. | Enforce MFA, implement credential stuffing protection (rate limiting, CAPTCHA), rotate session IDs after login |
| 8 | **Software and Data Integrity Failures** | Unverified CI/CD pipelines, deserialization of untrusted data, unsigned updates. | Sign artifacts, pin dependency versions, use OIDC for CI/CD auth |
| 9 | **Security Logging and Monitoring Failures** | Inadequate logging prevents detection or response to active breaches. | Centralized logging (ELK), alert on auth failures and anomalies, immutable audit trails |
| 10 | **SSRF (Server-Side Request Forgery)** | Application fetches remote resources without validating user-supplied URIs. | URL allowlist, block private IP ranges, use explicit HTTP clients with restricted redirects |

---

[⬅️ Back to Security Patterns](./README.md)
