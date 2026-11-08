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
| FR-008 | Category workflows are configurable/versioned | ARCHITECTURE §4 | Category contract tests | PROPOSED |
| FR-009 | Payment status comes from verified provider evidence | PAYMENT_MODEL | Webhook/reconciliation tests | BLOCKED |
| NFR-001 | Every resource enforces object-level authorization | SECURITY §2 | Authorization matrix tests | PROPOSED |
| NFR-002 | Sensitive data is minimized and classified | DATA_MODEL; DATA_PROTECTION | Data inventory/privacy review | PROPOSED |
| NFR-003 | Russian localization and cross-border flows reviewed | DATA_PROTECTION §3–6 | Written legal/data-flow assessment | BLOCKED |
| NFR-004 | Backup restoration is tested | SECURITY §2 | Restore drill report | PROPOSED |
| NFR-005 | Provider integrations are replaceable | ARCHITECTURE §5 | Adapter contract tests | PROPOSED |
| GOV-001 | Material changes have impact analysis | CHANGE_MANAGEMENT | PR/ADR review | ACCEPTED |
| GOV-002 | CI evidence is tied to exact commit | CHANGE_MANAGEMENT §4–5 | CI run/job/artifact references | ACCEPTED |

**Status meanings:** PROPOSED = requirement defined, implementation absent; BLOCKED = dependent legal/provider decision unresolved; ACCEPTED = governance rule approved. Do not mark IMPLEMENTED or VERIFIED without fresh evidence.
