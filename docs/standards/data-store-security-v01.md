# Data Store Security Standard v0.1

**Status:** Normative, under [ADR-0006](../decisions/ADR-0006-polyglot-persistence-and-data-store-security.md)

This standard applies to every capability that stores data, in any kind of store. It refines [Security Architecture v0.1](../security/security-architecture-v01.md) §6 and §8 and [Privacy Architecture v0.1](../privacy/privacy-architecture-v01.md) §3 and §6. Requirements use MUST, SHOULD and MAY as in the other normative documents.

## 1. Inventory

Each capability MUST document, in its composition contract, every store it uses:

| Field | Meaning |
|---|---|
| Kind | Relational, graph, document, search, object, cache, time-series or other |
| Port | The provider function and the client type the host supplies |
| Holds | The data it holds, and whether it is the system of record or a derived copy |
| Classification | Whether it holds personal data, secrets or credentials (normally none outside Profile and Authentication) |
| Isolation | How tenant and group isolation is enforced (§3) |
| Erasure | How erasure and anonymisation reach it (§6) |
| Rebuild | For a derived copy, how it is rebuilt from the system of record |

## 2. Connections and credentials

1. The host supplies every client. A capability MUST NOT create its own connection from environment variables or embed credentials.
2. Each capability SHOULD receive credentials limited to its own schema, keyspace, index, bucket or prefix. Where the store supports it, the credential MUST NOT be able to read another capability's data.
3. Connections MUST use TLS outside a single trusted host, with certificate verification enabled.
4. Credentials MUST be rotatable without a capability release.
5. Administrative credentials (migrations, index creation) SHOULD be separate from runtime credentials and unavailable to request handling.

## 3. Authorisation and isolation

1. No store is reachable from the browser. Pre-signed object URLs are the only exception: they MUST be issued after server-side authorisation, scoped to one object and one operation, and expire within 15 minutes.
2. Every record, document, node, index entry and object key MUST carry its tenant identifier and owning group identifier.
3. Every query MUST be filtered by the tenant resolved on the server for the request. The filter is applied in the capability's data-access code, which callers cannot bypass.
4. Where the store cannot evaluate the platform's policy, the query MUST be narrowed by the caller's visible scopes from Authorisation's scopes query, and results MUST be checked per item for permissions whose risk level is `high` or `critical`.
5. On PostgreSQL, tables with tenant- or group-isolated rows SHOULD use row-level security:
   - policies read the tenant and scopes from transaction-local settings set with `SET LOCAL` (or `set_config(..., true)`), never session-level settings that a pooled connection could carry to another request;
   - runtime roles MUST NOT own the tables and MUST NOT have `BYPASSRLS`;
   - row-level security is containment only and never replaces application authorisation.
6. Graph traversals MUST be bounded in depth and MUST NOT cross a tenant boundary. A relationship between nodes of different tenants is refused on write.
7. Search and vector indexes MUST apply the tenant and scope filter inside the query, not by post-filtering a top-N result, so that results neither leak nor silently drop authorised items.
8. Caches MUST key entries by tenant and by every input that affects the result, including the caller's scopes where results differ by caller. Authorisation decisions MUST NOT be cached beyond the consistency bound Authorisation's contract allows.
9. Isolation tests MUST show, for each store, that one tenant cannot read or infer another tenant's data.

## 4. Encryption and secrets

1. Stores SHOULD be encrypted at rest by the provider or the host.
2. Secrets and credentials MUST be stored only by the capability that owns them, hashed or encrypted as its threat model requires, and never copied to derived stores.
3. Application-managed keys follow Security Architecture §8: owner, rotation, revocation and recovery stated.

## 5. Integrity and events

1. Writes that other capabilities must learn about MUST be published through a transactional outbox in the system of record, so that the event exists if and only if the write committed.
2. Derived copies MUST be idempotent under redelivery and tolerate out-of-order events, using a version or timestamp from the system of record.
3. Events and logs carry opaque identifiers and a correlation identifier only, never personal data, secrets or query text containing them.

## 6. Privacy lifecycle

1. Erasure and anonymisation MUST reach every store, including derived copies, caches and search indexes, within the capability's documented period. Backups age out on a documented schedule.
2. Anonymisation of a person is by unlinking ([ADR-0005](../decisions/ADR-0005-iam-suite.md)): stores outside Profile hold only opaque identifiers, so removing Profile's link is sufficient unless a capability documents personal data of its own.
3. Retention periods MUST be documented per store and enforced by the capability.
4. A tenant's data region MUST be honoured by every store holding that tenant's data, including derived copies and backups. A capability that cannot honour a region refuses to store that tenant's data rather than storing it elsewhere.

## 7. Verification

Each capability's threat model and control register MUST reference this standard and record, per store, the evidence for §2 to §6. Tests use disposable local instances of each store, as ADR-0002 requires for PostgreSQL.
