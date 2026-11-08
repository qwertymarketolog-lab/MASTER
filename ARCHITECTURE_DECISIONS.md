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
