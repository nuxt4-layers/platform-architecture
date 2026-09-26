# Identity and Authorization Architecture v0.1

**Status:** Normative baseline

## 1. Separation of concerns

### Authentication
Establishes that a principal has authenticated and manages secure session lifecycle.

### Identity
Owns users, groups, memberships and identity/profile concepts required by applications.

### Authorization
Evaluates whether an actor may perform an action on a resource.

These capabilities MUST remain conceptually and contractually distinct.

## 2. Core identity model

The baseline model supports:

- User;
- Group;
- GroupMembership;
- zero-to-many group memberships per user;
- application-defined profile information where appropriate.

Group membership MUST be represented explicitly rather than by a single `groupId` on a user.

## 3. Authorization model

Authorization MUST be resource-aware. Simple global roles such as ADMIN/EDITOR/USER MAY exist but MUST NOT be the only mechanism where resources have owners or scoped access.

A decision conceptually evaluates:

```text
actor + action + resource + relevant context -> decision
```

Resources MAY support user ownership, group ownership, explicit grants, role/permission rules, or domain-specific policies.

## 4. Enforcement

Authoritative authorization MUST occur on the server for every protected operation. Route middleware and hidden UI elements MAY improve user experience but MUST NOT be relied upon as enforcement.

Database row-level security MAY be used as an additional containment mechanism.

## 5. Sessions

Session identifiers/tokens MUST be protected using contemporary secure-cookie/session practices. Session rotation, revocation, expiry, reauthentication for sensitive operations, and visibility of active sessions SHOULD be supported according to risk.

## 6. Administrative boundaries

Administrative privileges SHOULD be scoped and least-privilege. Application administration, platform administration and resource ownership SHOULD not be conflated unless explicitly required.
