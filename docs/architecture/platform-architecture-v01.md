# Platform Architecture v0.1

**Status:** Initial normative baseline  
**Scope:** `nuxt4-layers` ecosystem  
**Version:** 0.1

## 1. Purpose

The ecosystem consists of bounded capabilities, implemented as Nuxt Layers, that can evolve independently and are combined into lightweight Nuxt applications. Each application acts as a composition root: it selects the capabilities it needs, supplies configuration, connects infrastructure and owns deployment-specific concerns.

This architecture keeps reusable ecosystem rules separate from the routing, branding, product and deployment choices of any one application.

## 2. Architectural model

The system distinguishes four concepts.

### 2.1 Foundation capabilities

Cross-cutting capabilities expected to be reusable by many applications. Initial examples include UI, authentication, identity, authorization, and privacy.

### 2.2 Platform capabilities

Capabilities commonly required by a composed platform but not necessarily universal, such as dashboard/workspace composition, notifications, settings, audit presentation, or article management.

### 2.3 Application/domain capabilities

Bounded business or application domains, such as an Architecture Registry or a game.

### 2.4 Composition applications

Lightweight Nuxt applications that combine capabilities, connect infrastructure, define application-specific routes, provide branding and environment configuration, and decide how the application is deployed.

A composition application MUST NOT become the default location for domain behaviour that belongs to a bounded capability.

## 3. Core invariants

1. A layer represents a bounded capability, not merely a convenient collection of files.
2. Cross-layer interaction MUST use declared public contracts.
3. A layer MUST NOT import another layer's private implementation.
4. The layer dependency graph MUST be directed and acyclic.
5. Composition is preferred over deep layer inheritance.
6. Authentication, identity, and authorization are distinct capabilities.
7. Client-side access controls are user-experience controls, not security boundaries.
8. Authoritative authorization MUST be enforced server-side.
9. Persistent resources requiring isolation MUST have explicit ownership/access semantics.
10. Infrastructure providers MUST be replaceable where practical through ports/adapters or equivalent boundaries.
11. Security, privacy, accessibility and internationalisation are architectural properties, not late additions.
12. User-customisable presentation MUST preserve mandatory accessibility constraints.
13. Public presentation SHOULD use semantic HTML and appropriate structured metadata.
14. A separately deployed service is not required merely because a capability is separately bounded.

## 4. Modular-monolith default

The default deployment architecture is a modular monolith. Capability separation is logical and contractual; it does not imply network distribution.

A capability MAY later become a separately deployed service when operational, scaling, security, or ownership requirements justify the added distributed-systems cost.

## 5. Contract boundaries

Each independently consumed layer MUST declare, as applicable:

- capabilities it provides;
- capabilities it requires;
- public domain types;
- commands and queries;
- events;
- configuration;
- public errors;
- compatibility requirements.

Internal database models, framework integration code, implementation services and provider SDKs MUST NOT become public contracts by accident. Making any of them public requires an explicit architectural decision.

## 6. Identity and access

Authentication establishes who has signed in and manages the authenticated session. Identity models users, groups, memberships and related identity information. Authorization decides whether an actor may perform an action on a resource.

These concerns MUST remain separable even where one implementation package supplies integrations between them.

The architecture MUST support a user belonging to zero, one, or multiple groups. Resource access MUST be capable of expressing user ownership, group ownership/membership, roles or permissions, and resource-specific policy where required.

## 7. Data and infrastructure

Domain capabilities SHOULD depend upon repository/service ports rather than directly upon a hosted database vendor API.

PostgreSQL is the preferred relational database for initial platform applications because it works with managed hosting and can later move to containerised or self-hosted deployment. Database row-level security MAY add another layer of protection, but it MUST NOT replace application-level authorization.

## 8. Routing principle

Reusable layers SHOULD expose route capabilities without assuming one universal host URL hierarchy.

A consuming application MAY distinguish public and authenticated management projections. For example, an application may expose a public resource under one namespace and its management surface under an authenticated workspace namespace. Concrete route choices are application architecture unless explicitly standardized by this repository.

## 9. Presentation, internationalisation and semantics

WCAG 2.2 Level AA is the minimum accessibility engineering target for web presentation. Internationalisation MUST be supported as a standard platform capability even where an initial application enables only one locale. Reusable UI MUST use semantic presentation primitives and a coherent design-token model. User preferences MAY alter colour schemes, typography, density and selected layout characteristics but MUST NOT invalidate accessibility requirements. Public-facing capabilities MUST prefer semantic HTML and SHOULD provide appropriate Schema.org structured data from authoritative domain data.

## 10. Security and privacy

The minimum application-security target is OWASP ASVS 5.0 Level 2 across the platform. Security-sensitive capabilities and operations SHALL also apply relevant Level 3 requirements where they are technically and operationally appropriate.

NIST CSF 2.0 structures the overall security programme. NIST SP 800-218 SSDF guides secure development, and NIST SP 800-63-4 guides digital identity assurance. Cost constraints MUST NOT silently weaken a required security control. If a control must be deferred, the gap and its risk treatment MUST be recorded explicitly.

Security controls MUST be layered across browser, server, persistence, dependency/supply-chain and deployment boundaries.

Privacy MUST follow data-protection-by-design principles. Storage/access technologies, telemetry and third-party integrations MUST be purpose-classified rather than introduced implicitly.

## 11. Evolution

Architecture changes that alter an invariant, public contract model, dependency rule, or ecosystem-wide standard require an ADR or an explicit revision to the governing normative document.

The architecture is expected to evolve; compatibility and migration MUST be explicit rather than accidental.
