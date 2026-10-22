# Platform Architecture v0.1

**Status:** Initial normative baseline  
**Scope:** `nuxt4-layers` ecosystem  
**Version:** 0.1

## 1. Purpose

The ecosystem consists of bounded capabilities, implemented as Nuxt Layers, that can evolve independently and are combined into lightweight Nuxt applications. Each application acts as a composition root: it selects the capabilities it needs, supplies configuration, connects infrastructure and owns deployment-specific concerns.

This architecture keeps reusable ecosystem rules separate from the routing, branding, product and deployment choices of any one application.

## 2. Architectural model

The system distinguishes five concepts. Capabilities (§2.1 to §2.3) are headless ([ADR-0009](../decisions/ADR-0009-headless-capability-layers.md)); presentation (§2.4) draws their screens.

### 2.1 Foundation capabilities

Cross-cutting capabilities expected to be reusable by many applications, such as authentication, identity, authorisation, profile, theme management and privacy.

### 2.2 Platform capabilities

Capabilities commonly required by a composed platform but not necessarily universal, such as notifications, configuration, audit or publishing.

### 2.3 Application/domain capabilities

Bounded business or application domains, such as an Architecture Registry or a game.

### 2.4 Presentation and libraries

Presentation layers draw screens over capabilities' contracts: `ui`, the component library; capability kits (`<capability>-ui`); composition kits; and `content-editor`. Stateless libraries, such as `rich-text`, hold no state, ports or screens ([ADR-0011](../decisions/ADR-0011-shared-rich-text-and-configurable-copy.md) §7).

### 2.5 Composition applications

Lightweight Nuxt applications that combine capabilities, connect infrastructure, define application-specific routes, provide branding and environment configuration, and decide how the application is deployed.

A composition application MUST NOT become the default location for domain behaviour that belongs to a bounded capability.

## 3. Core invariants

1. A layer represents a bounded capability, not merely a convenient collection of files.
2. Cross-layer interaction MUST use declared public contracts.
3. A layer MUST NOT import another layer's private implementation.
4. The layer dependency graph MUST be directed and acyclic.
5. Composition is preferred over deep layer inheritance.
6. Authentication, identity, and authorisation are distinct capabilities.
7. Client-side access controls are user-experience controls, not security boundaries.
8. Authoritative authorisation MUST be enforced server-side.
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

Authentication establishes who has signed in and manages the authenticated session. Identity models users, groups, memberships and related identity information. Authorisation decides whether an actor may perform an action on a resource.

These concerns MUST remain separable even where one implementation package supplies integrations between them.

The architecture MUST support a user belonging to zero, one, or multiple groups. Resource access MUST be capable of expressing user ownership, group ownership/membership, roles or permissions, and resource-specific policy where required.

## 7. Data and infrastructure

Domain capabilities SHOULD depend upon repository/service ports rather than directly upon a hosted database vendor API.

PostgreSQL is the preferred relational database for initial platform applications because it works with managed hosting and can later move to containerised or self-hosted deployment. Database row-level security MAY add another layer of protection, but it MUST NOT replace application-level authorisation.

## 8. Routing principle

Capability layers define no routes ([ADR-0009](../decisions/ADR-0009-headless-capability-layers.md)); their HTTP APIs and protocol endpoints sit under their own namespace. Pages and routes belong to the application, which MAY separate public pages from authenticated administration, for example on different hosts.

## 9. Presentation, internationalisation and semantics

WCAG 2.2 Level AA is the minimum accessibility engineering target for web presentation. Internationalisation MUST be supported as a standard platform capability even where an initial application enables only one locale. Reusable UI MUST use semantic presentation primitives and Theme Manager's design-token vocabulary. User preferences MAY alter colour schemes, typography, density and selected layout characteristics but MUST NOT invalidate accessibility requirements. Public-facing capabilities MUST prefer semantic HTML and SHOULD provide appropriate Schema.org structured data from authoritative domain data.

## 10. Security and privacy

The minimum application-security target is OWASP ASVS 5.0 Level 2 across the platform. Security-sensitive capabilities and operations SHALL also apply relevant Level 3 requirements where they are technically and operationally appropriate.

NIST CSF 2.0 structures the overall security programme. NIST SP 800-218 SSDF guides secure development, and NIST SP 800-63-4 guides digital identity assurance. Cost constraints MUST NOT silently weaken a required security control. If a control must be deferred, the gap and its risk treatment MUST be recorded explicitly.

Security controls MUST be layered across browser, server, persistence, dependency/supply-chain and deployment boundaries.

Privacy MUST follow data-protection-by-design principles. Storage/access technologies, telemetry and third-party integrations MUST be purpose-classified rather than introduced implicitly.

## 11. Evolution

Architecture changes that alter an invariant, public contract model, dependency rule, or ecosystem-wide standard require an ADR or an explicit revision to the governing normative document.

The architecture is expected to evolve; compatibility and migration MUST be explicit rather than accidental.
