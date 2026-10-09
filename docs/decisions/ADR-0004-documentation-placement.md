# ADR-0004 — Documentation Placement

**Status:** Accepted  
**Date:** 2026-10-09

## Context

This repository holds the ecosystem's normative architecture. As capabilities are designed, there is pressure to place their detailed specifications here too: data models, contracts, process steps and threat models. Doing so would mix ecosystem-wide rules with capability-specific detail, duplicate what capability repositories already document, and let the two drift apart.

Capability repositories already maintain their own contract, composition, threat-model and roadmap documents (Layer Repository Standard §3). Several capabilities also form suites whose processes span more than one of them, such as the Identity and Access Management (IAM) suite.

## Decision

A document lives with whoever is responsible for keeping it true.

| Location | Holds |
|---|---|
| `nuxt4-layers/platform-architecture` | Ecosystem-wide principles and invariants; cross-cutting standards; Architecture Decision Records; a glossary; the capability catalogue as an index of one line and one link per capability |
| Each capability repository, under `docs/` | Its public contract, composition contract, data model, threat model and control register, roadmap, and the processes it owns |
| A suite's integration repository, e.g. `nuxt4-layers/iam-integration`, under `docs/` | The suite's architecture and the processes that span several of its capabilities |

Rules:

1. **ADRs stay short.** An ADR records context, the decision, its consequences and links. It names the detailed document that carries the decision out; that document is authoritative because the ADR names it.
2. **Links to another repository's documents are pinned** to a tag or commit, so a decision refers to exactly the version that was reviewed. A material change to the detail requires a new or amended ADR that moves the pin.
3. **Each capability's README lists the ADRs that govern it**, so links run in both directions.
4. **Ecosystem-wide rules are not restated** in capability repositories; they link here.
5. **Detail that has moved leaves a pointer** at its former location for one release of this repository, then the pointer is removed.

The documentation authority order in the README is unchanged: an ADR outranks the detailed document it names.

## Consequences

### Positive

- This repository stays small enough to read in full.
- Capability detail is maintained beside the code that implements it.
- Pinned links make every decision reproducible.

### Costs

- Readers follow links across repositories, some of them private.
- Changing pinned detail requires an ADR update.

## Guardrails

- The Markdown check rejects links to another `nuxt4-layers` repository's documents that point at a moving branch.
- The [Group Model Definition v0.1](../identity/group-model-definition-v01.md) moves to the Identity repository once it exists, leaving a pointer here.
- The capability catalogue is an index; definitions longer than one line belong in the capability's own README.
