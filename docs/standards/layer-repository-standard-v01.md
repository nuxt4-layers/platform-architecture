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

## 7. Distribution and consumption

An independently maintained layer repository SHOULD be package-ready even before it is published to a package registry.

Its `package.json` SHOULD provide the metadata and dependency declarations needed for package-manager installation. Dependencies imported by the layer MUST be declared by the layer rather than relying on undeclared dependencies from the consuming application.

During early development, consumers MAY install the repository directly as a Git-backed package dependency. Stable releases SHOULD normally be published and consumed as versioned packages, using the `@nuxt4-layers/` scope when appropriate.

Direct remote Nuxt `extends` remains a supported Nuxt mechanism but is not the preferred ecosystem distribution method.

Repository boundaries and package boundaries SHOULD normally align for independently released capabilities, while remaining architectural implementation choices rather than definitions of the capability boundary.
