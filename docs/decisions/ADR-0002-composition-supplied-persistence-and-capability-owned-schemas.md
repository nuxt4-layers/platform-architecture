# ADR-0002 — Composition-Supplied Persistence and Capability-Owned Schemas

**Status:** Accepted  
**Date:** 2026-10-03

## Context

Several capabilities need durable relational storage: authentication (credentials, sessions, MFA factors), identity, authorization, theme management and future domain capabilities.

The Platform Architecture already establishes that:

- the host application is the composition root and connects infrastructure adapters (Composition and Dependency Model §1);
- PostgreSQL is the preferred relational database (Platform Architecture §7);
- infrastructure providers must be replaceable where practical (Platform Architecture invariant 10);
- consumers must not depend on another capability's internal database schema (Layer Architecture §4);
- the dependency graph must remain acyclic (Composition and Dependency Model §2).

Two approaches were considered for giving capabilities a database:

1. **A shared data/persistence capability** that every other capability depends upon for database access.
2. **Composition-supplied persistence**, where each capability declares a persistence port and the host application supplies the connection.

The first approach makes one capability a mandatory dependency of almost every other capability, concentrates unrelated data models in one place, encourages cross-capability table access and couples every release to a single persistence package. It also tends to leak one vendor's client library into every capability.

Hosted database products also offer their own SDKs, authentication services and data APIs. Using those directly inside a capability would bind its contract to one provider.

## Decision

### 1. No shared persistence capability

There is no ecosystem-wide data-access layer. A capability that needs persistence declares its own persistence port as part of its public composition contract.

### 2. The composition root supplies the connection

The host application creates and owns database connections (pool lifecycle, credentials, TLS, pooling mode) and supplies them to each capability through that capability's documented provider function, normally from a Nitro server plugin.

A capability MUST fail closed when a required persistence provider has not been supplied. It MUST NOT silently fall back to an in-memory or file-based store outside explicit test or development configuration.

### 3. Each capability owns its schema

On PostgreSQL, each capability that persists data owns a dedicated, named database schema (for example `authentication`, `identity`, `authorization`).

- The capability owns the table definitions, migrations and indexes in its schema.
- The capability ships its migrations with its package and documents how the host applies them.
- No capability reads or writes another capability's schema. Cross-capability data access uses public service, query or event contracts.
- Cross-schema foreign keys MUST NOT be introduced. A capability that refers to another capability's entity stores that entity's public identifier.
- The host MAY place several capabilities' schemas in one physical database, or split them across databases, without changing any capability contract.

Where the host can grant per-schema database roles, it SHOULD give each capability a role limited to its own schema.

### 4. Hosted providers are used as infrastructure only

A managed PostgreSQL service (for example Supabase, Neon or a cloud provider's managed PostgreSQL) is used as a PostgreSQL endpoint through a standard driver connection.

Capabilities MUST NOT depend on a provider's proprietary SDK, data API, or authentication service unless an ADR deliberately makes that provider part of a public contract. In particular, a hosted provider's built-in authentication service MUST NOT run alongside the authentication capability as a competing source of sessions or credentials.

### 5. Credentials and environments

Connection strings and database credentials are secrets supplied through runtime secret management. They MUST NOT be committed or exposed through public runtime configuration.

Automated tests and CI SHOULD use a disposable local PostgreSQL instance rather than a shared hosted database.

## Consequences

### Positive

- no mandatory persistence dependency shared by every capability;
- capability data models stay private and evolve independently;
- hosts can change database vendor, topology or pooling without capability releases;
- capability tests can run against a disposable database with no external service;
- the dependency graph stays acyclic.

### Costs

- each persistence-using capability maintains its own migrations;
- the host must apply several capabilities' migrations in a defined order;
- cross-capability reporting requires contracts or events rather than SQL joins;
- each capability documents its provider function and expected connection type.

## Guardrails

- A capability's persistence port is documented in its composition contract, including the expected connection type and the migration procedure.
- Migrations are additive within a release line; destructive schema changes follow the Compatibility and Versioning Standard.
- Each capability's security tests include a negative test showing it fails closed when its persistence provider is absent.
