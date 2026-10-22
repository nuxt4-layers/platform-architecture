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
| Kits | One per capability, named `<capability>-ui`, created to receive a released layer's screens or when a second application needs them. No `publisher-ui` yet |
| User management | Built in `company-console` first; moved to the composition kit `user-management-ui` when a second application needs it |
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

| Kit | Audience | Receives |
|---|---|---|
| `theme-manager-ui` (new) | Administrators | Theme library and editor |
| `configuration-ui` (new) | Administrators | Settings and pending changes |
| `profile-ui` (new) | People; operators for requests and departures | Own profile, disclosure, export, contact verification, person, requests, departures |
| `identity-ui` (new) | People; administrators | Account, groups, members, invitations, join requests, changes, safety periods, service identities. This is group management |
| `authorisation-ui` (new) | Administrators | Roles, assignments, sharing, access review, changes |
| `authentication-ui` (new) | People; operators for break-glass | Sign-in, sign-up, passwords, multi-factor, step-up, break-glass, security settings |

Screens are moved, then rebuilt on `ui`'s components and Theme Manager's vocabulary. Each kit keeps its sources' accessibility tests and wording. The empty `group-management-ui` repository is retired; `user-management-ui` is kept for the composition kit.

## Host application

Built in the host first; moved to a kit when a second application needs it.

| Concern | Why there |
|---|---|
| Routes, shell, navigation and layout | Only the product knows its structure |
| Which theme each tenant uses | A choice of Theme Manager data |
| Final wording and tenant overrides | Kits give defaults; the product decides |
| Ports: persistence, key wrapper, signed-in person, tenant resolver, rate limits | The host supplies them ([ADR-0002](../decisions/ADR-0002-composition-supplied-persistence-and-capability-owned-schemas.md)) |
| Wiring between capabilities: names, `iam-integration`'s adapters | Capabilities never import one another |
| Security per route: headers, step-up, administration on its own host | Depends on deployment |
| Page metadata, search and structured data | Built from capabilities' answers |
| Feature switches and tenant entitlements | Product and commercial decisions |
| Journeys across capabilities, such as user management | Until a second application needs them |
| Whole-page and workflow tests, accessibility included | [Baseline](presentation-internationalisation-accessibility-v01.md) §7 |

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
| 3 | `theme-manager-ui`, then `configuration-ui` | 0.1.0 each |
| 4 | Theme Manager: screens removed | 1.0.0 |
| 5 | Configuration: screens removed; long fields on `rich-text` | 2.0.0 |
| 6 | `profile-ui` | 0.1.0 |
| 7 | Profile: screens removed; long fields on `rich-text` | 2.0.0 |
| 8 | `identity-ui`, then `authorisation-ui` | 0.1.0 each |
| 9 | Identity, then Authorisation: screens removed | 2.0.0 each |
| 10 | `authentication-ui` | 0.1.0 |
| 11 | Authentication: screens removed | 2.0.0 |

`iam-integration` and the test harness are recomposed at each release, as ADR-0008 requires.

## Risks

| Risk | Treatment |
|---|---|
| An application loses a screen | No major release before its kit's release; deprecation notices name the replacement |
| A guarantee slips when a screen moves | The *Headless surface* section lists it; the kit's tests prove it against that contract |
| Two majors for one layer | Rich text and screen removal share one major |
| `ui` and Theme Manager drift | Theme Manager owns the vocabulary; `ui` only consumes it |
| Kits outnumber their users | One per released layer with screens, no more; journeys stay in the application until a second one needs them |
