# Compatibility and Versioning Standard v0.1

**Status:** Normative

## 1. Objective

Composition must select known-compatible capability versions rather than implicitly tracking another repository's default branch.

## 2. Semantic versioning

Independently released layers SHOULD use semantic versioning.

A breaking public-contract or documented semantic change requires a major version increment once the package has reached 1.0.0. Pre-1.0 evolution MUST still document breaking changes explicitly.

## 3. Contract versions

Capability contract versions MAY evolve independently from package versions. A package manifest MUST identify the contract versions it provides and requires where machine-readable manifests are used.

## 4. Reproducibility

Production composition MUST pin dependencies through normal package-manager lockfiles and version constraints. Direct consumption of mutable default branches is unsuitable as the production compatibility mechanism.

## 5. Compatibility testing

A layer SHOULD test against supported dependency ranges. The composition application MUST test the exact integrated dependency set before deployment.

## 6. Deprecation

Where practical, breaking removals SHOULD be preceded by documented deprecation and migration guidance.

## 7. Provider independence

Changing a private infrastructure provider without altering the public contract does not inherently require a major version. Provider-specific observable behaviour that is part of the documented contract does.
