# Identity and Authorization Architecture v0.1

**Status:** Normative baseline

## 1. Separation of concerns

### Authentication
Establishes who has signed in and manages the secure lifecycle of that authenticated session.

### Identity
Owns users, groups, memberships and identity/profile concepts required by applications.

### Authorization
Decides whether an actor may perform an action on a resource.

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

Authorization MUST consider the resource being accessed. Simple global roles such as ADMIN, EDITOR or USER MAY exist, but they MUST NOT be the only access-control mechanism when resources have owners or scoped access.

A decision conceptually evaluates:

```text
actor + action + resource + relevant context -> decision
```

Resources MAY support user ownership, group ownership, explicit grants, role/permission rules, or domain-specific policies.

## 4. Enforcement

Every protected operation MUST be authorized on the server. Route middleware and hidden UI elements MAY improve the user experience, but they MUST NOT be treated as security enforcement.

Database row-level security MAY be used as an additional containment mechanism.

## 5. Sessions

Session identifiers/tokens MUST be protected using contemporary secure-cookie/session practices. Session rotation, revocation, expiry, reauthentication for sensitive operations, and visibility of active sessions SHOULD be supported according to risk.

## 6. Administrative boundaries

Administrative privileges SHOULD be scoped and least-privilege. Application administration, platform administration and resource ownership SHOULD not be conflated unless explicitly required.
