# Nuxt 4 Layers Platform Architecture

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

## Documentation map

- [Platform Architecture v0.1](docs/architecture/platform-architecture-v01.md)
- [Layer Architecture v0.1](docs/architecture/layer-architecture-v01.md)
- [Composition and Dependency Model v0.1](docs/architecture/composition-dependency-model-v01.md)
- [Presentation, Internationalisation and Accessibility Architecture v0.1](docs/architecture/presentation-internationalisation-accessibility-v01.md)
- [Layer Interface Standard v0.1](docs/contracts/layer-interface-standard-v01.md)
- [Capability Manifest v0.1](docs/contracts/capability-manifest-v01.md)
- [Identity and Authorization Architecture v0.1](docs/identity/identity-authorization-architecture-v01.md)
- [Security Architecture v0.1](docs/security/security-architecture-v01.md)
- [Privacy Architecture v0.1](docs/privacy/privacy-architecture-v01.md)
- [Layer Repository Standard v0.1](docs/standards/layer-repository-standard-v01.md)
- [Compatibility and Versioning Standard v0.1](docs/standards/compatibility-versioning-v01.md)
- [Layer Consumption Workflow v0.1](docs/standards/layer-consumption-workflow-v01.md)
- [ADR-0001 — Capability Repositories and Thin Composition Shells](docs/decisions/ADR-0001-capability-repositories-and-composition-shells.md)

## Status

Version 0.1 establishes the initial architectural baseline. It is intentionally technology-aware but avoids binding domain contracts to replaceable infrastructure providers.
