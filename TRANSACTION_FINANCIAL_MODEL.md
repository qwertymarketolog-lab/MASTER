# MASTER — Transaction & Financial Model v0.1

**Status:** PROPOSED conceptual design for review. No payment provider, legal role, fund flow, or production financial operation is approved by this document.

## 1. Purpose and boundaries

Define a transaction model that can support individual providers, sole proprietors, legal entities, and informal collaboration teams without assuming that a platform Party is always the legal counterparty or payment beneficiary.

The model separates:
- domain agreement and Order lifecycle;
- PaymentIntent (request to initiate a payment);
- Payment (provider-reported attempt/result);
- Transaction (immutable record of a financial event or provider movement);
- Refund;
- Payout (instruction to pay a provider/beneficiary);
- Settlement (provider-confirmed allocation/transfer and reconciliation);
- Ledger (MASTER's auditable accounting representation, not a customer wallet).

MASTER must not take custody of customer funds or represent itself as an escrow provider unless a separately approved legal and regulated arrangement explicitly permits it.

## 2. PartyType × TaxStatus × PaymentFlow matrix

This is a routing/design matrix, not a final legal classification. TaxStatus is time-bound and source-backed. The actual contracting party, payment recipient, receipt issuer, tax agent, merchant of record, and refund obligor must be resolved per legal scenario before a live flow is enabled.

| Platform Party / OrganizationType | Relevant TaxStatus | Candidate contract/payment arrangement | Required controls and unresolved gates |
|---|---|---|---|
| Individual Party | INDIVIDUAL (ordinary natural person) | Direct customer-to-provider payment only if the provider is legally eligible and the service/contract structure is approved. | Confirm provider eligibility, contract parties, consumer disclosures, receipt/fiscalization duties, and provider onboarding. |
| Individual Party | SELF_EMPLOYED_NPD | Candidate direct customer-to-provider flow through a provider supporting the applicable NPD workflow; commission/split only if legally and technically confirmed. | Verify current NPD status at defined points; capture provider-issued NPD receipt evidence where applicable; define cancellation/refund and correction handling. |
| Individual Party | SOLE_PROPRIETOR | Candidate provider settlement to the verified business bank account through a marketplace-capable provider, or another counsel-approved route. | Verify registration/status, account ownership, activity restrictions, receipt/54-FZ responsibility, VAT/tax treatment where applicable, and contract role. |
| Organization(LEGAL_ENTITY) | LEGAL_ENTITY / applicable tax regime | Candidate organization onboarding and provider settlement to its verified account; platform fee only under approved agency/marketplace arrangement. | Verify legal entity, representative authority, bank-account ownership, receipt issuer, merchant/agent role, tax treatment, and provider eligibility. |
| Organization(SOLE_PROPRIETOR) | SOLE_PROPRIETOR / applicable regime | Organization is a platform business workspace; payment must route to the legally identified individual entrepreneur/business beneficiary, not to a fictional separate legal person. | Resolve who is the contracting party and account holder; validate representative authority and tax status. No automatic corporate split. |
| Organization(TEAM) with independent members | Each member retains own TaxStatus (e.g. NPD, IP, individual) | No pooled TEAM payout by default. Model member-level agreements/obligations and separate provider payment/payout legs, or appoint one legally responsible lead provider only after explicit legal review. | TEAM is not automatically a legal entity, partnership, employer, agent, or contracting party. Define who contracts, who performs, who invoices/issues receipts, liability, IP, refunds, and member allocation. |
| Organization(TEAM) with one lead provider | Lead provider's own verified TaxStatus | Possible direct customer-to-lead-provider flow; lead provider separately settles with subcontractors only under their lawful contracts and tax/receipt obligations. | MASTER must not infer subcontracting authority or employment status. Validate lead provider's right to engage others and responsibility for delivery. |

### Matrix invariants

1. Party kind and OrganizationType describe platform participation/operating container; they do not alone identify the legal counterparty.
2. TaxStatus is checked for the relevant Party and time, then snapshotted into the accepted Agreement/PaymentIntent and financial events.
3. A mismatch, expired check, unknown status, or unresolved legal scenario blocks payment initiation or payout (HOLD).
4. A TEAM container cannot be used as a pooled beneficiary by default.
5. Payment provider capability does not prove that the legal structure is lawful.
6. Payment routing must not be inferred from a profile label, client-supplied bank details, or an AI classification.

## 3. Core financial objects

### 3.1 PaymentIntent

A versioned instruction to attempt a payment for a defined Order/agreement/milestone:
- payer Party and immutable payer ActingPartySnapshot;
- intended legal payee/beneficiary Party (resolved independently of the platform Organization container);
- Order, AgreementSnapshot, milestone, currency, gross amount, fee basis/version, and due condition;
- selected provider adapter and provider-side intent reference;
- idempotency key, expiry, risk/verification requirements, and state history.

An intent is not proof that money was received or settled.

### 3.2 Payment

A provider-reported payment attempt/result linked to one PaymentIntent. Preserve raw provider event references and normalized status separately. Browser redirects and client success pages are never authoritative.

### 3.3 Transaction

An immutable normalized financial event: charge, capture, refund, reversal, chargeback, fee, payout movement, settlement movement, or correction. It records provider, provider event/reference, amount, currency, effective/observed timestamps, related Order/Intent, payer/payee where known, source evidence, idempotency/deduplication key, and reconciliation state.

Corrections append compensating events; they do not rewrite a historical transaction.

### 3.4 Ledger

An append-only double-entry journal for MASTER's own accounting and reconciliation view. Each posted journal entry balances debits and credits in the same currency (or uses explicit FX legs). Ledger entries are created only from authoritative provider/bank/accounting evidence and approved accounting rules. The ledger is not a wallet, does not create custody rights, and does not replace statutory accounting or provider statements.

### 3.5 Refund

A separately tracked instruction and provider-confirmed refund movement linked to the original Payment/Transaction. Partial refunds are first-class and cumulative refunds cannot exceed the refundable amount after accounting for prior refunds/reversals and provider rules.

### 3.6 Payout

An instruction to transfer funds to a verified eligible beneficiary under the approved provider/legal arrangement. A payout is not complete when requested; only provider/bank evidence can establish final state.

### 3.7 Settlement

A provider-reported allocation or transfer of funds to the intended beneficiary and/or platform fee recipient. Settlement is distinct from payment authorization, capture, payout instruction, and internal ledger posting. Reconciliation matches expected and actual amounts, fees, dates, currencies, and references.

## 4. State machines

All state transitions are append-only, timestamped, actor-attributed, idempotent, and driven by authoritative evidence. Exact provider statuses are mapped by a versioned adapter contract. Unknown provider states remain UNKNOWN and block downstream completion.

### 4.1 PaymentIntent

DRAFT → REQUIRES_ACTION → PROCESSING → SUCCEEDED

Alternative transitions:
- DRAFT → CANCELED | EXPIRED
- REQUIRES_ACTION → PROCESSING | CANCELED | EXPIRED | FAILED
- PROCESSING → SUCCEEDED | FAILED | UNKNOWN
- UNKNOWN → SUCCEEDED | FAILED only after authoritative reconciliation evidence.

SUCCEEDED means the defined provider payment step succeeded; it does not necessarily mean beneficiary settlement is complete.

### 4.2 Payment

CREATED → PENDING_PROVIDER → PROCESSING → AUTHORIZED (optional) → CAPTURED

Alternative terminal/exception states:
- PENDING_PROVIDER | PROCESSING | AUTHORIZED → FAILED | CANCELED | EXPIRED
- CAPTURED → PARTIALLY_REFUNDED | REFUNDED | CHARGEBACK_OPEN
- CHARGEBACK_OPEN → CHARGEBACK_WON | CHARGEBACK_LOST | CHARGEBACK_PARTIAL

Only include AUTHORIZED or CAPTURED when the selected provider exposes those distinct stages. A single-step provider may map directly to its authoritative success state. Payment state does not imply payout or settlement state.

### 4.3 Refund

REQUESTED → PENDING_PROVIDER → SUCCEEDED

Alternative transitions:
- REQUESTED → REJECTED | CANCELED
- PENDING_PROVIDER → FAILED | UNKNOWN
- UNKNOWN → SUCCEEDED | FAILED after reconciliation.

A refund request is not a completed refund. Preserve original payment reference, requested/approved amount, reason, initiator, legal basis/policy, provider evidence, and cumulative refunded amount. Do not refund more than the refundable balance.

### 4.4 Payout

DRAFT → ELIGIBILITY_CHECK → SUBMITTED → PROCESSING → PAID

Alternative transitions:
- ELIGIBILITY_CHECK → HELD | REJECTED
- SUBMITTED | PROCESSING → FAILED | RETURNED | UNKNOWN
- HELD → ELIGIBILITY_CHECK after the blocking condition is resolved and re-authorized.

PAID requires authoritative provider/bank confirmation. A beneficiary change after submission requires cancellation/reversal where possible and a new authorized payout instruction; never silently mutate the destination.

### 4.5 Settlement

EXPECTED → PROVIDER_REPORTED → RECONCILING → RECONCILED

Alternative transitions:
- PROVIDER_REPORTED | RECONCILING → MISMATCH | UNKNOWN
- MISMATCH → RECONCILING after evidence/correction.
- RECONCILED → REVERSED only through a new authoritative reversal/chargeback event; preserve the original reconciliation record.

A settlement may be split among provider, platform fee, tax/withholding, or other permitted recipients only where the provider and approved legal model support each leg.

## 5. Financial invariants

1. Order completion, Payment success, Payout completion, Settlement reconciliation, and Refund completion are separate facts and states.
2. No browser callback, client screenshot, unverified webhook, or AI summary can create a terminal financial state.
3. Webhooks require signature verification, timestamp/replay protection where supported, idempotent processing, raw event retention, and adapter-version tracking.
4. Provider statements/reports and bank evidence are reconciled; duplicate, missing, out-of-order, or conflicting events remain visible and do not silently overwrite one another.
5. Every amount uses integer minor units and an explicit ISO currency; rounding and FX rules are versioned.
6. Fee calculations are deterministic, versioned, disclosed before agreement, and not retroactively changed for an accepted transaction.
7. Ledger journal entries balance; corrections use compensating entries.
8. A payment/refund/payout/settlement event retains the relevant immutable ActingPartySnapshot and TaxStatus/verification claim versions.
9. Beneficiary eligibility, bank-account verification, tax status, representation, and legal payment route are checked at the required risk points.
10. UNKNOWN, INCONCLUSIVE, mismatched or stale evidence results in HOLD; do not synthesize PASS.

## 6. Required legal and provider decision record

Before enabling any live flow, create a scenario-specific decision record containing:
- service category and customer/provider relationship;
- contracting parties and agreement issuer;
- platform legal role (if any) and exact obligations;
- payer, beneficiary, merchant of record, payment recipient, fee recipient;
- payment provider and supported product/contract;
- receipt issuer and applicable 54-FZ/NPD receipt duties;
- tax agent/withholding/VAT treatment where applicable;
- refund, cancellation, warranty, chargeback, and complaint responsibility;
- payout/settlement mechanics, provider evidence, reconciliation source;
- personal-data roles, data location, subprocessors, retention;
- legal opinion and provider confirmation references, date, scope, and expiry/review trigger.

## 7. Open decisions and blockers

- Legal role of MASTER per scenario (agent, information intermediary, marketplace operator, other).
- Merchant of record and receipt issuer for each flow.
- Whether split payments/commission are supported and lawful for each provider/tax-status combination.
- Provider eligibility and onboarding requirements for NPD, IP, legal entities, and multi-member teams.
- Whether any milestone flow is permitted without creating custody/escrow implications.
- Accounting chart, recognition timing, tax treatment, FX, and statutory reporting.
- Refund/chargeback allocation and negative-balance handling.
- Beneficiary bank-account verification and payout hold/review policy.
- Data localization, provider subprocessors, and cross-border transfer assessment.

**Gate:** no production payment, payout, settlement, or live money movement until the scenario-specific legal matrix, provider confirmation, accounting design, fiscalization responsibilities, security review, and end-to-end reconciliation tests are approved.
