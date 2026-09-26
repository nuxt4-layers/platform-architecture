# Capability Manifest v0.1

**Status:** Provisional normative standard

## 1. Purpose

Each independently versioned layer SHOULD provide machine-readable capability metadata so tooling and composition applications can reason about provision, requirements and compatibility.

## 2. Minimum conceptual model

```json
{
  "schemaVersion": "1",
  "name": "@nuxt4-layers/example",
  "version": "1.0.0",
  "classification": "foundation",
  "provides": [
    { "capability": "ExampleService", "contractVersion": "1" }
  ],
  "requires": [
    { "capability": "IdentityService", "contractVersion": "^1", "optional": false }
  ]
}
```

## 3. Required semantics

The manifest MUST identify:

- manifest schema version;
- package/layer identity;
- layer version;
- classification;
- provided capabilities and contract versions;
- required capabilities and compatible versions;
- optionality of requirements.

Future revisions MAY add configuration-schema references, events, runtime requirements, security metadata and compatibility evidence.

## 4. Authority

The manifest describes machine-readable compatibility claims. It MUST agree with the normative human-readable contracts and package metadata. A manifest does not override an architectural contract.
