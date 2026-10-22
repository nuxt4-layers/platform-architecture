# Layer Repository Standard v0.1

**Status:** Normative

## 1. Repository eligibility

Create a separate layer repository only when it represents a cohesive capability and separate versioning, reuse, ownership, testing, release or isolation provides clear value.

Do not create repositories for individual buttons, composables, entities, endpoints or similarly microscopic units.

## 2. Recommended structure

```text
/
├── app/
├── server/
├── shared/
├── tests/
├── docs/
├── nuxt.config.ts
├── package.json
└── README.md
```

Only directories relevant to the capability need exist.

## 3. Required documentation

Each independently versioned layer MUST document:

- purpose and bounded responsibility;
- public contract;
- provided and required capabilities;
- configuration;
- installation/composition;
- compatibility;
- testing;
- security/privacy considerations where applicable, with a threat model and control register for security-critical capabilities;
- for a capability, its *Headless surface*: what screens drawing it must do ([ADR-0009](../decisions/ADR-0009-headless-capability-layers.md));
- the ADRs that govern it, in its README ([ADR-0004](../decisions/ADR-0004-documentation-placement.md));
- its releases, in `CHANGELOG.md` ([ADR-0008](../decisions/ADR-0008-independent-layer-delivery.md)).

It MUST also ship a `capability.json` [manifest](../contracts/capability-manifest-v01.md).

## 4. Package naming

Published packages SHOULD use the `@nuxt4-layers/` scope when available and appropriate. Repository names SHOULD be concise capability names.

## 5. Branching and change control

Protected default branches are recommended. Material changes SHOULD be developed on a branch and integrated through pull request review.

Architecture-changing work MUST reference the governing architecture or an ADR where relevant.

## 6. Private implementation

Whether a repository is public or private does not define its API. Undocumented or internal implementation remains outside the supported cross-layer contract, even in an open-source repository.

## 7. Distribution and consumption

An independently maintained layer SHOULD be package-ready before it is published, and MUST declare every dependency it imports. Consumption follows the [Layer Consumption Workflow](layer-consumption-workflow-v01.md). Repository and package boundaries SHOULD align.
