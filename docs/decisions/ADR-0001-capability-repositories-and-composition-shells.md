# ADR-0001 — Capability Repositories and Thin Composition Shells

**Status:** Accepted  
**Date:** 2026-09-26

## Context

The ecosystem is expected to host multiple Nuxt capabilities, including shared UI, authentication, identity, authorization, privacy, dashboard/workspace functionality and independently meaningful applications such as Architecture Registry.

Keeping every capability in one host repository would make independent reuse and versioning harder. At the other extreme, giving every component its own repository would create unnecessary maintenance and dependency overhead.

## Decision

The `nuxt4-layers` organisation will host separately versioned repositories for cohesive bounded capabilities where independent development provides material value.

Nuxt applications will remain lightweight composition roots. They select compatible capability versions, provide application and deployment configuration, connect infrastructure and own application-specific routing and branding.

Cross-layer interaction occurs through declared public contracts. Repository separation does not permit consumers to depend upon another capability's private implementation.

The default runtime remains a modular monolith. A separate repository does not mean that the capability must run as a separate service.

## Consequences

### Positive

- capability reuse across multiple Nuxt applications;
- independent testing and release;
- explicit compatibility boundaries;
- clearer ownership and encapsulation;
- infrastructure and host applications can evolve independently.

### Costs

- additional repository/release administration;
- explicit version compatibility management;
- cross-repository changes may require coordinated releases;
- contract design becomes a mandatory engineering discipline.

## Guardrails

A new repository requires a bounded-capability justification. Microscopic repositories are discouraged.

The dependency graph must remain acyclic, and application-specific integrations should normally be wired at the composition root rather than introducing mutual domain dependencies.
