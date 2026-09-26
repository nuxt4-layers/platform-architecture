# Nuxt 4 Layers Platform Architecture

This repository contains the normative architecture, engineering principles, contracts, standards, and architectural decisions governing the `nuxt4-layers` ecosystem.

## Purpose

The ecosystem is based on independently evolvable **bounded capabilities** implemented as Nuxt Layers and composed by thin Nuxt application shells. Repository boundaries support, but do not themselves define, architectural boundaries.

The governing principles are:

- capability-oriented decomposition rather than component-oriented fragmentation;
- contract-first interaction between layers;
- encapsulation of implementation details;
- explicit dependency direction with no circular layer dependencies;
- composition at application boundaries rather than deep inheritance chains;
- independently testable and versionable capabilities;
- infrastructure replaceability behind domain-owned ports;
- server-side security enforcement and defence in depth;
- privacy and data protection by design.

## Documentation authority

Where documents conflict, authority descends in this order:

1. accepted Architecture Decision Records for the decision they explicitly settle;
2. Platform Architecture;
3. architecture and contract standards;
4. security and privacy baselines;
5. layer-specific normative documentation;
6. application-specific documentation;
7. implementation.

Implementation is evidence of current behaviour; it is not automatically architectural authority.

## Documentation map

- [Platform Architecture v0.1](docs/architecture/platform-architecture-v01.md)
- [Layer Architecture v0.1](docs/architecture/layer-architecture-v01.md)
- [Composition and Dependency Model v0.1](docs/architecture/composition-dependency-model-v01.md)
- [Layer Interface Standard v0.1](docs/contracts/layer-interface-standard-v01.md)
- [Capability Manifest v0.1](docs/contracts/capability-manifest-v01.md)
- [Identity and Authorization Architecture v0.1](docs/identity/identity-authorization-architecture-v01.md)
- [Security Architecture v0.1](docs/security/security-architecture-v01.md)
- [Privacy Architecture v0.1](docs/privacy/privacy-architecture-v01.md)
- [Layer Repository Standard v0.1](docs/standards/layer-repository-standard-v01.md)
- [Compatibility and Versioning Standard v0.1](docs/standards/compatibility-versioning-v01.md)
- [ADR-0001 — Capability Repositories and Thin Composition Shells](docs/decisions/ADR-0001-capability-repositories-and-composition-shells.md)

## Status

Version 0.1 establishes the initial architectural baseline. It is intentionally technology-aware but avoids binding domain contracts to replaceable infrastructure providers.
