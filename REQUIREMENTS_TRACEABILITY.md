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
| FR-009 | Payment status comes from verified provider evidence | PAYMENT_MODEL; TRANSACTION_FINANCIAL_MODEL §§3–5 | Webhook/reconciliation tests | BLOCKED |
| FIN-001 | PaymentIntent, Payment, Transaction, Refund, Payout, Settlement, and Ledger are separate concepts | TRANSACTION_FINANCIAL_MODEL §3 | Domain schema and lifecycle contract tests | PROPOSED |
| FIN-002 | PartyType, OrganizationType, and TaxStatus constrain but do not determine legal payment flow | TRANSACTION_FINANCIAL_MODEL §2; BUSINESS_AND_LEGAL_MODEL | Scenario legal-role matrix and provider confirmation | BLOCKED |
| FIN-003 | Payment, Refund, Payout, and Settlement have independent evidence-driven state machines | TRANSACTION_FINANCIAL_MODEL §4 | State-transition and provider-adapter contract tests | PROPOSED |
| FIN-004 | Ledger uses balanced append-only double-entry records and is not a wallet | TRANSACTION_FINANCIAL_MODEL §§3.4, 5 | Accounting design review and journal invariant tests | PROPOSED |
| FIN-005 | Financial terminal states require authoritative provider/bank evidence and reconciliation | TRANSACTION_FINANCIAL_MODEL §§4–5 | Raw event, idempotency, reconciliation, and audit tests | PROPOSED |
| FIN-006 | Live flows require scenario-specific legal, provider, fiscalization, accounting, and security approval | TRANSACTION_FINANCIAL_MODEL §§2, 6–8; ADR-2026-011 | Written legal/provider approvals and end-to-end test evidence | BLOCKED |
| FIN-007 | Customer-facing supplier/MoR and statutory roles are resolved per scenario; ADR-2026-011 remains unapproved | ADR-2026-011; TRANSACTION_FINANCIAL_MODEL §2 | Written legal opinion, provider confirmation, fiscalization matrix | BLOCKED |
| FIN-008 | Provider eligibility distinguishes ordinary individual, NPD, IP individual profile, IP workspace, legal entity and TEAM patterns | TRANSACTION_FINANCIAL_MODEL §2.2 | Scenario-specific legal/tax/provider review | BLOCKED |
| FIN-009 | IP profile and legal counterparty mapping are frozen for each accepted Order | TRANSACTION_FINANCIAL_MODEL §2.2; IDENTITY_AND_ORGANIZATION_MODEL | Agreement snapshot and profile-switch regression tests | PROPOSED |
| FIN-010 | Ledger account catalogue and journal entries are source-backed, balanced and do not imply custody | TRANSACTION_FINANCIAL_MODEL §8.1 | Accounting opinion, journal invariant and reconciliation tests | PROPOSED |
| FIN-011 | Order, Payment, Payout and Settlement lifecycles are independent | TRANSACTION_FINANCIAL_MODEL §8.2 | Cross-lifecycle state and event contract tests | PROPOSED |
| FIN-012 | Hold/custody and dispute reserves are provider-controlled unless separately approved | TRANSACTION_FINANCIAL_MODEL §8.3 | Provider contract and legal review | BLOCKED |
| FIN-013 | Chargeback loss allocation and post-payout recourse are contractually defined before live processing | TRANSACTION_FINANCIAL_MODEL §8.4 | Provider contract, customer/provider terms, legal review | BLOCKED |
| FIN-014 | Reconciliation has defined cadence, accountable owner, exception handling and immutable source reports | TRANSACTION_FINANCIAL_MODEL §8.5 | Daily reconciliation evidence, exception register, monthly close | PROPOSED |
| FIN-015 | Pilot scope is bounded to approved domestic RUB provider routes; cards/SBP remain conditional | TRANSACTION_FINANCIAL_MODEL §8.6 | Provider capability and legal/data launch gates | PROPOSED |
| ID-016 | SERVICE Accounts have accountable owners and cannot be ActingParty independently | IDENTITY_AND_ORGANIZATION_MODEL §7; SECURITY | Machine-principal ownership and attribution tests | PROPOSED |
| ID-017 | Membership, RoleAssignment and Representation grant/revoke history is append-only and event-audited | IDENTITY_AND_ORGANIZATION_MODEL §7; future Evidence & Audit model | Grant/revoke audit integrity and historical authority reconstruction tests | PROPOSED |
| ID-001 | Account authentication is distinct from Person identity | IDENTITY_AND_ORGANIZATION_MODEL §2 | Account/person linking and recovery tests | PROPOSED |
| ID-002 | Party distinguishes individual and organization participants | IDENTITY_AND_ORGANIZATION_MODEL §2–3 | Party relationship and persistence tests | PROPOSED |
| ID-003 | One Person may act as customer and provider in different workflows | IDENTITY_AND_ORGANIZATION_MODEL §3 | Multi-capacity authorization tests | PROPOSED |
| ID-004 | Organization actions require active membership and scoped permissions | IDENTITY_AND_ORGANIZATION_MODEL §2.5–2.6 | Membership/role authorization matrix tests | PROPOSED |
| ID-005 | Representation is separate from membership and time/scope bounded | IDENTITY_AND_ORGANIZATION_MODEL §2.7 | Representation proof, expiry, and revocation tests | PROPOSED |
| ID-006 | Every consequential action records actor and acting Party | DATA_MODEL §4, §9 | Audit attribution tests | PROPOSED |
| ID-007 | Identity, membership, and representation data have separate purposes, access, and retention rules | DATA_PROTECTION §4; IDENTITY_AND_ORGANIZATION_MODEL | Data inventory and retention review | PROPOSED |
| ID-008 | Party kind and OrganizationType are distinct; SOLE_PROPRIETOR and TEAM containers do not assert separate legal personality | IDENTITY_AND_ORGANIZATION_MODEL §§2–4; GLOSSARY | Party taxonomy and legal-scenario review | PROPOSED |
| ID-009 | TaxStatus is time-bound, Party-scoped, source-backed, and snapshotted for consequential transactions | IDENTITY_AND_ORGANIZATION_MODEL §§2.8, 3; DATA_MODEL | Tax-status lifecycle and transaction snapshot tests; legal review | PROPOSED |
| ID-010 | All consequential actions use normalized ActingParty and immutable event-time snapshot | IDENTITY_AND_ORGANIZATION_MODEL §3; DATA_MODEL §§4, 9 | Cross-domain event schema and audit attribution tests | PROPOSED |
| ID-011 | Person/Organization verification claims are independent and source/time scoped | IDENTITY_AND_ORGANIZATION_MODEL; GLOSSARY | Verification policy and risk-gate tests | PROPOSED |
| ID-012 | ContactPoints have independent lifecycle, verification, privacy, and retention | GLOSSARY; DATA_MODEL; DATA_PROTECTION | Contact change, verification, access, and deletion tests | PROPOSED |
| ID-013 | Service Accounts are non-human principals without a required Person link | IDENTITY_AND_ORGANIZATION_MODEL; SECURITY | Machine-principal authorization and audit tests | PROPOSED |
| ID-014 | MVP enforces one active Account per Person; account merge is out of scope | IDENTITY_AND_ORGANIZATION_MODEL §2.1, §8 | Identity uniqueness and recovery tests | PROPOSED |
| ID-015 | Lawful pseudonymization preserves minimal historical transaction attribution via tombstone/reference | IDENTITY_AND_ORGANIZATION_MODEL §7; DATA_PROTECTION | Retention/deletion and historical audit tests; legal review | PROPOSED |
| SEC-001 | Cross-tenant access is denied unless explicitly granted | SECURITY §4 | API/database/storage/search/cache/export isolation tests | PROPOSED |
| SEC-002 | Membership and representation revocation blocks new actions | SECURITY §4 | Revocation and delayed-job authorization tests | PROPOSED |
| SEC-003 | Platform staff privileges are separate and audited | SECURITY §5 | Privilege boundary and access audit tests | PROPOSED |
| SEC-004 | Account takeover, stale authority, and forged representation are mitigated | THREAT_MODEL §Threat scenarios 15–19 | Account recovery, revocation, representation, and abuse tests | PROPOSED |
| NFR-001 | Every resource enforces object-level authorization | SECURITY §2–4 | Authorization matrix tests | PROPOSED |
| NFR-002 | Sensitive data is minimized and classified | DATA_MODEL; DATA_PROTECTION | Data inventory/privacy review | PROPOSED |
| NFR-003 | Russian localization and cross-border flows reviewed | DATA_PROTECTION §3–6 | Written legal/data-flow assessment | BLOCKED |
| NFR-004 | Backup restoration is tested | SECURITY §2 | Restore drill report | PROPOSED |
| NFR-005 | Provider integrations are replaceable | ARCHITECTURE §6 | Adapter contract tests | PROPOSED |
| GOV-001 | Material changes have impact analysis | CHANGE_MANAGEMENT | PR/ADR review | ACCEPTED |
| GOV-002 | CI evidence is tied to exact commit | CHANGE_MANAGEMENT §4–5 | CI run/job/artifact references | ACCEPTED |

**Status meanings:** PROPOSED = requirement defined, implementation absent; BLOCKED = dependent legal/provider decision unresolved; ACCEPTED = governance rule approved. PASS at design review means the principle is accepted, not that a requirement is implemented or verified. Do not mark IMPLEMENTED or VERIFIED without fresh evidence.
