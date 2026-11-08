# MASTER — Conceptual Data Model v1.0

**Status:** conceptual model; no database schema has been implemented.

## 1. Modeling rules
Use stable internal identifiers; explicit ownership; timestamps; versioned records; data classification; retention rules; and audit events. Avoid storing identity-provider credentials, payment-card data, or unnecessary identity documents.

## 2. Core entities
- Account: platform principal and lifecycle.
- IdentityLink: external provider, external subject identifier, verification metadata, link status. Never store provider password or access token unless a narrowly justified secure integration requires a short-lived token.
- PartyProfile: customer/provider public and operational profile.
- ProviderBusinessStatus: declared legal form/status, source, check time, result, expiry/recheck policy.
- Qualification: category, credential type, issuer, reference, verification status, validity period.
- ServiceCategory / CategoryVersion: category schema, rules, workflow and version.
- ServiceArea / Availability: provider coverage and schedule.
- Request: initial customer need and attachments.
- Offer: provider proposal and validity.
- Order: agreed service engagement and lifecycle.
- AgreementSnapshot: immutable agreed scope, price, deadlines, cancellation, warranty, acceptance criteria, and parties.
- ChangeRequest / ChangeApproval: proposed change and each required party's decision.
- Milestone / Deliverable: work stages and outputs.
- EvidenceItem: file/reference, author, source, timestamps, hash, classification, access and retention.
- PaymentReference: external provider reference, amount/currency, status events, reconciliation status; no card number.
- Complaint / Case / ExpertAssignment / ExpertOpinion: dispute and review records.
- Review: feedback tied to a completed eligible order.
- Project / ProjectTask / ProjectParticipant: later multi-order project model.
- ConsentOrLegalBasisRecord: purpose, data category, notice/version, legal basis, timestamp, withdrawal where applicable.
- AuditEvent: actor, action, target, time, correlation ID, before/after references where appropriate.
- NotificationDelivery: channel, template version, provider reference, delivery state.
- SupportTicket / SecurityIncident: operational and incident records.

## 3. Important relationships
A customer can create many requests. A request can receive many offers and become one or more orders only through explicit acceptance rules. An order has one or more immutable agreement versions, change requests, milestones, evidence items, payment references, and complaint cases. A project groups tasks and orders but does not erase the separate parties and agreements. An expert opinion belongs to one case and one expert assignment.

## 4. Data classification
- Public: published category descriptions and deliberately public provider profile fields.
- Internal: operational metadata, non-public provider availability, support workflow.
- Personal: names, contact details, addresses, account identifiers, messages, order history.
- Sensitive/high-risk: identity verification artifacts, access credentials, payment-related tokens, private case materials, precise location, confidential client files.
- Special-category data: do not collect unless a documented legal basis and strict necessity are established.

## 5. Data minimization
Collect only what is necessary for a defined purpose. Use external providers for authentication and payment-card handling where lawful and available. A provider's assertion or verification token is not a blanket substitute for the platform's own legal obligations. Avoid passport scans and card data in MASTER by default.

## 6. Russian data localization
Before processing personal data of Russian citizens, design and verify database placement and data flows against applicable Russian localization requirements and exceptions. The production architecture must be reviewed by qualified counsel; do not assume foreign SaaS storage is acceptable.

## 7. Lifecycle and retention
Every entity/data category needs: purpose, legal basis, source, owner/controller role, access roles, retention period/event, deletion/anonymization process, backup expiry, and legal-hold exception. Retention periods are not guessed in code; they are approved in the data inventory and policy.

## 8. Integrity and audit
Use immutable agreement snapshots and append-only audit records for material actions. Corrections are new events/versions, not silent edits. Store cryptographic hashes for evidence files where useful, while recognizing a hash proves file identity after capture, not truth of the content or when the depicted event occurred.

## 9. Open design questions
Resolve before schema freeze: legal role of MASTER for each data flow; exact identity attributes; address precision and retention; messaging content retention; expert case access; payment reconciliation fields; cross-border provider flows; deletion vs statutory retention; project/tenant model.
