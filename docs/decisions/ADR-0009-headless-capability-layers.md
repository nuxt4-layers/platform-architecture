# ADR-0009 — Headless Capability Layers

**Status:** Proposed  
**Date:** 2026-10-21

> **In short:** capability layers do the work; they do not draw the screens. Each offers a clear set of services, and whoever composes it decides how those services look.

## Why

Several of our layers have grown their own screens, styles and wording. A host that composes them inherits several opinions about how things should look, and every layer repeats the same presentation work.

A layer's real value is what it knows and guarantees, such as a document's lifecycle or a person's consent. How that is shown is a separate concern, and it belongs to whoever composes the layer.

## The decision

Foundation and platform layers are **headless by default**.

| A headless layer offers | A headless layer never offers |
|---|---|
| **Contracts:** types, schemas and pure helpers | **Screens:** pages, routes or layouts |
| **Server functions:** one clear task each | **Components** or slots for other layers' components |
| **An HTTP API**, which the host may switch off | **Styling:** styles, class maps or design tokens |
| **Events** it publishes, and **ports** it asks the host to fill | **Display wording** or its translations |
| **Data formats** such as feeds and export files | |
| **A typed client**, if useful, holding data and actions only | |

This includes Theme Manager: its themes, tokens, contrast checks and preferences are services, and its generated CSS is a data format. Its token vocabulary is the contract that presentation builds on.

## Where presentation lives

| Who | Does what |
|---|---|
| **`nuxt4-layers/ui`** | The component library: generic components built on Theme Manager's token vocabulary |
| **Kits** (optional) | One capability's screens built from `ui`, such as a publishing console. Created only when a second application needs the same screens |
| **Applications** | Compose capabilities and `ui`, build their own screens, and own routes, branding and wording |

`ui`, kits and applications use capabilities only through their contracts and APIs. A capability never depends on a presentation layer. The [presentation, internationalisation and accessibility baseline](../architecture/presentation-internationalisation-accessibility-v01.md) applies wherever screens are drawn, and accessibility testing lives there too.

## Safety stays with the data

Going headless never hands a guarantee to the screen.

- **Risky content is returned in a safe form.** Rich text comes back as structure that cannot be shown unsafely, never as markup.
- **Rules are enforced by the layer's functions.** Permissions, step-up, consent and visibility hold whichever screen calls them.

## Exceptions

A layer may keep an interface only if that interface carries a guarantee no other layer can give. Sign-in and step-up are the likely examples. An exception is:

- argued in the layer's contract;
- kept as small as possible;
- still offered as services, for hosts that draw their own.

## Adopting it

| Layer | Adopts it |
|---|---|
| New, or not yet at 1.0 | Headless from the start. `publisher` is the first |
| Released, with screens, including Theme Manager | Moves its screens to `ui`, a kit or its applications, deprecating them in a minor release and removing them in the next major ([ADR-0008](ADR-0008-independent-layer-delivery.md)) |

## What follows

| Gains | Costs |
|---|---|
| A composed application looks and reads as one product | Every capability needs screens in `ui`, a kit or the application before people can use it |
| Hosts compose services without competing routes or styles | Released layers face a major release to drop their screens |
| Layers are smaller and quicker to test, with no design-system dependency | A capability's own tests no longer show how it looks |
| Any front end can use a capability: console, website, app or another service | |

## Guardrails

- A capability's contract lists its public surface, and names any exception with its reason.
- A capability's tests prove its services and API; `ui`'s and the application's tests prove the screens, including accessibility in light and dark mode.
- `ui`, kits and applications never reach a capability's storage or private code ([ADR-0001](ADR-0001-capability-repositories-and-composition-shells.md)).
