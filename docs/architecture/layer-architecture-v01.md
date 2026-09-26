# Layer Architecture v0.1

**Status:** Normative

## 1. Definition

A Nuxt Layer in this ecosystem is an implementation vehicle for a bounded capability. A repository boundary MAY coincide with a layer boundary when independent ownership, versioning, reuse, testing, release, or security justify it.

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

Repository proliferation is an architectural cost and requires a capability-level justification.

## 6. Testing

Each layer SHOULD have:

- contract tests for its public surface;
- unit tests for domain logic;
- integration tests for infrastructure adapters where applicable;
- compatibility tests for required capabilities;
- security tests appropriate to its threat surface.

A consuming shell MUST additionally test the composed system.
