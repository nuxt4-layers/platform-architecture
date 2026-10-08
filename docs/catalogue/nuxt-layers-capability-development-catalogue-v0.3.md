# Nuxt 4 Layers — Capability Development Catalogue v0.3

**Status:** Proposed implementation catalogue; group-model extension proposed for review, not yet normative. Reconciled against Platform Architecture v0.1 and accepted ADR-0001/ADR-0002  
**Authority:** `nuxt4-layers/platform-architecture`  
**Purpose:** Practical starting point for independently developed, configurable, cohesive and loosely coupled Nuxt 4 capabilities.

## 1. Governing rules

1. Each layer implements a cohesive bounded capability.
2. Foundation, Platform and Domain/Application are the normative capability classifications.
3. Management interfaces are optional projections of capability contracts, not competing authorities.
4. Applications are thin composition roots, supplying infrastructure, configuration, routing and deployment choices.
5. Dependencies must be directed, acyclic, explicit and contract-based.
6. Public contracts should be exposed through deliberate entry points such as `/contracts`.
7. Independently versioned layers should declare capability metadata using the existing manifest standard.
8. Stateful capabilities own their persistence ports, PostgreSQL schemas and migrations. No shared persistence service is permitted.
9. Identity owns users, groups, single-parent group hierarchies and memberships; Authentication owns sessions; Authorisation owns access decisions. Human identities have one system-managed personal (unary) group under the proposed group model.
10. Required persistence must fail closed when its provider is absent.
11. All protected operations require server-side authorisation.
12. Security follows OWASP ASVS 5.0 Level 2, with applicable Level 3 controls for security-sensitive operations.
13. Applicable presentation must meet WCAG 2.2 AA, support internationalisation and use semantic design tokens.
14. Privacy, data minimisation, retention and consent requirements apply across capabilities.
15. Separate deployment is optional; the default architecture is a modular monolith.

## 2. Foundation capabilities

| Capability | Definition |
|---|---|
| `theme-manager` | Owns theme definitions, semantic design tokens, selection and configuration. Existing implementation retained. |
| `ui` | Owns reusable accessible presentation components and interaction primitives. Consumes theme contracts. |
| `identity` | Owns canonical users, one system-managed personal (unary) group per human identity, general-purpose groups, direct memberships, single-parent/multiple-child group hierarchies, group lifecycle and baseline identity/profile information. Group relationships do not automatically confer access. |
| `authentication` | Owns authentication, credential verification, authenticated sessions, rotation, revocation and reauthentication. |
| `authorisation` | Owns server-side, resource-aware access decisions using identities, effective memberships, group hierarchy, explicit grants, ownership, tenant context and policy. No automatic privilege inheritance; membership-derived access must be revocable. Existing implementation retained pending contract reconciliation. |
| `privacy` | Owns reusable privacy governance contracts, purpose classification, consent and privacy preference mechanisms. |
| `logging-service` | Owns structured diagnostic logging, context, redaction, severity and replaceable transports. |
| `uuidv7-generator` | Provides UUIDv7 generation and validation without business-domain responsibilities. May be a lightweight library rather than an independent Nuxt layer. |
| `caching-service` | Provides reusable cache operations, expiry and invalidation primitives. Domain policies remain with consumers. |
| `storage-service` | Provides optional object-storage operations through replaceable adapters. Does not own domain records. |
| `configuration-service` | Candidate only: shared configuration validation and resolution where existing Nuxt mechanisms are insufficient. |
| `clock-service` | Candidate utility for injectable time and deterministic tests. Prefer a small library unless broader capability needs arise. |

## 3. Platform capabilities

| Capability | Definition |
|---|---|
| `audit-service` | Owns attributable, protected and retention-governed audit evidence, distinct from operational logs. |
| `observability-service` | Owns reusable metrics, traces, health instrumentation and exporter integration. |
| `notification-service` | Owns notification delivery and channel integration; business triggers remain domain-owned. |
| `event-bus-service` | Provides optional event transport and delivery contracts; business event schemas remain with their originating domains. |
| `job-service` | Provides background execution, retry and cancellation infrastructure. |
| `scheduling-service` | Owns technical schedules, recurrence and execution dispatch. |
| `workflow-service` | Owns workflow definitions, state transitions and execution orchestration, delegating domain actions through contracts. |
| `process-service` | Candidate for independently modelled business procedures; must not duplicate workflow execution. |
| `task-service` | Owns assignable tasks, task lifecycle, responsibility and completion. Includes personal to-do requirements initially. |
| `project-service` | Owns projects, milestones and project-level coordination without duplicating task authority. |
| `reporting-service` | Owns report definitions and generation, accessing domain data through authorised contracts or projections. |
| `search-service` | Provides replaceable indexing and query infrastructure with domain-controlled access rules. |
| `publishing-service` | Owns generic publication state, revision and publication operations. |
| `image-service` | Owns image processing, metadata, variants and lifecycle; binary storage is delegated. |
| `text-editor-service` | Owns reusable rich-text editing and editor-content contracts. |
| `document-service` | Owns managed document metadata, versions and lifecycle, using replaceable storage adapters. |
| `location-service` | Owns reusable location, address and geographic reference records where cross-domain reuse justifies independent authority. |
| `tenancy-service` | Candidate for tenant provisioning and lifecycle. Tenant isolation is distinct from group membership and must be enforced at each applicable security and data boundary. |

## 4. Domain/Application capabilities

### Organisation and governance

| Capability | Definition |
|---|---|
| `organisation-service` | Owns organisation-specific records, structure and business lifecycle; references Identity group IDs for membership and access without duplicating group authority. |
| `company-service` | Conditional specialisation for legal/commercial company attributes not covered by organisations; references Identity groups without duplicating membership authority. |
| `department-service` | Owns department-specific business semantics and operational assignments; references associated Identity groups rather than duplicating their memberships or hierarchy. |
| `club-service` | Owns clubs, affiliation and club-specific governance; references Identity groups for participant membership and access. |
| `committee-service` | Owns committee mandates, appointments, terms and governance lifecycle; references Identity groups for membership context. |
| `membership-service` | Owns domain membership agreements, eligibility, categories, status and renewal. |
| `office-service` | Deferred candidate for office-specific operational rules not covered by location, assets and booking. |

### Commerce and finance

| Capability | Definition |
|---|---|
| `product-service` | Owns products, variants, catalogue attributes and product lifecycle. |
| `pricing-service` | Owns price definitions, price lists and pricing rules. |
| `inventory-service` | Owns stock balances, reservations, adjustments and movements. |
| `order-service` | Owns commercial order lifecycle and fulfilment intent. |
| `invoice-service` | Owns invoice issuance, numbering, adjustment and lifecycle. |
| `billing-service` | Owns billable accounts, charges and billing cycles. |
| `accounting-service` | Owns journals, ledgers, postings, accounting periods and reconciliation. |
| `payment-service` | Owns payment coordination, payment status and refunds through compliant providers. |
| `subscription-service` | Owns recurring commercial agreements, renewals and subscription status. |
| `shop-service` | Candidate for shop-specific configuration and merchandising; prefer application composition initially. |
| `marketplace-service` | Owns marketplace-specific seller participation, listing governance and transaction rules. |

### Operations and facilities

| Capability | Definition |
|---|---|
| `warehouse-service` | Owns warehouse facilities, zones, bins and operational capacity. |
| `logistics-service` | Owns shipments, dispatch, carrier coordination and delivery lifecycle. |
| `asset-service` | Owns individually identifiable assets, custody and asset lifecycle. |
| `workshop-service` | Owns workshop facilities, resources and workshop-specific operational rules. |
| `booking-service` | Owns reservation allocation, booking status, amendments and cancellation. |

### Education, activities and content

| Capability | Definition |
|---|---|
| `course-service` | Owns courses, curricula, prerequisites and course versions. |
| `enrolment-service` | Owns enrolment eligibility, status, withdrawal and completion relationships. |
| `training-service` | Conditional capability for training delivery, assessments, competencies and outcomes. |
| `event-service` | Owns events, programmes and event lifecycle; may reference Identity groups for participant and organiser membership. |
| `game-service` | Candidate for a clearly defined game domain, including rules, sessions and results. |
| `blog-service` | Owns blogs, posts, editorial metadata and blog-specific lifecycle. |

## 5. Management interfaces

Management interfaces consume authoritative capability contracts. They do not independently own the corresponding business data.

| Interface | Responsibility |
|---|---|
| `user-management-ui` | Administers Identity users, personal-group lifecycle visibility and profiles through authorised public contracts. |
| `group-management-ui` | Administers Identity groups, one-parent/many-child hierarchies, direct memberships and revocation through authorised public contracts; never owns group state. |
| `warehouse-management-ui` | Presents warehouse and inventory administration. |
| `workflow-management-ui` | Presents workflow definitions, execution and monitoring. |
| `workshop-management-ui` | Presents workshop administration and operations. |
| `media-library-ui` | Presents image and document management. |
| `company-console` | Composes authorised company administration interfaces and dashboards. |

These may be separately versioned presentation capabilities or components within a larger administrative capability. Separate repositories require demonstrated reuse or independent lifecycle value.

## 6. Infrastructure integrations and application compositions

| Component | Responsibility |
|---|---|
| `integration-adapters` | Catalogue of independently installable external-provider adapters, not one mandatory runtime layer. |
| `api-gateway` | Optional application-edge routing and API composition. Domain APIs remain capability-owned. |
| `application-compositions` | Deployable Nuxt applications selecting capability versions, supplying adapters and configuring routes. |

The composition root supplies database connections and secrets. Each persistence-using capability retains its own schema and migrations.

## 7. Consolidations and removals

- Remove the proposed shared `persistence-service`; prohibited by ADR-0002.
- Consolidate generic `user-service` and `group-service` ownership into Identity; implement the proposed group model there, not in a competing service.
- Keep session lifecycle within Authentication unless an explicit architectural decision authorises another boundary.
- Consolidate `todo-manager` into `task-service` initially.
- Treat `company-manager`, `office-manager`, `process-manager`, `training-manager` and `shop-manager` as conditional capabilities.
- Treat `api-services` and `integrations` as composition/integration concerns rather than monolithic business layers.
- Preserve existing published repository names and contracts unless an independently reviewed change justifies migration.

## 8. Group ownership and membership model (proposed)

- Each human identity has exactly one immutable, system-managed personal (unary) group; other group memberships are optional and many-to-many.
- Each group has **at most one parent** and **zero or more children**; roots have no parent. Cycles are forbidden. This is a forest of rooted trees, not a multiple-parent graph.
- Group hierarchy is organisational structure, **not implicit permission inheritance**. Any inherited membership or authorisation must be explicitly specified and tested.
- Resource ownership, creator provenance, membership, tenant isolation and access grants are distinct concepts. Domain capabilities own their business resources and reference public group identifiers.
- When a user leaves a group, all access derived from that membership, including any explicitly inherited or delegated access, must be revoked, with stale sessions and caches accounted for. Independently granted access is evaluated separately.
- Group membership alone does not grant unrestricted access. Authorisation is evaluated server-side against actor, action, resource and context.
- Companies, departments, clubs, committees, events and similar domains may associate their records with groups but retain ownership of their domain-specific rules and data.
- Full proposed definitions and acceptance criteria: [Group Model Definition v0.1](../identity/group-model-definition-v01.md). Adoption as normative architecture requires review and approval.

## 9. Minimum development definition

Every proposed implementation must identify:

1. Purpose and bounded responsibility.
2. Owned state, rules and lifecycle.
3. Explicit exclusions.
4. Provided public contracts and contract versions.
5. Required and optional capability contracts.
6. Configuration and persistence-provider requirements.
7. Security, privacy, accessibility and internationalisation obligations where applicable.
8. Independent contract, integration and consumer acceptance tests.

## 10. Development order

1. Preserve and verify the existing Theme Manager and Authorisation implementations.
2. Implement Logging Service as the next foundational reference capability.
3. Build missing identity/authentication/privacy capabilities according to actual application requirements.
4. Validate a complete domain-to-management-interface composition.
5. Add platform and business capabilities incrementally, driven by consumers.
6. Introduce separate infrastructure capabilities only where reusable contracts and multiple consumers justify them.

**Implementation policy:** Do not create a repository solely because a capability appears in this catalogue. Establish its bounded responsibility, public contract, dependency requirements and independent lifecycle justification first.