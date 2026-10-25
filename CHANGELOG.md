# Changelog

Major changes, newest first. This repository has no releases; each entry is dated.

## 2026-10-25

- `docs/PLAN.md` removed: this repository holds no plans or status (ADR-0010 amended); candidate capabilities moved to the catalogue.

- Documents moved to `docs/` without version suffixes or single-file folders; the decision index moved to `docs/decisions/README.md`.

- `docs/identity/identity.md` replaces the identity and authorisation architecture and the group model definition; `docs/security/security.md` replaces the security architecture and absorbs the data store security standard.

- One architecture overview (`docs/architecture/architecture.md`) replaces the platform architecture and the composition and dependency model.

- One layer standard (`docs/standards/layer-standard.md`) replaces the layer architecture, layer interface standard, capability manifest, layer repository standard, layer consumption workflow and compatibility and versioning standard.

- Documents consolidated: `docs/RESOURCES.md`, `docs/PLAN.md`, `CONTRIBUTING.md` (replacing `AI_DEVELOPMENT.md`) and this changelog added; the headless migration plan removed; the capability catalogue reduced to existing capabilities.
- ADR-0010 amended: documents that stay true without upkeep.
- ADR-0012 accepted, with `CONTRACT.md` limited to 200 lines.
- ADR-0009 amended: a headless core with an optional presentation part.
