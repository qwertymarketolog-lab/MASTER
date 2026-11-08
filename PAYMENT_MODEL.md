# MASTER — Payment Model v1.0

**Status:** target principles; no provider or payment flow selected.

## 1. Objectives
Make payment flows transparent, traceable, and replaceable while minimizing the platform's exposure to card data and custody of customer funds.

## 2. Preferred direction
Evaluate a payment provider with marketplace capabilities that can legally support the chosen customer-provider relationship, provider onboarding, split/commission handling where appropriate, refunds, disputes, reconciliation, and required fiscal receipts. Prefer direct provider settlement plus a separately accounted platform fee only if the provider, counsel, and actual legal model confirm it.

## 3. Prohibited assumptions
- Do not build a wallet or store card data.
- Do not call a payment hold “escrow” unless a legally appropriate regulated arrangement exists.
- Do not assume that outsourcing card processing removes MASTER's obligations.
- Do not assume the provider or platform issues every receipt; determine this from the legal/payment flow.
- Do not treat a redirect, client-side success page, or unverified webhook as proof of settlement.

## 4. Required capabilities to evaluate
Provider onboarding and status checks; payment intent; provider payout; platform fee; split settlement; partial/full refund; cancellation; chargeback/dispute handling; signed webhooks; idempotency; reconciliation reports; receipt/fiscalization support; test environment; Russian legal-entity eligibility; data location and subprocessors; service levels and support.

## 5. Payment state model
PROPOSED → PENDING_PROVIDER → AUTHORIZED/PROCESSING (only if provider supports it) → SETTLED or FAILED. Refunds and reversals are separate linked events. Preserve raw provider event reference and normalized status. Reconcile provider statements; never infer final settlement from a browser callback.

## 6. Milestone payments
For larger projects, milestone-based payment may be useful. Each milestone requires a clear deliverable, acceptance rule, amount, due condition, and cancellation/refund rule. Do not implement conditional holding/release of funds until the legal and provider model is confirmed.

## 7. Fees
Disclose the fee payer, calculation base, timing, taxes, and refund treatment before agreement. Keep fee calculation deterministic and versioned. Do not change a fee retroactively on an agreed order.

## 8. Pre-launch gate
Obtain written provider confirmation and legal review of: merchant/agent roles, fund flow, settlement, fiscal receipts under 54-FZ, NPD receipts where applicable, refunds, chargebacks, consumer rights, platform commission, and accounting records.
