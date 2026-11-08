# MASTER — Product Specification v1.0 (Foundation)

**Status:** baseline proposal for review; not a statement of implemented behavior.

## 1. Product purpose
MASTER is a digital platform that helps customers discover independent service providers, create and agree work requests, coordinate delivery, preserve a reliable history, and resolve complaints through transparent procedures. The initial pilot is planned for Lesnoy, Sverdlovsk Oblast.

## 2. Product boundaries
MASTER is initially a marketplace and coordination intermediary. It is not assumed to be the employer of every provider, the contractor performing the work, a bank, an escrow institution, a state court, a certification body, or a universal guarantor. Actual legal roles depend on real workflows, contracts, control over pricing/acceptance/payments, and applicable law; these must be reviewed before launch.

## 3. Actors
- Customer: creates a request, selects a provider, agrees scope/price/deadline, confirms or disputes completion.
- Provider: an eligible self-employed person, individual entrepreneur, or legal entity; independently decides whether to accept and performs the contracted service.
- Platform operations: supports matching, communications, status tracking, evidence capture, complaints, and account operations.
- Expert: an independent specialist invited to provide a domain-specific opinion in a dispute.
- Platform administrator/support: operates the service under least-privilege access and audited procedures.
- External provider: identity, payment, messaging, hosting, storage, monitoring, or verification partner.

## 4. Service domains
The core must not encode business rules for one trade. Initial pilot categories may include plumbing, electrical work, heating, small household repairs, and finishing. Later domains may include construction, IT/software, design, accounting, marketing, engineering, legal services, teams/projects, materials, and logistics. Each domain has its own qualification rules, workflow, deliverables, acceptance criteria, and restricted-activity checks.

## 5. Core customer journey
1. Sign in using an enabled identity method or another supported account flow.
2. Choose a category and describe the need using text, voice transcription, photos, or video where supported.
3. Provide only the location/contact data needed to fulfil the request.
4. Review clarifying questions and urgency guidance; emergency situations are directed to appropriate emergency/utility services.
5. Receive suitable provider offers or matching suggestions.
6. Compare provider profiles, verified attributes, availability, and relevant history.
7. Agree scope, price, schedule, materials, cancellation terms, and acceptance criteria.
8. Track milestones and approved changes.
9. Review completion evidence and accept, request correction, or open a complaint.
10. Leave feedback tied to a real platform order.

## 6. Provider journey
- Create an account and select service categories/coverage area.
- Complete proportionate identity, business-status, qualification, and eligibility checks appropriate to the category and law.
- Set availability and commercial terms.
- Review requests and submit an offer or decline.
- Confirm the order terms before work begins.
- Record milestones, approved changes, materials, and completion evidence.
- Receive payment under the approved provider payment flow.
- Respond to complaints and maintain current profile information.

## 7. Order lifecycle
Minimum states: DRAFT → PUBLISHED → OFFERED → AGREED → IN_PROGRESS → SUBMITTED_FOR_ACCEPTANCE → COMPLETED. Alternative terminal or exception states: CANCELLED, EXPIRED, DISPUTED, REFUNDED/PARTIALLY_REFUNDED (only when supported by provider-confirmed financial evidence). State transitions must be explicit, authorized, timestamped, and auditable. A status label alone is not proof of payment or completion.

## 8. Change requests
After agreement, scope, price, deadline, materials, or milestones may change only through a versioned change request. It records the initiator, reason, old/new values, timestamp, and affirmative confirmation by all affected parties. Never silently overwrite original terms.

## 9. Acceptance and evidence
Each service domain defines what constitutes a deliverable and acceptance. Evidence may include messages, structured checklists, photographs, videos, files, test results, timestamps, and confirmations. Preserve provenance and versions; distinguish user-supplied raw evidence from platform-generated summaries or AI-derived suggestions. Evidence insufficiency remains INCONCLUSIVE.

## 10. Complaints and expert panel
A complaint process gives both parties notice and a fair opportunity to submit evidence and respond. A three-person independent expert panel may provide a reasoned non-judicial expert opinion where suitable experts are available. Conflict-of-interest disclosure, recusal, replacement, equal access to relevant case materials, deadlines, separate opinions, majority rationale, and an INCONCLUSIVE outcome are mandatory design requirements. Experts are paid for review effort, never for a desired outcome. The panel is not a state court; its opinion is not automatically enforceable and must not remove statutory consumer, court, or other rights.

## 11. Professional projects and teams
A future project object may group multiple orders, providers, tasks, milestones, budgets, deliverables, and dependencies. It must preserve each provider's distinct contract, responsibility, access scope, and payment. Do not launch team orchestration until project ownership, lead-provider responsibility, IP rights, confidentiality, and payment flows are legally reviewed.

## 12. Trust and reputation
Separate factual verification (identity/status/qualification), completed-order history, customer feedback, complaint history, and platform risk signals. Never present an unverified claim as verified. Do not use raw star ratings as the sole quality measure. Provide correction/appeal paths and protect against retaliation, fake reviews, duplicate accounts, and discriminatory ranking.

## 13. AI boundaries
AI may classify requests, ask clarifying questions, draft task descriptions, suggest categories, and assist matching. AI must disclose uncertainty, allow human correction, and not independently certify qualifications, diagnose dangerous conditions, set binding prices, adjudicate disputes, or fabricate evidence. Human confirmation is required for consequential decisions.

## 14. Monetization hypotheses
Possible models include a transparent fee/commission for completed orders and separately agreed project-coordination fees. No monetization model is accepted until the legal role, payment/fiscalization flow, consumer disclosures, provider contract, and unit economics are reviewed.

## 15. Non-functional requirements
Security and privacy by design; Russian data-localization assessment for personal data of Russian citizens; accessibility on mobile; resilient operation; auditable state transitions; backups and tested restoration; observability; provider portability; versioned APIs; data export/deletion workflows; and documented incident response.

## 16. Non-goals for the first pilot
No custom bank/payment wallet, no storage of card data, no passport-scan archive, no claim of escrow, no automatic binding arbitration, no nationwide launch, no full native mobile apps, no autonomous AI adjudication, and no launch of all professional categories at once.

## 17. Launch gates
Pilot launch requires approved legal/business model, reviewed privacy notices and contracts, confirmed payment and receipt responsibilities, security review, incident/backup procedures, provider eligibility rules, complaint handling, support ownership, and successful end-to-end tests. UNKNOWN means HOLD, not approval.
