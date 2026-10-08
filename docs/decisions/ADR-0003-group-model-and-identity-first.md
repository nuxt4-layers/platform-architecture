# ADR-0003 — Group Model and Identity Before Logging

**Status:** Accepted  
**Date:** 2026-10-08

## Context

The [Group Model Definition v0.1](../identity/group-model-definition-v01.md) was added as a proposed extension to the Identity and Authorisation architecture. It defines personal (unary) groups, single-parent group hierarchies, membership lifecycle, the separation of ownership from creator provenance and of tenants from groups, the absence of implicit hierarchy inheritance, and revocation on departure. Its §10 requires an accepted ADR before its invariants become normative.

The Authorisation capability (`nuxt4-layers/authorisation`, contract version 2) has already been reconciled with that definition: hierarchy inheritance is opt-in per role assignment, tenant isolation is a separate check, creator provenance grants nothing, only active memberships count, and its directory port carries a revocation consistency guarantee.

The [Capability Development Catalogue v0.3](../catalogue/nuxt-layers-capability-development-catalogue-v0.3.md) §10 placed Logging Service next. Authorisation's persistence and server functions, and Authentication's principal-to-actor integration, cannot be completed against a real implementation until Identity provides users, groups, hierarchy and memberships.

## Decision

1. The Group Model Definition v0.1 is **accepted as normative architecture**. Its invariants and §9 acceptance criteria apply to Identity, Authorisation and any capability that associates domain entities with groups.
2. **Identity** is the next capability to be developed, before Logging Service. Identity implements the group model; Authorisation consumes it through its directory port.
3. Logging Service follows Identity as the next foundational reference capability.

## Consequences

### Positive

- Identity is built against settled invariants rather than a proposal.
- Authorisation's phase 2 and Authentication's host integration are unblocked as soon as Identity's contract exists.
- The catalogue's development order matches the order in which capabilities are actually needed.

### Costs

- Identity and Authorisation must meet the group model's acceptance tests, including membership-derived revocation and cache invalidation.
- Logging Service, and the structured audit sinks that would consume capabilities' events, are deferred; capabilities continue to emit events to host-supplied sinks.

## Guardrails

- Changes to the group model's invariants require a new ADR or an explicit normative revision of the definition.
- Tenant provisioning and lifecycle remain a candidate `tenancy-service`; until one exists, tenants are supplied through Identity and the composition root.
