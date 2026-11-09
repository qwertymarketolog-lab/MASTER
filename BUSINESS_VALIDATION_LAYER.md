# MASTER — Business Validation Layer v0.1

**Status:** PROPOSED — research and pilot-design document only. Not an approved operating model, legal opinion, demand forecast, or implementation specification.

**Geographic scope limitation:** Initial research is limited to Lesnoy, Sverdlovsk Oblast. Lesnoy is a small, closed administrative-territorial formation (ZATO); recruitment access and local networks may materially bias the sample. Findings are local and exploratory and must not be extrapolated automatically to Yekaterinburg, Moscow, or any other region.

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

Record date, participant role, question set, anonymized notes, recruitment channel, and whether a statement is a direct quote or interviewer summary. Before collecting any personal data (including names, phone numbers, or addresses), document the purpose, data categories, retention, access, applicable lawful basis under 152-FZ, and any required consent. Do not assume consent is the only possible lawful basis or that informal research is exempt. Prefer anonymous interviews; keep contact details separate and collect no passport data, payment credentials, unnecessary precise addresses, or sensitive personal details.

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

## 2.5 Evidence chain and audit record — mandatory for every claim

Every evidence item must have a stable unique ID and a traceable chain from source to conclusion. A summary without its underlying evidence reference is not an auditable finding.

Minimum evidence record:
- evidence_id and evidence_class;
- claim or hypothesis being tested, including the exact interview question/research question;
- source type and source reference (participant pseudonym, public URL, document ID, or calculation input IDs);
- who collected/recorded it and when (timestamp with timezone);
- when the underlying event or statement occurred, if different;
- geography, category, participant role, recruitment channel, and relevant sample context;
- verbatim quote or faithful raw note, clearly distinguished from interviewer interpretation;
- consent status and documented lawful basis for any personal-data processing; do not store unnecessary identifiers in the evidence log;
- transformations, coding, exclusions, corrections, and the person/date responsible for each;
- links to derived findings and calculations; preserve the original record when a correction is made.

For public sources, retain URL, publisher, publication/update date where available, access timestamp, and the exact claim supported. For estimates, retain formula, input evidence IDs, assumptions, units, and sensitivity range. For AI-generated material, retain model/tool and prompt/context where practical, label it SIMULATED, and prohibit promotion to customer evidence.

Do not silently overwrite or delete source notes to make a hypothesis look stronger. Corrections must be appended with reason and author. Restrict access to interview records; store contact details separately from research notes and only when necessary and lawfully justified. If a complete chain cannot be reconstructed, downgrade the claim to UNKNOWN / INCONCLUSIVE.

## 2.6 Recruitment channels and sample-bias control

Track the channel through which each respondent was reached: personal contact, referral, local community/group, public listing, business outreach, inbound form, or other specified channel. Record whether the recruiter has a personal/business relationship with the respondent and whether any incentive was offered.

Do not recruit only through one social circle or one provider network. Use at least three distinct recruitment routes where feasible; actively seek respondents outside the founder's immediate contacts, including people who declined or chose a competing alternative. Maintain a recruitment log with invitations, responses, refusals, duplicate detection, and channel counts. Do not interpret convenience or referral samples as representative of Lesnoy.

The suggested initial discovery target of 10 customers and 10 providers is qualitative and exploratory, not a statistical sample-size claim. Report the actual denominator, recruitment channels, role/category mix, and obvious coverage gaps. Small-city network effects and repeated referrals can make respondents correlated; disclose that limitation and do not extrapolate automatically to Yekaterinburg, Moscow, or other regions.

## 2.7 Kill criteria and falsification discipline

Before each interview/research round, write down the hypothesis, what evidence would support it, what evidence would contradict it, and the threshold that triggers STOP, REVISE, or HOLD. Do not change thresholds after seeing results without recording a dated rationale and treating the revised test as a new hypothesis.

Initial falsification rules (PROPOSED thresholds to validate, not market facts):
- **Category problem:** if fewer than 3 of 10 independently recruited target customers in a category describe a recent, concrete problem with their current way of finding/commissioning the service, do not advance that category on discovery evidence alone; revise the segment/problem hypothesis.
- **Provider willingness:** if 0 of 10 independently recruited qualified providers in a category agree to a clearly described, legally reviewed next-step test under the disclosed proposed fee, stop that fee/category combination. Do not generalize the result to all categories or all providers.
- **Channel viability:** if a channel produces no qualified conversations after 20 documented, appropriately targeted invitations, pause that channel and investigate reach/message/segment before spending further.
- **Unit economics:** if contribution remains negative under the defensible base case and plausible upside sensitivity, do not proceed with paid acquisition or scale assumptions; revise price, cost, channel, or model.
- **Legal feasibility:** any unresolved material question about legal role, personal-data basis, marketing claims, consumer disclosures, or compensation means HOLD; no public pilot or transaction.
- **Trust/quality proposition:** if customers cannot identify a meaningful risk or coordination problem that MASTER can address beyond an existing free substitute, do not assume verification/coordination features create willingness to pay.

These thresholds are decision rules for a small exploratory test, not statistical proof. A failed threshold rejects or pauses the tested category/offer/channel combination, not MASTER as a whole. A positive result does not prove product-market fit or justify launch.

## 3. Category selection for pilot validation

Do not use one easy category as a proxy for the entire business model. Test 2–3 categories in parallel where recruitment and legal review permit, with different interview guides and separate results. Keep “husband for an hour” / minor household repair as a low-complexity control category, not the sole primary test.

Selection criteria:
1. Typical job value plausibly above RUB 15,000–20,000, to be verified through real local quotes rather than assumed.
2. Multi-visit or multi-stage work that creates coordination, scheduling, scope-change, or acceptance friction.
3. A meaningful need to assess qualifications, experience, permits, or specialist competence, where applicable.
4. Discovery friction: customers struggle to identify and compare suitable providers through existing channels.
5. Evidence that professional providers may pay for a qualified opportunity or coordination service; willingness must be tested, not presumed.

| Candidate category | Ticket hypothesis | Coordination value | Qualification / trust value | Discovery friction | Initial role |
|---|---|---|---|---|---|
| Apartment renovation / multi-stage work | Potentially high; verify quotes and scope | High: stages, trades, materials, changes, acceptance | High; scope and competence vary | To test; referrals may dominate | Primary candidate |
| Complex plumbing / electrical work | Variable; split urgent small jobs from major work | Medium to high for complex or multi-stage jobs | Potentially high; qualification/safety checks matter | Potentially high for uncommon specialists | Primary candidate, narrowly scoped |
| Small-business premises work (offices/shops) | Potentially medium/high; verify B2B budgets | High when scheduling, access, downtime, and multiple tasks matter | Depends on work type and procurement requirements | To test with business decision-makers | Primary candidate if reachable |
| Specialist trades (tiler, welder, stretch-ceiling installer) | Potentially medium/high; verify by job type | Medium; rises with project duration and dependencies | Potentially high | Potentially high for scarce local skills | Candidate / compare |
| Minor household repair (“husband for an hour”) | Often hypothesized lower; collect actual local quotes | Usually low for a single visit | Variable | May be low due to personal referrals | Control group, not sole test |

All entries in the matrix are initial hypotheses, not verified descriptions of the Lesnoy market. Do not assign numeric scores or rank categories until source-backed desk research and interviews exist. Record category-specific exclusions, seasonality, safety risks, licensing/qualification questions, and whether the work is legally suitable for a referral pilot.

For each selected category, compare at least: (a) recent customer problem and current workaround, (b) actual quote/contract value, (c) number of stages/visits and coordination burden, (d) qualification evidence required, (e) how the respondent currently finds providers, (f) provider willingness under a clearly disclosed fee, and (g) legal/safety stop conditions.

## 3. Low-exposure pilot hypothesis

**Initial hypothesis to investigate, not an approved legal conclusion:** a narrow information/referral service helps a customer find an independent provider; the customer and provider independently decide whether to contract and settle directly.

For the first validation stage:
- no customer funds pass through MASTER;
- no wallet, escrow, payment holding, split settlement, or payment initiation;
- MASTER does not sign the service contract for either party;
- MASTER does not negotiate or accept terms as an undisclosed representative;
- MASTER does not promise provider performance, licensing, solvency, or a guaranteed result; all advertising, landing-page, interface, and sales statements must be accurate, substantiated, and consistent with the actual service. A disclaimer in the footer does not cure a misleading headline or implied guarantee;
- provider identity/status claims are shown only with a defined source, date, scope, and limitations;
- no public rating or VERIFIED badge without a documented evidence standard;
- communications and personal-data processing are minimized and documented; even a simple public landing page or lead form requires review of its notices, privacy information, data collection, and legal basis before publication.

This hypothesis does not establish that MASTER is outside Russian platform-economy, advertising, consumer-protection, personal-data, agency, tax, or other regulation. Counsel must assess actual functionality, contracts, UI wording, advertising claims, data flows, and compensation before a public pilot. Any actual transaction or receipt of money — including a test payment of RUB 1 — is a hard trigger for a new legal, accounting, provider, fiscalization, security, and funds-flow review before the transaction is enabled, not after it occurs.

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
