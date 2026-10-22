# ADR-0009 — Headless Capability Layers

**Status:** Accepted  
**Date:** 2026-10-21  
**Amended and accepted:** 2026-10-22: no screen exceptions for sign-in or step-up; client helpers and protocol endpoints named; wording keys, theme authority and person names placed; screens removed only once replaced  
**Amended:** 2026-10-22: `ui` offers styled, customisable components; kits are named `<capability>-ui`; composition kits and the host's part defined

> **In short:** capability layers do the work; they do not draw the screens. Each offers a clear set of services, and whoever composes it decides how those services look.

## Why

Several of our layers have grown their own screens, styles and wording. A host that composes them inherits several opinions about how things should look, and every layer repeats the same presentation work.

A layer's real value is what it knows and guarantees, such as a document's lifecycle or a person's consent. How that is shown is a separate concern, and it belongs to whoever composes the layer.

## The decision

Every capability layer, whether foundation, platform or domain, is **headless**.

| A headless layer offers | A headless layer never offers |
|---|---|
| **Contracts:** types, schemas and pure helpers | **Screens:** pages, routes or layouts |
| **Server functions:** one clear task each | **Components** or slots for other layers' components |
| **An HTTP API**, which the host may switch off | **Styling:** styles or class maps |
| **Protocol endpoints** a standard requires, such as a sign-in provider's callback or an emailed link that redirects; they answer a redirect or data, never a page | **Display wording** or its translations |
| **Events** it publishes, and **ports** it asks the host to fill | |
| **Data formats** such as feeds, export files and a theme's CSS | |
| **A typed client**, holding data, actions, navigation guards and browser ceremonies (such as a passkey prompt); guards take their destinations from configuration | |
| **Codes and keys:** error codes and `messageKey`s, with their parameters | |

### Theme Manager

Theme Manager is headless too. Its themes, tokens, contrast checks and preferences are services; its CSS is a data format, and applying the effective theme to the page is delivering that format, not drawing. It stays the **only authority** for the effective theme and its token vocabulary (`SemanticPresentationTheme`). `ui` consumes that vocabulary and never defines, validates or applies a theme of its own.

## Where presentation lives

| Who | Does what |
|---|---|
| **`nuxt4-layers/ui`** | Fully styled components on Theme Manager's tokens, customisable through props, slots, variants and the theme. Generic only: no component knows a capability. It includes the safe-tree renderer, a person-name component fed by the application, and the wording for generic text and common error codes |
| **Capability kits**, `<capability>-ui` | One capability's screens, built from `ui`, with their own wording, such as `profile-ui` |
| **Composition kits** | One journey across several capability kits, such as managing users; no data or services of their own |
| **Applications** | Compose capabilities, `ui` and kits; own everything specific to the product (below) |

A kit is created when a second application needs the same screens, or to receive a released layer's screens when they leave it, so they are moved rather than rewritten. Until then, screens live in the application. A component moves to `ui` once it knows no capability.

Every kit:

- serves one audience (people themselves, tenant administrators or platform operators) and never mixes self-service with administration;
- takes the tenant, the signed-in person and other capabilities' data from the application, never from a request or a lookup of its own;
- uses only typed clients and HTTP APIs, so it can be embedded anywhere and never depends on how data is stored;
- releases a new major whenever its capability's contract does.

`ui`, kits and applications use capabilities only through their contracts, APIs and typed clients. A capability never depends on them. A screen that shows another capability's data, such as a person's name, gets it through the application. The [presentation, internationalisation and accessibility baseline](../architecture/presentation-internationalisation-accessibility-v01.md) applies wherever screens are drawn.

## Safety stays with the data

Going headless never hands a guarantee to the screen.

- **Risky content is returned in a safe form.** Rich text comes back as structure that cannot be shown unsafely, never as markup.
- **Rules are enforced by the layer's functions.** Permissions, step-up, consent, rate limits and visibility hold whichever screen calls them.
- **What a screen must still do is written down.** Each layer's contract has a *Headless surface* section listing it, such as no framing or caching of administration screens, or the right `autocomplete` on credential fields.

## Exceptions

A layer may keep an interface only if it carries a guarantee that its server functions cannot enforce and no composing screen can be trusted to give. **None is granted.** Sign-in and step-up do not qualify: their guarantees are enforced on the server, and what their screens must do is listed in Authentication's *Headless surface*.

A future exception needs an amendment to this ADR, is argued in the layer's contract and threat model, kept as small as possible, and still offered as services.

## Adopting it

| Layer | Adopts it |
|---|---|
| New, or before 1.0 | Headless from the start. `publisher` is the first; Theme Manager reaches 1.0 headless |
| Released, with screens | Deprecates its screens in a minor release and removes them in the next major ([ADR-0008](ADR-0008-independent-layer-delivery.md)) |

A layer removes its screens only once their replacements are released in `ui`, a kit or the application, so no application is left without them. The [headless migration plan](../architecture/headless-migration-v01.md) sets the order.

## What follows

| Gains | Costs |
|---|---|
| A composed application looks and reads as one product | Every capability needs screens in `ui`, a kit or the application before people can use it |
| Hosts compose services without competing routes or styles | Released layers face a major release to drop their screens |
| Layers are smaller and quicker to test, with no design-system dependency | A capability's own tests no longer show how it looks |
| Any front end can use a capability: console, website, app or another service | Kits add repositories to release and keep in step |

## Guardrails

- A capability's contract lists its public surface and its *Headless surface* duties; a test fails if it ships screens, components, routes, styles, display wording or slots.
- A capability's tests prove its services and API; `ui`'s, kits' and applications' tests prove the screens, including accessibility in light and dark mode.
- `ui`, kits and applications never reach a capability's storage or private code ([ADR-0001](ADR-0001-capability-repositories-and-composition-shells.md)).
- A capability's codes and `messageKey`s are part of its contract: renaming or removing one is a breaking change.
