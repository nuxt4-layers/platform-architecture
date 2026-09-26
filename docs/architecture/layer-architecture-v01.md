# Layer Architecture v0.1

**Status:** Normative

## 1. Definition

In this ecosystem, a Nuxt Layer implements a bounded capability. A capability MAY have its own repository when separate ownership, versioning, reuse, testing, release or security provides enough value to justify it.

A small component, composable, entity, or endpoint is not by itself sufficient justification for a repository.

## 2. Layer classifications

### Foundation
Reusable cross-cutting capabilities such as UI, authentication, identity, authorization and privacy.

### Platform
Reusable platform functions such as dashboard/workspace composition, settings, notifications and publishing.

### Domain/Application
Coherent end-user or business capabilities such as Architecture Registry or a game domain.

## 3. Public surface

A layer MUST distinguish its public surface from private implementation. Public surfaces MAY include:

- TypeScript contracts and types;
- Nuxt configuration intended for consumers;
- components explicitly designated public;
- composables explicitly designated public;
- server handlers or service interfaces intentionally exposed;
- events and configuration schemas.

Everything not explicitly public SHOULD be treated as private.

## 4. Encapsulation

Consumers MUST NOT depend on internal database schemas, private composables, internal server utilities, provider SDK objects, or undocumented filesystem paths.

A layer MAY change private implementation without a breaking release provided its documented public contract and observable guarantees remain compatible.

## 5. Granularity

Prefer cohesive capability ownership over microscopic packages. UI primitives belong within a UI capability; login/logout mechanics belong within authentication; users/groups/memberships belong within identity.

Every additional repository adds maintenance and dependency-management cost, so a new repository needs a clear capability-level reason.

## 6. Testing

Each layer SHOULD have:

- contract tests for its public surface;
- unit tests for domain logic;
- integration tests for infrastructure adapters where applicable;
- compatibility tests for required capabilities;
- security tests appropriate to its threat surface.

A consuming shell MUST additionally test the composed system.

## 7. How applications consume layers

The way a layer is consumed depends on who owns it and where it is in its lifecycle.

### Application-owned local layers

Use the application's `layers/` directory for capabilities that belong to that application and do not need independent versioning or release.

A local path in `extends` MAY be used when a local layer sits outside the application directory, including during workspace development.

### Independently maintained layers

An independently maintained layer SHOULD normally be installed through the application's package manager and referenced by package name from Nuxt `extends`.

During early development, the package dependency MAY point directly to the layer's Git repository. The package manager lockfile MUST capture the resolved revision used by the application.

When a capability becomes stable and independently released, it SHOULD normally be consumed as a versioned package.

### Direct remote Nuxt extends

Nuxt supports loading a remote Git repository directly from `extends`. This method SHOULD be exceptional for this ecosystem because resolution happens outside the normal package-manager lockfile workflow and is less reproducible.

A direct remote layer MUST be pinned to an intentional revision or release reference when reproducibility matters. A mutable default branch MUST NOT be used as the production compatibility mechanism.

### Git submodules and copied source

Git submodules are not the standard composition mechanism for independently maintained Nuxt Layers in this ecosystem.

Copies of an independently maintained layer MUST NOT be stored in a consuming application merely to compose that layer. If a temporary copy is unavoidable, it MUST be recorded as migration or recovery work and MUST NOT become a second authoritative source.
