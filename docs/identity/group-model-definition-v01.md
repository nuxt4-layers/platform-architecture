# Group Model Definition v0.1

**Status:** Normative — accepted by [ADR-0003](../decisions/ADR-0003-group-model-and-identity-first.md)  
**Authority:** Subject to the accepted ADRs and the Platform Architecture, Identity and Authorisation standards  
**Scope:** Identity groups, membership, hierarchy, resource ownership and access semantics

## 1. Purpose and terminology

A **group** is an Identity-owned collection context for zero or more users, formed for a defined purpose (for example an organisation, company, club, committee, event or department). A group is not itself the domain entity it represents. Identity owns generic groups and membership; domain capabilities own their business entities and reference public group identifiers.

An **identity** represents a principal independently of group membership. A **membership** is a separately managed association between an identity and a group. **Resource ownership** identifies the principal or group controlling a resource; **creator provenance** records who created it. **Authorisation** determines whether an actor may perform an action on a resource in context. A **tenant** is an isolation boundary, not a synonym for a group.

## 2. Personal (unary) group

Every human user identity MUST have exactly one system-managed personal group created as part of successful identity provisioning. The personal group has exactly one permanent principal member, its associated human identity. It MUST NOT be arbitrarily reassigned, transferred, joined by another user or deleted independently of the identity lifecycle. Any special delegation or recovery mechanism requires a separately authorised and audited contract. Non-human identities require an explicit lifecycle decision rather than automatic application of this human-user rule.

Creation of the identity and personal group MUST be atomic from the perspective of consumers: an incomplete identity must not become active.

## 3. Hierarchy: one parent, many children

Each group MUST have **zero or one parent** and MAY have any number of children. A group without a parent is a root. A group MUST NOT be its own ancestor; reparenting MUST reject cycles. The resulting structure is a **forest of rooted trees**. Multiple-parent relationships are prohibited. Unrelated business associations, cross-organisation collaboration and cross-group participation MUST be represented through explicit domain relationships or direct memberships, not a second parent.

The hierarchy records organisational relationships only. Neither parent-to-child nor child-to-parent access, membership or ownership inheritance is implicit.

## 4. Membership and lifecycle

An identity MAY belong directly to any number of ordinary groups, including groups in different trees, independently of its personal group. Memberships MUST have explicit identity/group references and lifecycle status. Changes to membership MUST be authorised, attributable and auditable. Ending one membership MUST NOT alter unrelated memberships or the identity itself.

The Identity contract SHOULD support active, suspended and ended memberships and effective-time semantics where required. Whether a hierarchy confers effective membership must be an explicit, versioned policy, not inferred from parentage.

## 5. Resource ownership and provenance

A domain resource MUST have an explicit ownership/access model appropriate to its domain and isolation context. A company-owned report remains company-owned when its creator leaves the company. The resource MAY record both an owner group ID and a separate creator identity ID; the creator ID MUST NOT itself imply continuing access.

Domain services own their data, schemas and migrations, as required by ADR-0002. Cross-capability references use public identifiers, not cross-schema foreign keys or private database access.

## 6. Authorisation and inheritance

All protected operations MUST be authorised on the server using the relevant actor, action, resource, context, policy, active memberships and explicit grants. Membership alone does not imply unrestricted access. Group hierarchy MUST NOT confer privileges by default. Explicit inheritance rules, if introduced, MUST define direction, scope, exceptions, revocation and negative tests. Tenant and resource isolation remain separate checks; a client-supplied group or tenant identifier is never proof of authority.

## 7. Departure and revocation

When a membership ends, all permissions **derived from that membership** MUST cease to be effective, including expressly inherited or delegated grants whose validity depends on it. Implementations MUST account for cached authorisation decisions, session claims, long-lived tokens and asynchronous projections, using a documented revocation consistency guarantee and fail-closed behaviour for sensitive operations.

Revocation does not remove access obtained from a genuinely independent valid grant, nor can it retract data already exported or copied. Retained business records and audit provenance remain governed by their owning capability and retention policy.

## 8. Domain integration

Organisations, companies, clubs, committees, events, departments and other domain capabilities MAY associate domain entities with Identity groups through public identifiers. The domain entity owns its business semantics, attributes and lifecycle; Identity owns generic membership and hierarchy; Authorisation owns access decisions. An associated domain entity MUST NOT silently create a second source of truth for membership.

Management interfaces are authorised projections over these contracts and do not own group state.

## 9. Security, privacy and acceptance

The model MUST meet the platform's OWASP ASVS 5.0 Level 2 baseline and applicable Level 3 controls, server-side enforcement, least privilege, auditability and privacy-by-design requirements.

Acceptance tests MUST cover: atomic personal-group provisioning; exactly-one-personal-group invariant; many direct memberships; zero/one parent and many children; cycle and second-parent rejection; no implicit privilege inheritance; isolation between companies/tenants; creator departure without ownership transfer; membership-derived revocation and cache/session invalidation; independent grants remaining independent; and unauthorised membership/hierarchy mutation rejection.

## 10. Adoption and compatibility

This document extends the Identity and Authorisation architecture and was accepted by [ADR-0003](../decisions/ADR-0003-group-model-and-identity-first.md). It does not supersede earlier accepted ADRs. Implementations MUST assess migration and compatibility impacts before introducing new mandatory group invariants. Changes to governing invariants require an accepted ADR or explicit normative architecture revision.
