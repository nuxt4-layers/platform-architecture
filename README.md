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
2. the [Architecture](docs/architecture.md) overview;
3. architecture and contract standards;
4. security, privacy, accessibility and other cross-cutting normative baselines;
5. layer-specific normative documentation;
6. application-specific documentation;
7. implementation.

Implementation is evidence of current behaviour; it is not automatically architectural authority.

## Where to find things

This repository holds what applies to the whole ecosystem: principles, cross-cutting standards, Architecture Decision Records and the capability catalogue as an index. A capability's own contracts, data model, threat model, roadmap and processes live in its repository's `docs/`; a suite's architecture and cross-capability processes live in the suite's integration repository, such as `nuxt4-layers/iam-integration`. Documents in other repositories are listed, with why and where, in [Resources](docs/RESOURCES.md). This repository holds no plans or status: those belong to each capability's and application's own `docs/PLAN.md`. See [ADR-0004](docs/decisions/ADR-0004-documentation-placement.md) and [ADR-0010](docs/decisions/ADR-0010-documentation-style.md).

## Documentation map

- [Architecture](docs/architecture.md)
- [Presentation, Internationalisation and Accessibility](docs/presentation.md)
- [Identity and Access](docs/identity.md)
- [Security](docs/security.md)
- [Privacy](docs/privacy.md)
- [Layer Standard](docs/layer-standard.md)
- [Capability Catalogue](docs/catalogue.md)
- [Resources](docs/RESOURCES.md)
- [Decisions](docs/decisions/README.md)
