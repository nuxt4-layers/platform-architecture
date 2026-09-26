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
- security/privacy considerations where applicable.

## 4. Package naming

Published packages SHOULD use the `@nuxt4-layers/` scope when available and appropriate. Repository names SHOULD be concise capability names.

## 5. Branching and change control

Protected default branches are recommended. Material changes SHOULD be developed on a branch and integrated through pull request review.

Architecture-changing work MUST reference the governing architecture or an ADR where relevant.

## 6. Private implementation

Whether a repository is public or private does not define its API. Undocumented or internal implementation remains outside the supported cross-layer contract, even in an open-source repository.
