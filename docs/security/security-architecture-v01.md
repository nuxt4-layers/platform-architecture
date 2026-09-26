# Security Architecture v0.1

**Status:** Normative baseline

## 1. Assurance baseline

Security is part of the architecture and an ongoing engineering responsibility.

The platform SHALL use **OWASP ASVS 5.0 Level 2** as its minimum application-security target. Verification records MUST state the exact ASVS release and SHOULD use version-qualified requirement identifiers.

Security-sensitive capabilities and operations SHALL also apply relevant ASVS Level 3 requirements where they are technically and operationally appropriate. This includes authentication, authorization, privileged administration, session management, tenant and resource isolation, secrets and key management, and highly sensitive data handling.

A consuming application MAY impose stronger requirements but MUST NOT silently weaken this baseline.

## 2. Security programme

The platform uses one coordinated security-control programme rather than treating each standard as a separate compliance project:

- NIST Cybersecurity Framework 2.0 provides the overarching Govern, Identify, Protect, Detect, Respond and Recover structure;
- NIST SP 800-218 Secure Software Development Framework informs secure-development practice;
- OWASP ASVS 5.0 provides the application-security verification baseline;
- NIST SP 800-63-4 provides digital-identity assurance guidance;
- NIST SP 800-53, ABAC and Zero Trust guidance MAY be used as design/control references;
- ISO/IEC 27001:2022, Cyber Essentials, NHS DSPT/DTAC and HITRUST MAY be mapped as assurance or future compliance frameworks when applicable.

Reference to a framework MUST NOT be represented as certification, formal compliance or regulatory applicability unless that status has actually been established.

## 3. Principles

- secure by default;
- least privilege;
- deny by default for protected resources;
- server-side authorization;
- defence in depth;
- explicit trust boundaries;
- minimal attack surface;
- strong tenant and resource isolation;
- dependency and supply-chain hygiene;
- secrets never committed to repositories;
- auditable sensitive operations;
- safe failure without disclosure of sensitive internals;
- explicit risk treatment for deferred controls.

## 4. Digital identity

Systems with authenticated users SHOULD be designed toward NIST SP 800-63-4 AAL2 when the application's risk and identity model make that assurance level appropriate. If an application claims AAL2, it MUST meet all applicable AAL2 requirements rather than selecting only convenient controls.

Phishing-resistant authentication MUST be available for elevated-risk accounts and SHALL be required for privileged platform-administration access unless a documented risk treatment establishes an equivalent or stronger control.

The platform does not claim AAL3 merely because individual AAL3-style controls are adopted.

## 5. Web and session controls

Applications MUST implement applicable ASVS baseline controls. The architecture MUST support, as applicable, secure session cookies, CSRF protection, input validation, context-appropriate output encoding, Content Security Policy and security headers, abuse controls, contemporary password protection where passwords are supported, session expiry/rotation/server-side revocation, reauthentication or step-up authentication for sensitive operations, and protection against account enumeration and credential abuse.

Client-side access controls are user-experience mechanisms only and MUST NOT constitute the authoritative security boundary.

## 6. Authorization and isolation

Every protected operation MUST receive authoritative server-side authorization.

Multi-user and multi-tenant resources MUST clearly define ownership, tenancy and access rules. A tenant or resource identifier supplied by the client MUST NOT, by itself, grant access.

Database row-level security SHOULD be used where appropriate as an additional containment boundary, particularly for tenant or owner-isolated relational data, but MUST NOT replace application authorization policy.

Isolation controls MUST have negative tests demonstrating that one principal or tenant cannot access another's protected resources.

## 7. Trust boundaries and threat modelling

Threat modelling MUST consider, as applicable, browser/untrusted input, Nuxt/Nitro server, persistence, identity providers, third parties, CI/CD, source control, package supply chain, deployment platform, administrator boundaries and tenant boundaries.

Security-critical capabilities MUST maintain a threat model proportionate to their risk.

## 8. Secrets and cryptography

Secrets MUST be supplied through appropriate deployment/runtime secret management and MUST NOT be exposed through public client runtime configuration.

Cryptographic design MUST use contemporary, reviewed mechanisms and provider/platform primitives where suitable. Custom cryptographic algorithms or protocols MUST NOT be introduced.

Key ownership, rotation, revocation and recovery requirements MUST be explicit where application-managed cryptographic keys exist.

## 9. Logging and audit

Security-relevant events MUST be auditable at a level proportionate to risk without logging credentials, session secrets, unnecessary personal data or other prohibited sensitive values.

Authentication events, authorization-policy changes, privileged actions and security-control changes SHOULD produce structured audit records.

## 10. Secure development and supply chain

Repositories and delivery pipelines SHOULD implement proportionate controls including protected default branches and pull-request workflows, dependency/vulnerability scanning, secret scanning, static analysis, SBOM generation where useful, security-header/configuration tests, dynamic web scanning where appropriate, automated authorization/isolation tests and dependency update discipline.

Security verification MUST be part of normal development rather than a pre-release-only activity.

## 11. Control register and evidence

The ecosystem SHOULD maintain a unified security control register mapping implemented controls to applicable ASVS, NIST and other assurance-framework requirements. Each control SHOULD identify status, implementation/evidence, responsible capability, verification mechanism, known gaps and risk treatment.

## 12. Cost constraints and deferred controls

Cost may affect how or when a security control is implemented, but it MUST NOT silently weaken a required security standard.

A required control that cannot yet be implemented MUST be recorded as a known gap with its rationale, affected assets/data/capabilities, risk, compensating controls where available and intended treatment or acceptance decision.

Paid certification, commercial security products and external assurance are not baseline requirements unless required by risk, contract or law.

## 13. Conditional higher-assurance contexts

ISO/IEC 27001 certification, Cyber Essentials certification, NHS DSPT/DTAC, HITRUST, health-sector controls, independent penetration testing, specialist SIEM services or external KMS services become mandatory only when application risk, data classification, contractual obligation or applicable law requires them.

The architecture SHOULD avoid preventable design choices that would make later adoption of such controls disproportionately difficult.

Applications processing special-category, health, regulated or otherwise high-impact data MUST perform a separate applicability and risk assessment before such processing is introduced.

## 14. Incident response and recovery

Applications handling material user data SHOULD maintain proportionate incident-response and recovery procedures aligned with current NIST guidance.

Recovery requirements, backup strategy and restore testing MUST be explicit for persistent data whose loss would materially affect users or platform operation.

## 15. Verification

Security claims MUST be backed by evidence appropriate to the claim. Automated tests SHOULD be used wherever feasible and manual review MUST supplement automation where tooling cannot establish the required property.

Certification, penetration testing and external assurance MAY provide additional evidence but do not replace secure architecture and continuous verification.
