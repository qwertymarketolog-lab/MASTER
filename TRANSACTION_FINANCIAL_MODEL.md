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

This is a constrained design matrix, not a legal conclusion. Platform Party, contracting party, service provider, consumer-facing supplier, payee/beneficiary, receipt issuer, payment-service provider and any agent are separate roles. “Merchant of Record” (MoR) is a commercial/technical shorthand; it does not replace Russian-law analysis of actual contracts, funds flow, consumer-facing offer, fiscalization and regulated activity.

### 2.1 MoR / customer-facing supplier alternatives — decision OPEN

| Option | Customer-facing supplier | Payment and receipt implications | Consequences |
|---|---|---|---|
| A. Platform-as-MoR | MASTER is the actual supplier to the customer and undertakes the service obligation; providers act as lawful subcontractors. | MASTER's customer consideration, refunds, consumer obligations, KKT, revenue, subcontractor payments and taxes require a full seller model. | Highest change to business model, risk and accounting. Not assumed for a matching platform. |
| B. Executor-as-contracting-supplier | Customer contracts directly with the identified executor; MASTER supplies platform/coordination services under separate terms. | Executor's supplier and fiscal obligations must be mapped; provider-direct settlement is preferred for evaluation where lawful. MASTER's commission has its own contract and fiscal treatment. | Candidate direction, but does NOT automatically make MASTER a statutory payment agent. |
| C. Hybrid | Supplier and contractual structure vary by approved service/provider scenario. | Receipt issuers, payment routes, refund obligors and fee accounting may differ. | Flexible but complex; requires explicit scenario registry and fail-closed routing. |

**Decision:** ADR-2026-011 remains PROPOSED and selection is BLOCKED. Option B is only the first hypothesis to test, not an approved MVP model. Do not create live PaymentIntents or move money while the legal supplier/MoR model is unresolved.

**Legal caution:** Do not equate MoR with a Russian statutory role. 103-FZ defines payment-agent activity around acceptance of cash payments from individuals and subsequent settlements; it is not a synonym for every card/SBP marketplace flow. Applicability of 103-FZ, 161-FZ, 54-FZ, consumer/aggregator rules, 115-FZ and 289-FZ must be assessed separately against the real flow. A MoR label alone cannot answer these questions.

### 2.2 Provider eligibility and Party / TaxStatus matrix

| Platform Party / provider form | Relevant status | MVP design treatment | Payment route / gate |
|---|---|---|---|
| Individual Party — customer | INDIVIDUAL | Allowed as customer, subject to onboarding and consumer-data controls. | Payment only after a supplier scenario is enabled. |
| Individual Party — executor without NPD/IP | INDIVIDUAL | RESTRICTED / disabled by default pending legal and tax review. | No payment until contract type, PIT/insurance, withholding/reporting, KKT/receipt, consumer and category rules are reviewed. |
| Individual Party — executor, NPD | SELF_EMPLOYED_NPD | Candidate provider type if current NPD status, activity and statutory conditions are verified. | Provider-direct route preferred for evaluation; preserve NPD status evidence and receipt reference. Commission/receipt route needs separate approval. |
| Individual Party — sole proprietor acting alone | SOLE_PROPRIETOR | Individual Party acting in the person's IP capacity when selected as contracting profile. | Contract, payee and receipt route tied to that verified profile. |
| Organization(SOLE_PROPRIETOR) — IP workspace/team | SOLE_PROPRIETOR | Separate platform container for memberships/delegation; it does not create a separate legal person. | Resolve contract/payment to the actual IP. Freeze profile/counterparty mapping at Order acceptance. |
| Organization(LEGAL_ENTITY) | LEGAL_ENTITY / applicable regime | Candidate provider organization after entity, authority, tax and bank checks. | Pay actual legal entity only through an approved provider/legal route. |
| Organization(TEAM) — independent members, no legal entity | Each member has own NPD/IP/other status | Only TEAM pattern to evaluate for pilot: each member separately identified, contracted and paid for their own scope, if counsel approves. TEAM itself is not supplier or pooled payee. | Separate member obligations, amounts, evidence and receipts; otherwise HOLD. |
| Organization(TEAM) — one lead executor | Lead's own NPD/IP/other status | Not enabled by default in pilot. Lead may contract for whole work only after review of subcontracting, authority, tax and consumer liability. | One lead receives payment only if genuinely the contracting supplier and subcontracting is lawful/documented. |
| Organization(TEAM) — one legal entity | LEGAL_ENTITY | Model as Organization(LEGAL_ENTITY), not informal TEAM. | Payment to legal entity under its own contract, tax, payroll/subcontractor and KKT rules. |

**IP profile selection:** At provider onboarding, select the legally accurate operating profile for a new engagement: Individual Party acting as the IP, or Organization(SOLE_PROPRIETOR) workspace representing the same IP with delegated access. The selected profile and legal counterparty mapping are snapshotted when Order terms are accepted. Existing Orders never silently switch profile or contracting party.

### 2.3 Matrix invariants

1. Platform Party and OrganizationType do not alone identify supplier, seller/MoR, payee, receipt issuer or agent.
2. TaxStatus is checked for the relevant legal person and time, then snapshotted into the accepted Agreement and financial events.
3. Unknown, stale, conflicting or expired status, unclear supplier identity or unresolved legal classification blocks payment and payout (HOLD).
4. TEAM is never a pooled legal person or beneficiary by default.
5. Provider capability does not prove the underlying contract/tax model is lawful.
6. Routing is never inferred from profile labels, client bank details or AI classification.
7. Supplier and payment route are frozen per accepted Order; material change requires a new approved agreement/change event and cannot rewrite history.

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

Only include AUTHORIZED or CAPTURED when the selected provider exposes those distinct stages. A single-step provider may map directly to its authoritative success state. Payment state does not imply payout or settlement state. Refunds and chargebacks are independent linked lifecycles; do not collapse concurrent partial refunds and chargebacks into one mutable Payment status. Payment retains its original capture outcome while linked Refund and Chargeback records track later movements.

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
10. Verification is claim-specific: email/phone/identity/business/bank-account claims are not interchangeable. Amount bands and transaction-risk thresholds must map to the exact required claims and provider/legal policy; no universal numeric verification score or arbitrary threshold is assumed.
11. UNKNOWN, INCONCLUSIVE, mismatched or stale evidence results in HOLD; do not synthesize PASS.

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


## 8. Financial accounting and operating controls — PROPOSED

### 8.1 Ledger account catalogue (conceptual)

This is a design seed, not a statutory chart or a conclusion that MASTER controls funds. Recognition, account class and gross/net presentation depend on the selected legal/provider model and accounting opinion.

| Code | Account | Purpose / posting condition |
|---|---|---|
| 1000 | Provider clearing / settlement receivable | Provider-confirmed amount due to MASTER only where MASTER has a recognized accounting claim. Never post customer funds merely because UI says paid. |
| 1010 | MASTER operating bank | MASTER's own funds after actual settlement to its bank. |
| 1100 | Platform fee receivable | Contractually earned platform fee due from the party named in the approved fee agreement. |
| 2000 | Executor payable | Recognized obligation only where MASTER is the contractual obligor or accounting rules require recognition; not a proxy for third-party funds held elsewhere. |
| 2010 | Refund payable | Approved refund obligation of MASTER, if MASTER is the legal refund obligor. |
| 2020 | Chargeback / dispute payable | Recognized chargeback obligation allocated to MASTER under provider contract and legal model. |
| 2030 | Tax / withholding payable | Amount legally owed/remitted by MASTER; never assume withholding-agent status. |
| 4000 | Platform commission revenue | Fee revenue recognized under approved contract/accounting policy; gross/net presentation requires accountant review. |
| 5000 | Provider processing expense | Provider fees and transaction costs borne by MASTER. |
| 9000 | Suspense / reconciliation differences | Temporary unresolved differences with owner, ageing, evidence and deadline; never a balancing plug. |

Every journal entry references source event, Order/Agreement, counterparty, currency, tax treatment, effective/posting dates, policy version and correction link. Entries balance per currency. Raw provider statements remain separate evidence; the ledger is derived accounting evidence.

### 8.2 Order and money lifecycles are independent

| Order event/state | Payment | Payout | Meaning |
|---|---|---|---|
| ACCEPTED; work not started | Not initiated | Not initiated | Contract exists; no proof of payment. |
| Work in progress | AUTHORIZED / CAPTURED / provider-confirmed success | Not initiated | Payment step may have succeeded; settlement and work are separate. |
| Order COMPLETED | Pending, captured, refunded or disputed | Pending or not initiated | Service acceptance does not assert settlement or payout. |
| Order COMPLETED | CAPTURED | PROCESSING | Payment recorded; beneficiary has no confirmed payout yet. |
| Order COMPLETED | CAPTURED | PAID; Settlement RECONCILED | Provider/bank evidence confirms transfer and reconciliation. |
| Order DISPUTED / REFUND_REQUESTED | Any prior payment state | May be blocked, pending or already paid | Dispute/refund is independent; no automatic reversal of work or settlement. |
| Order CANCELLED | Absent, pending, captured or refunded | Absent or recovery may be needed | Cancellation does not prove refund or clawback completion. |

Invariant: Order.COMPLETED ≠ Payment.SETTLED ≠ Payout.PAID ≠ Settlement.RECONCILED. Each requires its own evidence.

### 8.3 Hold period and custody boundary

No platform-defined hold period is approved. MASTER must not claim to hold, escrow, safeguard or control customer funds in the pilot. If provider offers delayed settlement, rolling reserve or dispute reserve, the provider is the holder under its contract; duration, release conditions, ownership, insolvency treatment and dispute process must be documented before use. If no provider mechanism exists, MASTER must not simulate one with an internal balance. A dispute can block payout only where the provider contract/product permits it.

### 8.4 Chargeback and post-payout loss allocation

No universal allocation is approved. Before a route is enabled, provider and customer/executor contracts must define: merchant/provider account holder; party debited by acquirer; principal, fees, representment costs and FX loss; evidence duties/deadlines; lawful recourse against executor; set-off limits and negative-balance treatment; and recovery after payout or account closure. Do not automatically debit future earnings or create a negative wallet. Provider debit and contractual recourse govern accounting recognition.

### 8.5 Reconciliation operating policy

Pilot proposal, subject to provider capability:
- ingest signed webhooks continuously; preserve raw payload and signature-verification result;
- import provider transaction/settlement reports at least daily;
- run automated matching daily by provider reference, amount, currency, fee, date, Order and beneficiary;
- assign unmatched, duplicate, late, partial and conflicting records to a named Finance Operations owner; no silent auto-resolution;
- review exceptions each business day and escalate material/aged differences under approved thresholds;
- perform monthly close with bank/provider statement reconciliation and signed review evidence;
- store raw provider reports immutably with retrieval time, source, checksum, parser/adapter version and access audit; derived matching results are separately versioned.

A provider report is not proof of MASTER's bank receipt unless it evidences that transfer.

### 8.6 Pilot scope boundary — proposal, not launch approval

Initial technical scope to evaluate: domestic Russian transactions, RUB only, one approved provider/legal flow at a time, and payment methods explicitly supported and contractually enabled by that provider (candidate cards and SBP). Exclude foreign payers/payees, cross-border transfers, FX, crypto, cash collection, stored value, wallet, peer-to-peer balance transfer and multi-currency ledger from pilot design. These exclusions do not make every domestic card/SBP flow lawful. Every method remains disabled until legal, provider, fiscalization, data and operational gates pass.
