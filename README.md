# Nuxt 4 Layers Platform Architecture

> **AI-Driven Development**
>
> This repository is part of Nuxt 4 Layers, an experimental, AI-driven software engineering initiative.
>
> AI performs the principal architecture, development, testing, security assessment and documentation activities under human direction. The project owner retains authority over requirements, governance, acceptance and releases.
>
> **Our objective is to demonstrate that disciplined, specification-led AI development can deliver secure, maintainable, standards-compliant, production-quality open-source software.**
>
> All contributions are subject to the same engineering standards, quality controls and repository policies, regardless of origin. See [Contributing](CONTRIBUTING.md) for the methodology and governance model.

This repository contains the normative architecture, engineering principles, contracts, standards, and architectural decisions governing the `nuxt4-layers` ecosystem.

## Purpose

The ecosystem consists of bounded capabilities, implemented as Nuxt Layers, that can evolve independently and are combined into lightweight Nuxt applications. Repository structure supports these architectural boundaries but does not define them.

The main principles are:

- organise the system around bounded capabilities;
- define clear public contracts between layers;
- keep private implementation details inside each layer;
- keep dependencies one-way and free of cycles;
- combine capabilities rather than building deep inheritance chains;
- keep infrastructure replaceable where practical;
- enforce security on the server;
- design for privacy from the start;
- meet WCAG 2.2 Level AA as the accessibility baseline;
- design for internationalisation from the start;
- use meaningful semantic markup; and
- allow users to customise presentation without breaking accessibility.

## Documentation authority

Where documents conflict, authority descends in this order:

1. accepted Architecture Decision Records for the decision they explicitly settle;
2. Platform Architecture;
3. architecture and contract standards;
4. security, privacy, accessibility and other cross-cutting normative baselines;
5. layer-specific normative documentation;
6. application-specific documentation;
7. implementation.

Implementation is evidence of current behaviour; it is not automatically architectural authority.

## Where to find things

This repository holds what applies to the whole ecosystem: principles, cross-cutting standards, Architecture Decision Records and the capability catalogue as an index. A capability's own contracts, data model, threat model, roadmap and processes live in its repository's `docs/`; a suite's architecture and cross-capability processes live in the suite's integration repository, such as `nuxt4-layers/iam-integration`. Documents in other repositories are listed, with why and where, in [Resources](docs/RESOURCES.md); plans and status are only in the [Plan](docs/PLAN.md). See [ADR-0004](docs/decisions/ADR-0004-documentation-placement.md) and [ADR-0010](docs/decisions/ADR-0010-documentation-style.md).

## Documentation map

- [Platform Architecture](docs/architecture/platform-architecture-v01.md)
- [Layer Architecture](docs/architecture/layer-architecture-v01.md)
- [Composition and Dependency Model](docs/architecture/composition-dependency-model-v01.md)
- [Presentation, Internationalisation and Accessibility Architecture](docs/architecture/presentation-internationalisation-accessibility-v01.md)
- [Layer Interface Standard](docs/contracts/layer-interface-standard-v01.md)
- [Capability Manifest](docs/contracts/capability-manifest-v01.md)
- [Identity and Authorisation Architecture](docs/identity/identity-authorization-architecture-v01.md)
- [Group Model Definition](docs/identity/group-model-definition-v01.md)
- [Security Architecture](docs/security/security-architecture-v01.md)
- [Privacy Architecture](docs/privacy/privacy-architecture-v01.md)
- [Layer Repository Standard](docs/standards/layer-repository-standard-v01.md)
- [Compatibility and Versioning Standard](docs/standards/compatibility-versioning-v01.md)
- [Layer Consumption Workflow](docs/standards/layer-consumption-workflow-v01.md)
- [Data Store Security Standard](docs/standards/data-store-security-v01.md)
- [Capability Catalogue](docs/catalogue/capability-catalogue.md)
- [Plan](docs/PLAN.md)
- [Resources](docs/RESOURCES.md)
- [ADR-0001 — Capability Repositories and Thin Composition Shells](docs/decisions/ADR-0001-capability-repositories-and-composition-shells.md)
- [ADR-0002 — Composition-Supplied Persistence and Capability-Owned Schemas](docs/decisions/ADR-0002-composition-supplied-persistence-and-capability-owned-schemas.md)
- [ADR-0003 — Group Model and Identity Before Logging](docs/decisions/ADR-0003-group-model-and-identity-first.md)
- [ADR-0004 — Documentation Placement](docs/decisions/ADR-0004-documentation-placement.md)
- [ADR-0005 — Identity and Access Management Suite](docs/decisions/ADR-0005-iam-suite.md)
- [ADR-0006 — Polyglot Persistence and Data-Store Security](docs/decisions/ADR-0006-polyglot-persistence-and-data-store-security.md)
- [ADR-0007 — Break-Glass Emergency Access](docs/decisions/ADR-0007-break-glass-emergency-access.md)
- [ADR-0008 — Independent Layer Delivery](docs/decisions/ADR-0008-independent-layer-delivery.md)
- [ADR-0009 — Headless Capability Layers](docs/decisions/ADR-0009-headless-capability-layers.md)
- [ADR-0010 — Documentation Style](docs/decisions/ADR-0010-documentation-style.md)
- [ADR-0011 — Shared Rich Text and Configurable Copy](docs/decisions/ADR-0011-shared-rich-text-and-configurable-copy.md)
- [ADR-0012 — Trivial Composition](docs/decisions/ADR-0012-trivial-composition.md)
