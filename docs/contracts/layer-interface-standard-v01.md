# Layer Interface Standard v0.1

**Status:** Normative

## 1. Objective

Public contracts make layer boundaries clear, testable and replaceable.

## 2. Contract categories

A layer MAY expose:

- **Service contracts** — callable capability interfaces.
- **Data contracts** — stable cross-boundary values/types.
- **Command contracts** — requested state changes.
- **Query contracts** — read operations.
- **Event contracts** — facts emitted after domain events.
- **Configuration contracts** — validated consumer configuration.
- **Error contracts** — documented failure categories, as codes and `messageKey`s, never wording.
- **HTTP API contracts** — endpoints the host may switch off.
- **Port contracts** — what the layer asks the host to supply.
- **Typed clients** — data and actions for screens; never components.

A capability never exposes screens, components, slots, styles or display wording ([ADR-0009](../decisions/ADR-0009-headless-capability-layers.md)).

## 3. Rules

Contracts MUST use the language of the domain. They MUST NOT expose types from a replaceable provider's SDK unless the architecture deliberately makes that provider part of the public contract.

Public contract exports SHOULD be available from a deliberate package entry point such as `/contracts` or an equivalent documented export map.

Consumers MUST NOT import from paths designated `internal`, private server implementation paths, persistence adapters, or undocumented source paths.

## 4. Service example

```ts
export interface AuthorisationService {
  can(actor: Actor, action: Action, resource: Resource): Promise<AuthorisationDecision>
}
```

This contract says what authorisation provides without dictating its policy engine or database.

## 5. Evolution

Removing a public member, narrowing accepted input, changing documented semantics, or changing a public data shape incompatibly is a breaking contract change.

Adding something to a contract is not automatically safe. Its effects on behaviour and the type system MUST still be considered.

## 6. Validation

Contracts crossing trust boundaries MUST validate runtime input. TypeScript types alone are not runtime validation.
