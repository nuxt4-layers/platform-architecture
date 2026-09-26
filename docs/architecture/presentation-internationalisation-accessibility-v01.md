# Presentation, Internationalisation and Accessibility Architecture v0.1

**Status:** Normative baseline

## 1. Purpose

Presentation quality, internationalisation, accessibility and semantic web support are platform concerns and MUST be designed into capabilities from inception rather than retrofitted after feature completion.

## 2. Presentation system

Reusable presentation capabilities MUST use a coherent design system based on semantic design tokens rather than application-specific hard-coded presentation values.

The presentation system MUST support, where applicable, semantic colour roles, typography/font preferences, spacing/sizing/density/radius tokens, responsive layout primitives, light/dark/high-contrast presentation, operating-system preferences, user-selectable presentation preferences and persistence through an appropriate settings contract.

User customisation MUST NOT silently invalidate mandatory accessibility requirements. Theme and preference implementations MUST constrain, validate, derive or reject combinations where necessary to preserve required contrast, readability, focus visibility, reflow and interaction characteristics.

## 3. Internationalisation

Internationalisation is a standard platform capability.

Applications and reusable capabilities MUST avoid embedding user-facing language in domain logic, make user-facing strings localisable, declare document language correctly, support locale-aware formatting, preserve capability-owned translation resources and avoid requiring one monolithic shell-owned translation catalogue.

Locale-aware routing and translated metadata SHOULD be supported where public content requires them. An application MAY initially enable only one locale, but its architecture MUST NOT unnecessarily prevent additional locales.

## 4. Accessibility

WCAG 2.2 Level AA is the minimum accessibility engineering target for web presentation produced by the ecosystem.

Accessibility is a component and capability acceptance criterion. Implementations MUST account for applicable semantic HTML, keyboard operation, visible/predictable focus, accessible names and labels, contrast, text resizing/reflow, target sizing, reduced motion, status/error communication, language metadata and assistive-technology compatibility.

ARIA MUST supplement native semantics where necessary and MUST NOT replace suitable native HTML semantics without justification.

Automated accessibility testing SHOULD form part of CI, but MUST NOT be treated as sufficient evidence of conformance without appropriate manual verification.

## 5. Semantic web and structured data

Public-facing capabilities MUST prefer semantic HTML and meaningful document structure.

Where structured data materially describes public content, applications SHOULD expose appropriate Schema.org vocabulary using a machine-readable representation such as JSON-LD. Structured data MUST describe visible resource data truthfully and MUST NOT invent facts solely for search presentation.

Metadata, canonical URLs, locale alternates and structured data SHOULD be generated from authoritative domain data rather than duplicated manually where practical.

## 6. Layer responsibilities

The UI capability owns reusable presentation primitives, design-token consumption and component-level accessibility behaviour.

Internationalisation, semantic metadata and user-preference persistence MAY be separately bounded capabilities or integrations where their responsibilities justify separation. Their public contracts MUST remain independent of any single replaceable implementation library.

Domain capabilities SHOULD supply semantic content and metadata inputs without owning global presentation infrastructure.

## 7. Verification

Reusable UI components SHOULD have automated checks for accessibility-relevant behaviour where feasible. Consuming applications MUST verify accessibility at the composed-page and workflow level because component-level compliance alone cannot establish application-level WCAG conformance.

Internationalisation tests SHOULD include missing-message behaviour and locale-sensitive formatting. Structured data SHOULD be schema-validated where practical.
