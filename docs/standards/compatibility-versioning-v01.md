# Compatibility and Versioning Standard v0.1

**Status:** Normative

## 1. Objective

Applications must use known-compatible capability versions rather than silently following another repository's changing default branch.

## 2. Semantic versioning

Independently released layers SHOULD use semantic versioning.

A breaking public-contract or documented semantic change requires a major version increment once the package has reached 1.0.0. Pre-1.0 evolution MUST still document breaking changes explicitly.

## 3. Contract versions

Capability contract versions MAY evolve independently from package versions. A package manifest MUST identify the contract versions it provides and requires where machine-readable manifests are used.

## 4. Reproducibility

Production applications MUST pin dependencies using normal package-manager lockfiles and version constraints. They MUST NOT rely on a changing default branch as their production compatibility mechanism.

## 5. Compatibility testing

A layer SHOULD test against supported dependency ranges. The composition application MUST test the exact integrated dependency set before deployment.

## 6. Deprecation

Where practical, breaking removals SHOULD be preceded by documented deprecation and migration guidance.

## 7. Provider independence

Changing a private infrastructure provider without altering the public contract does not inherently require a major version. Provider-specific observable behaviour that is part of the documented contract does.

## 8. Layer dependency lifecycle

The preferred dependency lifecycle is:

1. **Local application layer** — use `layers/` when the capability belongs to one application and has no independent release lifecycle.
2. **Local workspace development** — a local path or package-manager workspace/link MAY be used while developing related repositories together.
3. **Git-backed package dependency** — during early independent development, the application MAY declare the layer's Git repository as a package dependency and extend the installed package by name.
4. **Versioned package dependency** — stable independently released layers SHOULD normally be consumed as versioned packages.
5. **Direct remote `extends`** — supported by Nuxt but exceptional in this ecosystem.

For a Git-backed package dependency, a full commit SHA gives the strongest immutable reference. Tags or semantic-version Git references MAY be used where their mutability and release process are understood.

The lockfile is part of the reproducibility record and MUST be committed for composition applications.

Dependency updates MUST be deliberate changes. Updating a layer version or Git revision MUST pass the consuming application's integration checks before deployment.
