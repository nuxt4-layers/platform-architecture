# Plan

> **In short:** what is planned for the ecosystem's architecture and capabilities, and where each stands. This is the only place plans and status are written ([ADR-0010](decisions/ADR-0010-documentation-style.md)). Each capability's own plan is in its repository's `docs/PLAN.md`; each application's in its own.

## Capabilities

| Capability | Status |
|---|---|
| `theme-manager`, `configuration`, the IAM suite (`identity`, `authentication`, `authorisation`, `profile`, `iam-integration`), `rich-text`, `publisher` | Released |
| `media`, `content-editor`, `gallery` | Designed; media is built first, then content-editor on it, then the gallery |
| `ui` | Not released. Its first release offers only Theme Manager's vocabulary (removing its own tokens and theme handling), common primitives, a safe-tree renderer, a person-name component fed by the host, keyed wording, an accessibility test kit and response-header settings |
| Logging and audit | Next foundation after the above: an audit capability collecting every layer's events, with reports |

Candidates (definitions in Resources: *Catalogue v0.4*), built only when an application needs one and its bounded responsibility is established:

- **Foundation:** `privacy`, `logging-service`, `uuidv7-generator`, `caching-service`, `clock-service`.
- **Platform:** `observability-service`, `notification-service`, `event-bus-service`, `job-service`, `scheduling-service`, `workflow-service`, `process-service`, `task-service`, `project-service`, `reporting-service`, `search-service`, `document-service`, `location-service`, `tenancy-service`.
- **Domain:** `organisation-service`, `company-service`, `department-service`, `club-service`, `committee-service`, `membership-service`, `office-service`; `product-service`, `pricing-service`, `inventory-service`, `order-service`, `invoice-service`, `billing-service`, `accounting-service`, `payment-service`, `subscription-service`, `shop-service`, `marketplace-service`; `warehouse-service`, `logistics-service`, `asset-service`, `workshop-service`, `booking-service`; `course-service`, `enrolment-service`, `training-service`, `event-service`, `game-service`.
- **Presentation and composition:** `user-management-ui`, `warehouse-management-ui`, `workflow-management-ui`, `workshop-management-ui`, `company-console`, `integration-adapters`, `integrations`, `api-gateway`.

## This repository

Consolidating its documents, one chat each:

| Step | Status |
|---|---|
| 1–2. Adopt ADR-0010's files; remove what was replaced (the headless migration plan; the catalogue reduced to existing capabilities) | Done |
| 3. One layer standard from the layer architecture, layer interface standard, capability manifest, layer repository standard and consumption workflow | Done |
| 4. One architecture overview from the platform architecture and the composition and dependency model | In review |
| 5. Identity documents merged; data-store security merged into the security architecture | Next |
| 6. Version suffixes dropped from file names; the decision index moved to `docs/decisions/README.md` | Planned |
