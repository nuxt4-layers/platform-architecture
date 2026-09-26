# Privacy Architecture v0.1

**Status:** Normative baseline

## 1. Principle

Privacy and data protection are part of the platform architecture. Compliance MUST NOT depend only on a cookie banner or privacy-policy page.

## 2. Scope

The architecture must be capable of governing:

- cookies;
- browser/local storage;
- similar client storage/access technologies;
- telemetry and analytics;
- third-party integrations;
- personal-data collection and purpose;
- consent/preferences where consent is the applicable basis;
- retention;
- export/access;
- deletion;
- auditability of privacy choices.

## 3. Data minimisation

Capabilities MUST collect and retain only data justified by their documented purposes. Optional telemetry SHOULD be absent by default in the initial platform unless a defined need justifies it.

## 4. Storage/access classification

Client storage/access mechanisms MUST be inventoried and classified by purpose and necessity. Mechanisms requiring consent MUST NOT be activated before valid consent.

Technically necessary mechanisms MUST still be documented transparently.

## 5. Jurisdiction

Reusable capability contracts SHOULD avoid hard-coding the rules of one jurisdiction. Applications MAY provide policies or adapters for the jurisdictions they need to support.

The initial personal platform is expected to design first for applicable UK data-protection and electronic-communications requirements while preserving extension points for other jurisdictions.

## 6. User rights and lifecycle

The architecture SHOULD support data access/export, correction where applicable, deletion/erasure workflows, retention rules, and account closure without requiring domain data to be manually discovered.

## 7. Security relationship

Privacy and security overlap, but they are not the same. Authorization controls who can access data. Privacy also asks whether the data should be collected, processed, retained, disclosed or transferred at all.
