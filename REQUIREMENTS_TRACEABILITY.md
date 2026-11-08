# MASTER — Requirements Traceability

This file maps requirements to design locations and future verification evidence. Update it with every material requirement or implementation change.

| ID | Requirement | Design reference | Verification evidence | Status |
|---|---|---|---|---|
| FR-001 | Customer can create a service request | SPECIFICATION §5 | Request-flow integration test | PROPOSED |
| FR-002 | Provider can submit an offer | SPECIFICATION §6 | Offer/authorization tests | PROPOSED |
| FR-003 | Agreed terms are versioned and immutable | SPECIFICATION §7–8; DATA_MODEL | Persistence and audit tests | PROPOSED |
| FR-004 | Scope/price changes require approvals | SPECIFICATION §8 | Change-request contract tests | PROPOSED |
| FR-005 | Evidence has provenance and access controls | SPECIFICATION §9; SECURITY | Upload/access/audit tests | PROPOSED |
| FR-006 | Complaint gives both parties a response path | SPECIFICATION §10 | Complaint workflow tests | PROPOSED |
| FR-007 | Expert panel supports recusal and inconclusive outcome | SPECIFICATION §10 | Expert-panel policy tests | PROPOSED |
| FR-008 | Category workflows are configurable/versioned | ARCHITECTURE §5 | Category contract tests | PROPOSED |
| FR-009 | Payment status comes from verified provider evidence | PAYMENT_MODEL | Webhook/reconciliation tests | BLOCKED |
| ID-001 | Account authentication is distinct from Person identity | IDENTITY_AND_ORGANIZATION_MODEL §2 | Account/person linking and recovery tests | PROPOSED |
| ID-002 | Party distinguishes individual and organization participants | IDENTITY_AND_ORGANIZATION_MODEL §2–3 | Party relationship and persistence tests | PROPOSED |
| ID-003 | One Person may act as customer and provider in different workflows | IDENTITY_AND_ORGANIZATION_MODEL §3 | Multi-capacity authorization tests | PROPOSED |
| ID-004 | Organization actions require active membership and scoped permissions | IDENTITY_AND_ORGANIZATION_MODEL §2.5–2.6 | Membership/role authorization matrix tests | PROPOSED |
| ID-005 | Representation is separate from membership and time/scope bounded | IDENTITY_AND_ORGANIZATION_MODEL §2.7 | Representation proof, expiry, and revocation tests | PROPOSED |
| ID-006 | Every consequential action records actor and acting Party | DATA_MODEL §4, §9 | Audit attribution tests | PROPOSED |
| SEC-001 | Cross-tenant access is denied unless explicitly granted | SECURITY §4 | API/database/storage/search/cache/export isolation tests | PROPOSED |
| SEC-002 | Membership and representation revocation blocks new actions | SECURITY §4 | Revocation and delayed-job authorization tests | PROPOSED |
| SEC-003 | Platform staff privileges are separate and audited | SECURITY §5 | Privilege boundary and access audit tests | PROPOSED |
| NFR-001 | Every resource enforces object-level authorization | SECURITY §2–4 | Authorization matrix tests | PROPOSED |
| NFR-002 | Sensitive data is minimized and classified | DATA_MODEL; DATA_PROTECTION | Data inventory/privacy review | PROPOSED |
| NFR-003 | Russian localization and cross-border flows reviewed | DATA_PROTECTION §3–6 | Written legal/data-flow assessment | BLOCKED |
| NFR-004 | Backup restoration is tested | SECURITY §2 | Restore drill report | PROPOSED |
| NFR-005 | Provider integrations are replaceable | ARCHITECTURE §6 | Adapter contract tests | PROPOSED |
| GOV-001 | Material changes have impact analysis | CHANGE_MANAGEMENT | PR/ADR review | ACCEPTED |
| GOV-002 | CI evidence is tied to exact commit | CHANGE_MANAGEMENT §4–5 | CI run/job/artifact references | ACCEPTED |

**Status meanings:** PROPOSED = requirement defined, implementation absent; BLOCKED = dependent legal/provider decision unresolved; ACCEPTED = governance rule approved. PASS at design review means the principle is accepted, not that a requirement is implemented or verified. Do not mark IMPLEMENTED or VERIFIED without fresh evidence.
