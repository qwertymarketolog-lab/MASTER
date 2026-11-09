# MASTER — Architecture Decision Register

This register records material decisions. Append new entries; do not silently rewrite history. Superseded decisions remain visible and link to the replacing decision.

## ADR template
- **ID:** ADR-YYYY-NNN
- **Title:**
- **Status:** PROPOSED / ACCEPTED / SUPERSEDED / REJECTED
- **Date:**
- **Context:** problem and constraints.
- **Decision:** chosen approach.
- **Alternatives:** considered options and reasons not selected.
- **Consequences:** benefits, costs, risks, limitations.
- **Impact:** modules, data, APIs, security, legal duties, integrations, operations.
- **Migration/rollback:** required steps and recovery plan.
- **Evidence:** tests, official documentation, legal opinion, provider confirmation, or other sources.
- **Approvers:**

## Initial decisions

### ADR-2026-001 — Documentation-first foundation
- **Status:** ACCEPTED
- **Context:** premature implementation risks locking in unresolved business, legal, data, and security assumptions.
- **Decision:** approve foundation documents and resolve critical contradictions before production code.
- **Consequences:** slower first code commit; lower risk of expensive structural rework.
- **Impact:** all modules and launch gates.

### ADR-2026-002 — Modular monolith for initial product
- **Status:** ACCEPTED
- **Context:** pilot scale does not justify distributed-system complexity.
- **Decision:** begin with a modular monolith and explicit internal boundaries; extract services only with evidence and a migration plan.
- **Consequences:** simpler deployment and operations; requires discipline to preserve module boundaries.
- **Impact:** backend, database, deployment, tests, observability.

### ADR-2026-003 — Category-independent order core
- **Status:** ACCEPTED
- **Context:** home services and professional work have different deliverables and acceptance criteria.
- **Decision:** common order/identity/evidence/audit core with versioned category-specific schemas and workflow packages.
- **Consequences:** category packages require validation and compatibility rules; no category-specific assumptions in core.
- **Impact:** catalog, orders, evidence, matching, complaints, API.

### ADR-2026-004 — External payment and identity adapters
- **Status:** ACCEPTED as design direction; provider selection BLOCKED
- **Context:** avoid storing card data and provider credentials; keep integrations replaceable.
- **Decision:** provider adapters; store minimal normalized references and verification results only.
- **Consequences:** dependence on provider contracts, uptime, eligibility, fees, and API availability.
- **Impact:** identity, payments, data protection, compliance, operations.

### ADR-2026-005 — Expert panel is advisory
- **Status:** ACCEPTED as product boundary; legal effect BLOCKED pending review
- **Context:** users proposed three specialists to review service disputes.
- **Decision:** design an independent three-expert opinion workflow, with recusal and INCONCLUSIVE; do not represent it as a state court.
- **Consequences:** needs expert governance and legal review before launch.
- **Impact:** complaints, evidence, access control, provider contracts, customer terms.

### ADR-2026-006 — Russian privacy and localization review before launch
- **Status:** ACCEPTED
- **Context:** initial market is Russia and the platform will likely process personal data.
- **Decision:** minimize data and obtain a documented legal/data-flow assessment, including localization, before production processing.
- **Consequences:** hosting/vendor selection constrained by legal review.
- **Impact:** data architecture, vendors, security, contracts, operations.

### ADR-2026-007 — Architecture changes require impact analysis
- **Status:** ACCEPTED
- **Context:** undocumented changes can break dependent modules or legal/security assumptions.
- **Decision:** apply CHANGE_MANAGEMENT.md to material changes and record the decision here.
- **Consequences:** additional review effort; traceable evolution.
- **Impact:** every material change.

### ADR-2026-008 — Scale-resilient platform foundation
- **Status:** ACCEPTED
- **Date:** 2026-11-08
- **Context:** MASTER must grow beyond the initial Lesnoy pilot, adding service categories, professional domains, project teams, regions, users, and external providers without repeated rewrites of its shared core.
- **Decision:**
  1. Preserve a stable, category-independent domain core for identity and access, requests/offers, agreements and versioned changes, orders, evidence, audit history, trust, complaints, and payment references.
  2. Add service categories and professional domains through versioned category packages: schemas, eligibility rules, workflows, deliverables, acceptance criteria, evidence requirements, and risk controls.
  3. Integrate identity, payments, messaging, storage, verification, maps, and other external capabilities through replaceable adapters and explicit contracts.
  4. Keep the initial deployment a modular monolith with enforced module boundaries; extract services only when measured scale, isolation, team ownership, or operational evidence justifies the added complexity.
  5. Treat data integrity, privacy, security, observability, backup/restore, API compatibility, and operational ownership as foundation requirements, not later add-ons.
  6. Keep MASTER, JAMP, VAULT, and ЦЕХ as independent projects with explicitly governed integrations; no shared-kernel or repository coupling is assumed.
  7. Require impact analysis for material changes across modules, data, APIs, security, legal duties, integrations, operations, compatibility, migration, and rollback.
- **Alternatives:** Build a narrowly scoped single-category application; start with microservices; bind the core directly to one provider; defer security/privacy/operations until after product-market fit. Rejected because they increase lock-in, complexity, or structural rework risk.
- **Consequences:** More design and review before implementation; stronger extension boundaries and lower expected cost of adding domains. This decision does not claim infinite scalability or prove that any implementation meets a particular load target.
- **Impact:** All product modules, data model, API contracts, integration adapters, security and privacy controls, legal review, deployment, operations, testing, and roadmap.
- **Migration/rollback:** No runtime migration is authorized by this design decision. Future implementation changes must follow CHANGE_MANAGEMENT.md, include measured capacity evidence where relevant, and provide migration/rollback plans.
- **Evidence:** User-approved platform principles recorded during the foundation discussion on 2026-11-08. Implementation, legal compliance, and load capacity remain unverified.
- **Approvers:** Product owner (user); technical/legal approvals remain required for their respective areas.

### ADR-2026-009 — Separate Account, Person, Party, Organization, Membership, and Representation
- **Status:** ACCEPTED AS DIRECTION (design only; not implementation approval)
- **Date:** 2026-11-08
- **Context:** MASTER must support people acting in multiple capacities, individual providers, legal entities with multiple human operators, scoped project teams, and delegated authority without conflating login identity, domain participation, and legal authority.
- **Decision proposal:**
  1. Model Account as an authentication principal and Person as the human identity linked through controlled identity resolution.
  2. Model Party as the participant in domain relationships, with INDIVIDUAL and ORGANIZATION kinds. Organization is a non-human Party represented by people.
  3. Model Membership as a Person-to-Organization relationship with explicit status, validity, and scope.
  4. Model Role, Permission, and scoped Role Assignment separately. Authorization combines RBAC, relationship checks, and ABAC; deny by default.
  5. Model Representation separately from Membership as time-bound, action-scoped authority to act for a Party. Membership alone does not prove legal signing authority.
  6. Keep customer/provider as contextual capabilities, not mutually exclusive Account types. Keep identity assurance, business/tax status, qualification, and authority as separate verification dimensions.
  7. Derive tenant scope from resource-owning Party and server-side relationships. Require explicit cross-Party grants and enforce equivalent isolation across database, storage, search, caches, exports, and background jobs.
  8. Keep Organization onboarding and delegated actions out of the pilot UI until legal, security, and operational gates pass, while preserving the conceptual extension points.
  9. Keep Party kind as INDIVIDUAL or ORGANIZATION and add OrganizationType (LEGAL_ENTITY, SOLE_PROPRIETOR, TEAM, OTHER). Organization(SOLE_PROPRIETOR) and Organization(TEAM) are platform operating containers only; they do not create legal personality, employer status, or a single contracting party by themselves.
  10. Attach time-bound, source-backed TaxStatus to the relevant Party; evaluate it in transaction context and preserve the event-time claim snapshot. TaxStatus does not by itself decide contracting party, receipt issuer, tax agent, or merchant of record.
  11. Normalize consequential action attribution as ActingParty = (Party, optional Representation), with an immutable ActingPartySnapshot including actor, Party, authority/role references, relevant claim versions, and timestamp.
  12. Add claim-specific verification profiles for Person and Organization; independently manage ContactPoints; support non-human Service Accounts without a Person; preserve historical attribution through lawful pseudonymization/tombstone; enforce one active Account per Person in MVP and defer account merging.
- **Alternatives:** One global User record with role flags; Organization as a shared login; treating Membership as signing authority; a single tenant_id trusted from clients. Rejected because these collapse distinct trust boundaries and create authorization and audit risks.
- **Consequences:** More explicit relationships and authorization logic; clearer audit attribution and safer multi-organization/project growth. The model does not itself establish legal identity or authority and does not authorize production organization onboarding. Organization(SOLE_PROPRIETOR) and Organization(TEAM) are software collaboration/business containers, not legal conclusions. TaxStatus and verification claims must be checked per transaction risk.
- **Impact:** Identity & Access, Parties & Organizations, profiles, requests, offers, agreements, orders, projects, evidence, complaints, security, data protection, APIs, audit, and support operations.
- **Migration/rollback:** No runtime migration is authorized. Before schema freeze, complete identity assurance, account linking, permission matrix, tenant isolation, representation proof, retention, and legal review. Any implementation migration requires forward/backward compatibility and tested rollback.
- **Evidence:** User's explicit decision on 2026-11-08: principles 1–8 confirmed; amendments A/B/C and supporting controls accepted as architectural direction. Supporting design recorded in IDENTITY_AND_ORGANIZATION_MODEL.md, GLOSSARY.md, DATA_MODEL.md, and REQUIREMENTS_TRACEABILITY.md. No code, migration, runtime test, legal opinion, or security verification is implied.
- **Approvers:** Product owner direction accepted on 2026-11-08. Technical, security, privacy, and legal approvals remain required in their domains.


### ADR-2026-010 — Transaction and financial lifecycle separation
- **Status:** PROPOSED
- **Date:** 2026-11-08
- **Context:** MASTER must distinguish agreed service obligations from payment initiation, provider-confirmed payment events, refunds, provider payouts, settlement, and internal accounting. Party taxonomy and TaxStatus affect possible flows, but do not by themselves determine legal counterparties or fiscal duties.
- **Decision proposal:**
  1. Model PaymentIntent, Payment, Transaction, Refund, Payout, Settlement, and Ledger as separate financial concepts with independent state histories.
  2. Use a PartyType × OrganizationType × TaxStatus × PaymentFlow matrix as a scenario-routing constraint, not as a legal conclusion.
  3. Keep the legal contracting party, payer, beneficiary, merchant of record, receipt issuer, tax agent, and platform role as separately resolved scenario attributes.
  4. Use provider-confirmed evidence and reconciliation for terminal financial states; client callbacks and unverified webhooks are non-authoritative.
  5. Maintain an append-only double-entry ledger for MASTER's accounting/reconciliation view; it is not a wallet and does not authorize custody of funds.
  6. Treat UNKNOWN, stale, mismatched, or inconclusive financial/legal evidence as HOLD.
  7. Do not enable live payment flows until legal counsel, provider, accounting, fiscalization, security, and reconciliation gates pass.
- **Alternatives:** One generic Payment status on Order; treating a successful redirect as payment; using an internal wallet/escrow abstraction; routing payments solely from profile tax labels. Rejected because they collapse distinct facts and may create legal, accounting, or custody risks.
- **Consequences:** More explicit domain objects and reconciliation work; clearer auditability, provider portability, refund/payout separation, and safer support for team-based work. No provider or legal arrangement is selected by this proposal.
- **Impact:** Identity/Party/TaxStatus, Agreements, Orders, Payments, Accounting, provider adapters, data protection, security, support, refunds, complaints, APIs, and operations.
- **Migration/rollback:** No runtime migration. Before schema freeze, approve scenario legal roles, state transition contracts, ledger posting rules, idempotency/reconciliation design, and migration/rollback procedures.
- **Evidence:** Proposed model in TRANSACTION_FINANCIAL_MODEL.md; current PAYMENT_MODEL.md and BUSINESS_AND_LEGAL_MODEL.md; user-approved ADR-009 direction. No legal opinion, provider confirmation, implementation, or test evidence is implied.
- **Approvers:** Product owner review pending; legal, accounting, provider, technical, and security approvals required in their domains.


### ADR-2026-011 — Merchant of Record and customer-facing supplier model
- **Status:** PROPOSED — DECISION BLOCKED; no MVP model selected.
- **Date:** 2026-11-08
- **Context:** Financial architecture depends on who contracts with the customer, supplies the service, receives consideration, issues fiscal documents, owes refunds and bears consumer obligations. “Merchant of Record” is a commercial/technical label, not a substitute for Russian-law classification.
- **Options:** (A) Platform-as-MoR: MASTER is actual supplier and providers are lawful subcontractors; (B) Executor-as-contracting-supplier: customer contracts directly with executor and MASTER supplies platform services under separate terms; (C) Hybrid: explicit scenario-specific supplier and payment routes.
- **Current hypothesis, not decision:** Option B is the first model to investigate because it best matches the intended intermediary business. It is not approved for MVP or live money movement.
- **Legal caution:** Do not equate Option B with MASTER being a statutory payment agent. 103-FZ defines payment-agent activity around cash acceptance from individuals and subsequent settlements; card/SBP acquiring and marketplace split flows need separate analysis. 115-FZ applicability must be assessed against actual regulated activity, not inferred from the word “agent.”
- **Decision criteria:** supplier identity; consumer disclosures/liability; 54-FZ/KKT and NPD receipts; card/SBP fund flow/control; 161-FZ and provider product; 103-FZ and 115-FZ applicability; tax/accounting gross-vs-net and fee recognition; refunds/chargebacks; provider onboarding; data localization; 289-FZ obligations; operational cost and auditability.
- **Consequences while unresolved:** no live PaymentIntent creation or money movement; provider adapters remain abstract; no production ledger posting rule; unknown scenario routes HOLD.
- **Required evidence to accept:** written Russian legal opinion based on actual contracts and end-to-end funds flow; written provider confirmation; accountant-approved chart/posting and tax treatment; KKT/receipt responsibility matrix; approved customer/executor disclosures and refund/chargeback rules.
- **Impact:** BUSINESS_AND_LEGAL_MODEL, PAYMENT_MODEL, TRANSACTION_FINANCIAL_MODEL, DATA_MODEL, tax/receipt workflows, Order/Agreement, provider adapters, support, accounting and compliance.
- **Approvers:** Product owner decision pending; legal, accounting, provider, compliance and security approvals required in their domains.
- **Rollback:** If no lawful and operationally viable option is confirmed, disable online payments for pilot and use only a separately reviewed non-platform settlement path. Do not improvise custody or agency.


### ADR-2026-012 — Evidence-led business validation layer
- **Status:** PROPOSED — no launch or operating-model approval.
- **Date:** 2026-11-09
- **Context:** MASTER needs to validate customer problems, provider supply, alternatives, and unit economics before investing in a public marketplace or live transaction flow. External founder skills can offer methods but include simulated buyer outputs that must not be confused with market evidence.
- **Decision proposal:**
  1. Add a documentation-only validation layer with four workstreams: board-style business critique, source-backed competitor/substitute research, real customer/provider discovery, and traceable unit economics.
  2. Preserve explicit evidence classes OBSERVED, CUSTOMER_REPORTED, PUBLIC_SOURCE, ESTIMATE, SIMULATED, LEGAL_REVIEW_REQUIRED, and UNKNOWN/INCONCLUSIVE.
  3. Treat simulated buyers as hypothesis-generation only; never as customers, demand proof, testimonials, or conversion forecasts.
  4. Investigate a narrow information/referral model with direct customer-provider contracting and direct settlement as a hypothesis only; do not assume this excludes platform, advertising, consumer, privacy, agency, tax, or other duties.
  5. Exclude platform collection/holding of customer funds, escrow, payment split, payment initiation, undisclosed representation, and performance guarantees from the initial validation stage.
  6. Compare listing/subscription, qualified-lead, success-based referral, customer service, and transaction-commission models without selecting one until legal/accounting review of actual conduct and documents.
  7. Require real interviews, source-backed research, traceable economics, documented data handling, written professional review, and explicit stop gates before a pilot can be proposed.
- **Alternatives:** Install and run all external skills as platform dependencies; rely on a 100-agent simulated panel; start with transaction commissions and split payments. Rejected because they add unnecessary dependencies and can turn simulated or unresolved assumptions into apparent evidence or premature legal/financial exposure.
- **Consequences:** Slower but more defensible validation; no runtime behavior, data schema, payment flow, or legal status is changed by this proposal. The information/referral hypothesis may still create legal duties and requires counsel review.
- **Impact:** Business model, customer/provider journeys, marketing disclosures, privacy/data inventory, provider onboarding, complaint handling, fee model, accounting, roadmap, and future requirements traceability.
- **Migration/rollback:** Documentation-only. Remove or supersede the proposal if research or legal review rejects it; do not implement product behavior from this ADR alone.
- **Evidence:** Founder Skill README and four selected skill instructions reviewed 2026-11-09; MASTER BUSINESS_AND_LEGAL_MODEL.md, PAYMENT_MODEL.md, DATA_PROTECTION.md, ROADMAP.md, and CHANGE_MANAGEMENT.md. External panel is explicitly described by its own README as simulated, not a forecast. No customer interviews, legal opinion, provider confirmation, or financial validation has yet been obtained.
- **Approvers:** Product-owner review pending; qualified legal, accounting, privacy, and operational review required before any public pilot.


#### ADR-2026-012 — Amendment note (2026-11-09)
The validation proposal was expanded without changing its PROPOSED status:
- mandatory claim-level chain of evidence and append-only correction history;
- recruitment-channel logging, referral-bias controls, and explicit small-city/geographic limitations;
- predeclared kill criteria and category/offer/channel-specific stop rules;
- parallel category comparison, with minor household repair retained only as a control category;
- explicit review of marketing claims and landing-page notices, 152-FZ lawful-basis/data-minimization questions, and a mandatory new professional review before any real transaction, including a RUB 1 test payment.

These additions are methodology and governance requirements, not evidence that the relevant legal requirements have been satisfied. No personal-data collection, public pilot, payment, or production implementation is authorized.
