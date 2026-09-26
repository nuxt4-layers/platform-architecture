# Composition and Dependency Model v0.1

**Status:** Normative

## 1. Composition root

The host Nuxt application is the main composition root. It chooses layer versions, supplies configuration, connects infrastructure adapters, defines application-specific routes and connects capabilities where the application needs them to work together.

## 2. Dependency graph

Capability dependencies MUST form a directed acyclic graph (DAG).

Circular dependencies are prohibited. If A requires B, B MUST NOT require A directly or transitively.

If two capabilities appear to depend on each other, move their shared contract to an appropriate boundary, communicate through an event or port, or reconsider where the capability boundaries belong.

## 3. Contracts over internals

A dependency targets a capability's public contract, not the way that capability is implemented or deployed.

A domain capability requiring authorization asks for an AuthorizationService contract; authorization does not gain knowledge of that domain's internals. Domain resources are represented through agreed generic or domain-owned contract types.

## 4. Composition over inheritance

Nuxt `extends` is an implementation mechanism, not the architectural dependency model.

Avoid chains in which application layers extend platform layers that extend identity layers that extend authentication layers. Prefer the composition root to combine peer capabilities and use explicit adapters to connect their contracts.

## 5. Dependency declaration

A layer MUST document required capabilities and compatible contract versions. Optional dependencies MUST be identified as optional and behaviour without them documented.

## 6. Failure isolation

A capability SHOULD define expected failure modes at its boundary. Internal exceptions MUST NOT leak as undocumented cross-layer contracts.

## 7. Application-specific integration

Where two capabilities need application-specific coordination, prefer an adapter in the composition application rather than modifying either capability to know about the other unnecessarily.
