# MASTER — Business Validation Layer v0.1

**Status:** PROPOSED — research and pilot-design document only. Not an approved operating model, legal opinion, demand forecast, or implementation specification.

## 1. Purpose and evidence rules

Validate whether customers and independent providers in Lesnoy will use MASTER, what problem they will pay to solve, and which operating model can be tested with the least legal and financial exposure.

The layer organizes hypotheses, public research, interview notes, quotes, calculations, and decisions. Every claim must retain its evidence class:

- OBSERVED — directly recorded fact with source, date, scope, and provenance.
- CUSTOMER_REPORTED — statement from an interview/survey participant; not independently verified unless separately evidenced.
- PUBLIC_SOURCE — public information, linked and dated.
- ESTIMATE — calculation or assumption with explicit inputs and uncertainty.
- SIMULATED — LLM/persona output; never customer evidence.
- LEGAL_REVIEW_REQUIRED — legal question not resolved.
- UNKNOWN / INCONCLUSIVE — insufficient or conflicting evidence; decision remains HOLD.

No simulated buyer, generated competitor, generated review, or unverified market-size estimate may be presented as a real customer, market fact, testimonial, or verified outcome.

## 2. Four workstreams

### 2.1 Board review — business-model pressure test

Identify fatal assumptions, alternatives, and conditions for a low-cost pilot. Output: validation/board.md with strengths, top risks, unanswered questions, competing hypotheses, and a conditional decision. Analytical frameworks may be used as lenses; do not impersonate named people or imply endorsement.

### 2.2 Competitor and substitute research

Map how a customer currently finds a tradesperson: personal referrals, classifieds, aggregators, local groups, direct search, repeat contractors, or doing the work themselves.

Every named competitor, price, feature, review theme, and market claim needs a public source, access date, and scope. Read public pages; do not scrape against site terms. Distinguish direct competitors, indirect alternatives, and substitutes. Absence of evidence is not evidence that no competitor exists.

Output: validation/competitors.md and a source register.

### 2.3 Customer discovery — real people first

Start with a small qualitative round (suggested initial target: 10 customers and 10 providers, subject to recruitment feasibility). This is discovery, not a representative survey.

Record consent, date, participant role, question set, anonymized notes, and whether a statement is a direct quote or interviewer summary. Do not collect passport data, payment credentials, unnecessary precise addresses, or sensitive personal details.

Ask about past behavior and completed decisions, not only hypothetical willingness to buy. Seek disconfirming evidence and include people who decline. LLM-generated personas or simulated panels may generate interview questions and possible objections only. They cannot establish conversion, demand, or market size.

Outputs: interview protocol, anonymized evidence log, objection themes, contradictory observations, and unverified hypotheses.

### 2.4 Unit economics — scenario model, not forecast

Calculate the economics of one qualified introduction and one completed service transaction using traceable inputs. Include:
- customer price and provider service revenue as separate values;
- proposed platform revenue mechanism and fee payer;
- acquisition cost by channel;
- support and dispute-handling time/cost;
- payment-provider fees only if a future reviewed flow uses them;
- cancellations, failed matches, refunds, and unresolved tax/accounting inputs;
- contribution per lead and per completed order, fixed costs, and break-even volume.

Each input must be tagged OBSERVED, PUBLIC_SOURCE, or ESTIMATE, with source/date. Never label gross transaction value as MASTER revenue. A spreadsheet is not a tax or accounting opinion.

Output: validation/unit-economics.md and machine-readable inputs with source references.

## 3. Low-exposure pilot hypothesis

**Initial hypothesis to investigate, not an approved legal conclusion:** a narrow information/referral service helps a customer find an independent provider; the customer and provider independently decide whether to contract and settle directly.

For the first validation stage:
- no customer funds pass through MASTER;
- no wallet, escrow, payment holding, split settlement, or payment initiation;
- MASTER does not sign the service contract for either party;
- MASTER does not negotiate or accept terms as an undisclosed representative;
- MASTER does not promise provider performance, licensing, solvency, or a guaranteed result;
- provider identity/status claims are shown only with a defined source, date, scope, and limitations;
- no public rating or VERIFIED badge without a documented evidence standard;
- communications and personal-data processing are minimized and documented.

This hypothesis does not establish that MASTER is outside Russian platform-economy, advertising, consumer-protection, personal-data, agency, tax, or other regulation. Counsel must assess actual functionality, contracts, UI wording, data flows, and compensation before a public pilot.

## 4. Revenue models to compare — none selected

| Model | Commercial logic | Main questions / risk gates |
|---|---|---|
| Fixed listing/subscription fee | Provider pays for a listing or access period | Advertising/disclosure rules; supplied service; refund terms; tax/accounting |
| Fixed fee per qualified lead | Provider pays for a defined introduction | Definition of qualified; duplicate/invalid lead disputes; personal-data basis; actual agency/platform duties |
| Success-based referral fee | Provider pays after a completed deal | Proof of completion; fee disclosure; agency/commission characterization; tax/accounting; 289-FZ applicability |
| Customer service fee | Customer pays for a separately described information/coordination service | Consumer contract, scope, refunds, tax treatment |
| Transaction commission / payment split | Fee deducted from transaction | NOT IN INITIAL PILOT; requires full legal, provider, fiscalization, accounting, security, and funds-flow approval |

No model is selected. A success fee or lead fee is not automatically legally safer merely because MASTER does not receive the customer's service payment.

## 5. Pilot sequence and stop gates

1. Desk research: map existing alternatives and public provider offers in the exact pilot geography; preserve source links and dates.
2. Interviews: conduct customer and provider discovery; record refusals and contradictory evidence.
3. Manual concierge test: only after legal review of communications, data processing, disclosures, and compensation. Keep matching manual; do not represent the test as a launched marketplace.
4. Economics: calculate scenarios from observed acquisition/support effort and actual provider/customer quotes. Separate estimates.
5. Decision review: proceed only if evidence supports a real problem, reachable supply, repeatable acquisition, viable unit economics, and an approved legal/operational model.

STOP / HOLD if legal roles, privacy basis, provider eligibility, consumer disclosures, compensation terms, or complaint responsibility are unclear. Do not accept money, take deposits, hold funds, guarantee work, or represent a provider until relevant written approvals exist.

## 6. Acceptance criteria for a pilot proposal

A proposal may be marked READY FOR REVIEW only with:
- bounded service category and geography;
- real interview evidence with provenance and limitations;
- source-backed competitor/substitute research;
- provider recruitment and eligibility procedure;
- customer/provider journeys and responsibility boundaries;
- explicit revenue model and fee disclosure;
- unit economics with traceable inputs and sensitivity cases;
- data inventory, purpose, lawful-basis questions, retention, and access controls;
- complaint, cancellation, and incident handling;
- written legal review of actual contracts, UI, compensation, data flows, and pilot conduct;
- explicit stop conditions and an owner for every operational task.

READY FOR REVIEW is not permission to launch. Launch approval is a separate decision.

## 7. External founder-skill adaptation

The Founder Skill repository is an optional source of methods, not a MASTER dependency. Adapt only board critique, public competitor research, customer discovery, and unit economics.

Before executing any external skill or script, inspect its complete instructions and code, dependencies, file/network access, data handling, and license. Do not provide it secrets, private customer data, or repository write access. Do not execute external scripts in the MASTER repository as part of this proposal.

Reviewed on 2026-11-09: Founder Skill README and the four selected SKILL.md files. Its README explicitly states that the consumer panel is simulated and is not a forecast.

## 8. Non-goals and status

This layer does not implement the marketplace, choose a payment provider, approve a legal structure, determine tax status, establish provider qualifications, certify demand, authorize live money movement, or change the MASTER core architecture.

- Legal, tax, accounting, provider, demand, and operational conclusions: NOT VERIFIED.
- Implementation: DOCUMENT ONLY — NOT IMPLEMENTED.
- Approval: PROPOSED — product-owner and professional review required.
