# ADR-0011 — Shared Rich Text and Configurable Copy

**Status:** Proposed  
**Date:** 2026-10-21

> **In short:** every piece of copy is data. It is written in one shared rich-text format, checked by the layer that stores it, and edited through one editor that offers exactly what each field allows, from a single label to a full document.

## Why

Copy appears everywhere: published articles, a person's bio, a site's notices, a button's label. If each layer chose its own format, its own safety rules and its own editor, the same text would behave differently in each place, and one weak spot would put every reader at risk. Copy written into code cannot be changed without a release.

Copy also differs in what it needs. A label needs a single line; an article needs headings, tables and images. One editor should serve both, without giving a label the powers of an article.

## The decision

### 1. One rich-text format, in a shared library

The platform has one rich-text format: a safe subset of Markdown. It lives in **`nuxt4-layers/rich-text`**, a stateless library holding:

- the format's rules: which constructs and links are allowed;
- the parser, which turns text into a **safe tree** that can never carry markup;
- the checks a layer runs before saving;
- the profiles below.

### 2. Profiles, from label to document

Every rich-text field declares a **profile** and a maximum length.

| Profile | Allows | Typical use |
|---|---|---|
| **Label** | Plain text, one line | A button, a menu item, a setting's name |
| **Inline** | Bold, italic, links | A tagline, a caption, a short notice |
| **Block** | Paragraphs, lists, headings, quotes | A bio, a description, a summary |
| **Document** | Everything, including tables, images and links between documents | An article or a page |

### 3. The layer that stores it enforces it

The capability that owns a field checks every save against its profile, using `rich-text`. Anything beyond the profile is refused, whichever screen sent it. Readers receive only the safe tree.

### 4. One editor, adapting to the profile

**`nuxt4-layers/content-editor`** is the platform's editing component, built on `ui` and `rich-text`. It reads a field's profile and offers only those tools: a plain text box for a label, a full writing surface for a document. It edits text and hands it back; it knows nothing about the capability that stores it.

### 5. No hard-coded copy

Text a person reads is data, never code.

| Copy | Lives in |
|---|---|
| Published content | The publisher |
| A person's own words | Profile |
| A site's or organisation's text | Configuration |
| Interface labels and messages | Translation resources the application can override |

### 6. Capabilities may depend on stateless libraries

A capability may depend on a `nuxt4-layers` library that holds no state, no ports and no screens, such as `rich-text`, as it depends on `zod`. Capabilities still never depend on one another ([ADR-0001](ADR-0001-capability-repositories-and-composition-shells.md)).

## Adopting it

| Layer | Adopts it |
|---|---|
| `rich-text` | Created from the publisher's existing dialect, with profiles added |
| `publisher` | Uses `rich-text` before its 1.0 release; its own dialect is removed |
| `content-editor` | Built on `ui` and `rich-text` as a presentation layer ([ADR-0009](ADR-0009-headless-capability-layers.md)) |
| Profile, Configuration and others | Use `rich-text` for any field longer than a label, in their next release |

## What follows

| Gains | Costs |
|---|---|
| Text behaves and is protected the same everywhere | Every capability depends on one library, so its releases must be careful |
| A field's limits are clear to editors, servers and readers alike | Profiles must be chosen for every field |
| Copy changes without a release | Interface wording needs translation resources the application can edit |
| One editor to learn, build and test | `content-editor` must serve very different needs well |

## Guardrails

- `rich-text` never renders HTML; it returns safe trees only.
- A change that loosens a profile, or widens what the format allows, needs a new ADR.
- A stateless library holds no storage, ports, screens or configuration of its own.
