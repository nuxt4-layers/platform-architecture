# Layer Consumption Workflow v0.1

**Status:** Normative workflow

## 1. Purpose

This workflow defines how a consuming Nuxt application adds, updates, tests and releases an independently maintained Nuxt Layer.

The goal is to keep layer source authoritative in one place, make application builds reproducible and keep dependency updates explicit.

## 2. Choose the consumption method

Before adding a layer, classify it.

- If the capability belongs only to the application and has no independent lifecycle, use a local `layers/` layer.
- If related capabilities are being developed together locally, a workspace or local path MAY be used.
- If the layer has its own repository but is not yet a stable published package, install it as a Git-backed package dependency.
- If the layer has a stable release lifecycle, install the versioned package.
- Use direct remote Nuxt `extends` only when there is a specific reason to accept its weaker package-manager reproducibility.

Do not copy independently maintained layer source into the application as a normal integration method.

## 3. Add an independently maintained layer

For a Git-backed layer:

1. select an intentional Git revision, preferably an immutable commit SHA when exact reproducibility is required;
2. declare the Git repository in the application's package dependencies;
3. install it with the project's package manager;
4. commit the resulting lockfile;
5. reference the installed package name from Nuxt `extends`;
6. record any required capability contracts and configuration;
7. run the application's required type, test, security and build checks.

For a published layer, follow the same process using the selected package version instead of a Git repository reference.

## 4. Update a layer

A layer update is an explicit application change.

1. review the layer's release notes, contract changes and compatibility requirements;
2. update the package version or Git revision;
3. regenerate and review the lockfile;
4. run the layer's required checks where applicable;
5. run the consuming application's integration, security and build checks;
6. merge through the normal pull-request workflow.

Do not make production applications silently follow a repository's default branch.

## 5. Develop multiple capabilities together

During active development, package-manager workspace or local linking MAY replace the normal remote dependency temporarily.

Before release or deployment, restore the intended versioned or Git-backed dependency, regenerate the lockfile and test the exact dependency set that will be deployed.

Local development convenience MUST NOT make the production dependency graph ambiguous.

## 6. Promote a Git-backed layer to a released package

When a layer has a stable independent release lifecycle:

1. make the repository package-ready;
2. define its package version and compatibility policy;
3. publish it to the approved public or private package registry;
4. replace consuming Git dependencies with the released package version;
5. update and commit the lockfile;
6. run composition tests;
7. remove obsolete Git-specific installation configuration.

The Git repository remains the authoritative source repository. The published package is a versioned distribution of that source.

## 7. Direct remote extends

Nuxt can load Git repositories directly from `extends`, but this ecosystem treats that method as exceptional.

If it is used:

- state why package-manager installation is unsuitable;
- pin an intentional revision;
- account for the remote layer's dependency-installation behaviour;
- test the resolved layer in CI;
- do not use an unpinned mutable default branch for production.

## 8. Git submodules

Existing Git submodules MAY be preserved while legacy applications are reconciled, but new independently maintained Nuxt Layers SHOULD NOT use submodules as the default integration mechanism.

Migration away from a submodule MUST preserve history and authoritative source until the replacement dependency path has been verified.

## 9. Pull-request evidence

A pull request that adds or updates an independently maintained layer SHOULD show:

- selected package version or Git revision;
- lockfile change;
- compatibility impact;
- configuration changes;
- relevant contract changes;
- successful required checks;
- any known security, privacy or migration impact.

This evidence makes layer upgrades reviewable and repeatable.
