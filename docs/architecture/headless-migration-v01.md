# Headless Migration v0.1

> **In short:** build what `ui` must offer first, move each released layer's screens into a kit, then remove them in that layer's next major release, one layer at a time. No application is ever left without a screen.

Governed by [ADR-0009](../decisions/ADR-0009-headless-capability-layers.md) and [ADR-0011](../decisions/ADR-0011-shared-rich-text-and-configurable-copy.md). The publisher is the worked example: its [headless migration](https://github.com/nuxt4-layers/publisher/blob/4410cab7b84c1b0395de2bdf657e3bcc3d476b87/docs/headless-migration.md) and its contract's [*Headless surface*](https://github.com/nuxt4-layers/publisher/blob/4410cab7b84c1b0395de2bdf657e3bcc3d476b87/docs/contracts.md#15-headless-surface).

## Why

Six layers still ship screens, wording and styles. Removing them is a breaking change, and the screens are in use, so the replacements must exist before anything is removed.

## Decisions

| Question | Decision |
|---|---|
| Sequencing | `ui`'s first release comes before any layer removes a screen. A layer's major release waits for its kit's release |
| Released layers | Deprecate in a minor release, remove in the next major ([ADR-0008](../decisions/ADR-0008-independent-layer-delivery.md)). Theme Manager, before 1.0, removes them at 1.0 |
| Order | Theme Manager, Configuration, Profile, Identity, Authorisation, Authentication: least depended-on first, sign-in last |
| Exceptions | None. Sign-in and step-up are enforced on the server; their screens' duties go in Authentication's *Headless surface* |
| Kits | Created when a second application needs them, or to receive a released layer's screens. No `content-ui` yet: no second application needs it |
| Wording | Capabilities publish codes and `messageKey`s; `ui` and kits hold the wording; applications override it |
| Shared guarantees | Each contract lists its screens' duties; the [baseline's screen checklist](presentation-internationalisation-accessibility-v01.md#8-screen-checklist) says how they are proved |
| Rich text | Profile and Configuration adopt `rich-text` in the same major release, so each contract breaks once |

## What `ui` offers first

`ui` today (0.0.0) defines its own tokens and theme handling, which competes with Theme Manager. Its first release fixes that and provides what the existing screens use.

| Offer | Replaces |
|---|---|
| Theme Manager's `SemanticPresentationTheme` vocabulary only; its own tokens, theme validation and application removed | `ui`'s `tokens.css` and `./theme` |
| Primitives: button, link, field with label and error, choice, dialog, alert, table, list, empty and unavailable states | Each layer's `Alert`, `Field`, `Unavailable` and `SignedOut` |
| Safe-tree renderer for every `rich-text` profile, with links resolved by the caller | The publisher's removed renderer |
| `PersonName`, fed by a resolver the application supplies, with a stable fallback label | `ProfilePersonName`, `IdentityPersonName`, `AuthorisationPersonName` |
| Messages: keyed wording with application overrides, and wording for common error codes | Each layer's `messages.ts` and `use*Text()` |
| Accessibility test kit: axe in light and dark mode, keyboard and focus helpers | Each layer's Playwright and axe set-up |
| Response-header settings for administration and credential screens | Each layer's own framing and caching rules |

## Kits

| Kit | Receives |
|---|---|
| `settings-ui` (new) | Theme Manager's library and editor; Configuration's settings and pending changes |
| `user-management-ui` | Authentication's sign-in, sign-up, password, multi-factor, break-glass and security screens; Profile's own profile, disclosure, export, contact verification, person, requests and departures |
| `group-management-ui` | Identity's account, groups, members, invitations, join requests, changes, safety periods and service identities; Authorisation's roles, assignments, sharing, access review and changes |

Screens are moved, then rebuilt on `ui`'s primitives and Theme Manager's vocabulary. Each kit keeps its sources' accessibility tests and wording.

## Per layer

| Layer | Goes | Stays |
|---|---|---|
| Theme Manager 0.2.1 | `app/pages/`, `app/components/` | Contracts, server functions, API, `presentation.css`, the runtime that applies the effective theme, `useThemeRuntime` and preferences |
| Configuration 1.2.0 | `app/components/`, `app/configuration-pages/`, `modules/configuration-pages.ts`, messages, class map, pairings, `configuration-format.ts`, `useConfigurationRoutes`, `useConfigurationText`, browser tests | `useConfiguration`, the administration clients; `useConfigurationHead` becomes data the screen applies |
| Profile 1.0.0 | `presentation/`, `modules/presentation.ts`, `./presentation`, `./tailwind.css`, `useProfileText`, `useProfileRoutes`, `useProfileAction`, browser tests | `useProfile`, `useProfileNames`, `forgetProfileNames`, every server function and endpoint |
| Identity 1.0.0 | As Profile | `useIdentity`, conformance suite |
| Authorisation 1.0.0 | As Profile | `useAuthorisation` |
| Authentication 1.0.0 | As Profile | `useAuthentication`, its plugin and navigation guards; protocol endpoints |

Each layer also gains a *Headless surface* section in its contract, a headless test, and the switch for its HTTP API.

## Order of work

Each step is one draft pull request in one repository, against released tags.

| Step | Repository | Release |
|---|---|---|
| 1 | `ui`: the offers above | 0.1.0 |
| 2 | Each released layer: *Headless surface* section, deprecation notices on its screens, API switch | Minor, in any order |
| 3 | `settings-ui` | 0.1.0 |
| 4 | Theme Manager: screens removed | 1.0.0 |
| 5 | Configuration: screens removed; long fields on `rich-text` | 2.0.0 |
| 6 | `user-management-ui`: Profile's screens | 0.1.0 |
| 7 | Profile: screens removed; long fields on `rich-text` | 2.0.0 |
| 8 | `group-management-ui` | 0.1.0 |
| 9 | Identity, then Authorisation: screens removed | 2.0.0 each |
| 10 | `user-management-ui`: Authentication's screens | 0.2.0 |
| 11 | Authentication: screens removed | 2.0.0 |

`iam-integration` and the test harness are recomposed at each release, as ADR-0008 requires.

## Risks

| Risk | Treatment |
|---|---|
| An application loses a screen | No major release before its kit's release; deprecation notices name the replacement |
| A guarantee slips when a screen moves | The *Headless surface* section lists it; the kit's tests prove it against that contract |
| Two majors for one layer | Rich text and screen removal share one major |
| `ui` and Theme Manager drift | Theme Manager owns the vocabulary; `ui` only consumes it |
| Kits outnumber their users | Three kits, one per area, not one per layer |
