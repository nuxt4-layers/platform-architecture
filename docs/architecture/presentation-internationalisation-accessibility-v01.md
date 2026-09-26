# Presentation, Internationalisation and Accessibility Architecture v0.1

**Status:** Normative baseline

## 1. Purpose

Presentation quality, internationalisation, accessibility and semantic web support are platform-wide concerns. They MUST be designed in from the start rather than added after a feature is complete.

## 2. Presentation system

Reusable presentation capabilities MUST use a shared design system built from semantic design tokens. Applications MUST NOT rely on scattered hard-coded presentation values.

Where applicable, the presentation system MUST support semantic colour roles; typography and font preferences; spacing, sizing, density and radius tokens; responsive layouts; light, dark and high-contrast modes; operating-system preferences; user-selectable presentation preferences; and persistence through an appropriate settings contract.

User customisation MUST NOT break mandatory accessibility requirements. Theme and preference implementations MUST constrain, validate, adjust or reject combinations when needed to preserve contrast, readability, visible focus, reflow and usable interaction.

## 3. Internationalisation

Internationalisation is a standard platform capability.

Applications and reusable capabilities MUST keep user-facing text out of domain logic and make that text localisable. They MUST declare the document language correctly and support locale-aware formatting. Each capability MUST be able to own its translation resources; the application shell MUST NOT be forced to maintain one large translation catalogue.

Locale-aware routing and translated metadata SHOULD be supported where public content requires them. An application MAY initially enable only one locale, but its architecture MUST NOT unnecessarily prevent additional locales.

## 4. Accessibility

WCAG 2.2 Level AA is the minimum accessibility engineering target for web presentation produced by the ecosystem.

Accessibility is a component and capability acceptance criterion. Implementations MUST account for applicable semantic HTML, keyboard operation, visible/predictable focus, accessible names and labels, contrast, text resizing/reflow, target sizing, reduced motion, status/error communication, language metadata and assistive-technology compatibility.

ARIA MUST supplement native semantics where necessary and MUST NOT replace suitable native HTML semantics without justification.

Automated accessibility testing SHOULD form part of CI, but MUST NOT be treated as sufficient evidence of conformance without appropriate manual verification.

## 5. Semantic web and structured data

Public-facing capabilities MUST prefer semantic HTML and meaningful document structure.

When structured data is useful for public content, applications SHOULD use appropriate Schema.org vocabulary in a machine-readable form such as JSON-LD. Structured data MUST truthfully describe the resource and MUST NOT invent information purely for search presentation.

Metadata, canonical URLs, locale alternates and structured data SHOULD be generated from authoritative domain data rather than duplicated manually where practical.

## 6. Layer responsibilities

The UI capability owns reusable presentation primitives, design-token consumption and component-level accessibility behaviour.

Internationalisation, semantic metadata and user-preference persistence MAY be separately bounded capabilities or integrations where their responsibilities justify separation. Their public contracts MUST remain independent of any single replaceable implementation library.

Domain capabilities SHOULD supply semantic content and metadata inputs without owning global presentation infrastructure.

## 7. Verification

Reusable UI components SHOULD have automated checks for accessibility-relevant behaviour where feasible. Consuming applications MUST verify accessibility at the composed-page and workflow level because component-level compliance alone cannot establish application-level WCAG conformance.

Internationalisation tests SHOULD include missing-message behaviour and locale-sensitive formatting. Structured data SHOULD be schema-validated where practical.
