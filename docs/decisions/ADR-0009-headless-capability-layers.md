# ADR-0009 — Headless Capability Layers

**Status:** Proposed  
**Date:** 2026-10-21

## Context

Our foundation and platform layers have each grown their own screens: pages, components, routes, wording, style maps and accessibility tests. Configuration, Profile and the IAM members all carry a version of the same presentation kit, and a host composing several of them inherits several opinions about how things should look and where they should live.

That couples data to display. A layer's real value is what it knows and guarantees, such as a document's lifecycle or a person's consent. How that is shown belongs to whoever composes it.

## Decision

**Capability layers are headless by default.** A foundation or platform layer offers services. It does not offer screens.

Layers whose capability *is* presentation, such as Theme Manager's design system, are presentation layers and are not affected.

### What a headless layer provides

| Provides | For example |
|---|---|
| Contracts | Types, schemas and pure helpers, as its contract export |
| Server functions | Each a specific task on its data: create, publish, export |
| An HTTP API | Strict, coarse and subject-checked, which the host may switch off |
| Events and ports | Its outbox, and the ports it asks the host to fill |
| Data formats | Machine output such as feeds, sitemap entries and export bundles |
| A typed client | Optional and non-visual: data and actions only |

### What it never provides

Pages, routes, components, styles or class maps, display wording, layout, or slots for other layers' components. These belong to presentation.

### Where presentation lives

- **Presentation layers** draw one or more capabilities' services in the platform's design system, such as an administration console or a public site kit. They depend on the capabilities' contracts; a capability never depends on them.
- **Applications** compose capabilities and presentation, and own their routes, branding and wording.
- **Editing surfaces**, such as `content-editor`, are presentation layers too.

The [presentation, internationalisation and accessibility baseline](../architecture/presentation-internationalisation-accessibility-v01.md) applies wherever presentation lives. Accessibility testing moves with it.

### Safety stays with the data

Being headless never moves a guarantee to the presenter. Where showing data carries a risk, the layer answers in a form that cannot be shown unsafely: a Markdown or rich-text field comes back as its safe structure, never as markup. Rules such as step-up, consent and visibility are enforced in the layer's functions, whatever screen calls them.

### Exceptions

A layer may keep an interface only when that interface carries a guarantee no other layer can give. Authentication's sign-in and step-up flows are the likely example. Each exception is argued in the layer's contract, kept as small as it can be, and still offered as services for hosts that draw their own.

## Applying it

| Layer state | How it adopts this |
|---|---|
| New, or not yet at 1.0 | Headless from the start |
| Released, with screens | Screens deprecated in a minor release and moved to a presentation layer, then removed in the next major release ([ADR-0008](ADR-0008-independent-layer-delivery.md)) |

`nuxt4-layers/publisher` is the first layer built to this decision, before its 1.0 release.

## Consequences

### Positive

- One place decides look, wording and layout, so a composed application feels like one product.
- Hosts compose services without competing routes, styles or pages.
- Layers are smaller, faster to test and free of design-system dependencies.
- Any front end can use a capability: a console, a public site, a mobile app or another service.

### Costs

- Every capability needs a presenter before people can use it, which means at least one presentation layer to build and maintain.
- Released layers face a major release to remove their screens.
- Accessibility and wording tests move out of the capability, so its own CI no longer proves how it looks.

## Guardrails

- A headless layer's tests prove its services and API; a presentation layer's tests prove its screens, including axe in light and dark mode.
- A layer's contract lists its public surface; any exception to this decision is named there with its reason.
- Presentation layers reach capabilities only through their contracts and APIs, never their storage or private code ([ADR-0001](ADR-0001-capability-repositories-and-composition-shells.md)).
