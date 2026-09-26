# Security Architecture v0.1

**Status:** Normative baseline

## 1. Baseline

The ecosystem adopts OWASP ASVS Level 2 as the initial verification target for applications handling authenticated user data, unless a stronger application-specific requirement applies.

The exact ASVS release used by verification MUST be recorded by the consuming project so the baseline remains reproducible.

## 2. Principles

- secure by default;
- least privilege;
- deny by default for protected resources;
- server-side authorization;
- defence in depth;
- explicit trust boundaries;
- minimal attack surface;
- dependency and supply-chain hygiene;
- secrets never committed to repositories;
- auditable sensitive operations;
- safe failure without disclosure of sensitive internals.

## 3. Web controls

Applications SHOULD implement, as applicable:

- secure, HttpOnly and appropriately SameSite session cookies;
- CSRF protection where the authentication mechanism creates CSRF exposure;
- strict input validation;
- output encoding;
- Content Security Policy;
- appropriate security headers;
- rate limiting/abuse controls;
- secure password handling if passwords are supported;
- MFA/passkeys for elevated-risk accounts or actions;
- session expiry, rotation and revocation;
- reauthentication for sensitive account/security changes.

## 4. Trust boundaries

At minimum, threat modelling SHOULD consider browser, Nuxt/Nitro server, database, third-party provider, CI/CD, package supply chain and administrator boundaries.

## 5. Secrets

Secrets MUST be supplied through deployment/runtime secret management and MUST NOT be exposed through client runtime configuration.

## 6. Logging

Security-relevant events SHOULD be auditable without logging credentials, session secrets, unnecessary personal data, or other sensitive values.

## 7. Verification

Security claims SHOULD be backed by automated tests where feasible and periodic manual review where automation is insufficient.
