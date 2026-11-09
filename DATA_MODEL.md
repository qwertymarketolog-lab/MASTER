# MASTER — Conceptual Data Model v1.1

**Status:** conceptual model; no database schema has been implemented. Identity & Organization details are proposed in IDENTITY_AND_ORGANIZATION_MODEL.md.

## 1. Modeling rules

Use stable internal identifiers; explicit Party ownership; timestamps; versioned records; data classification; retention rules; and audit events. Separate authentication, human identity, domain participants, organizational membership, authority, and permissions. Avoid storing identity-provider credentials, payment-card data, or unnecessary identity documents.

## 2. Identity and organization entities

- **Account:** authenticated platform principal, session/security lifecycle, and linked authentication methods. It is not a customer/provider or organization.
- **IdentityLink:** external identity provider, external subject identifier, assurance metadata, link status, and verification time. No provider password; tokens only where narrowly justified and securely managed.
- **Person:** human identity record linked to an authenticated Account through a controlled linking process.
- **Party:** domain participant used by requests, offers, agreements, orders, projects, and cases; Party kind is INDIVIDUAL or ORGANIZATION.
- **IndividualParty:** Party representing a natural person acting in their own name.
- **Organization:** organizational Party/container with `organization_type` (LEGAL_ENTITY, SOLE_PROPRIETOR, TEAM, OTHER), separate legal/business attributes, jurisdiction, verification source, status, and retention controls. SOLE_PROPRIETOR/TEAM container types do not assert separate legal personality.
- **OrganizationType:** operating form of an Organization, distinct from Party kind and legal personality.
- **TaxStatus:** time-bound, source-backed status attached to the relevant Party, with checked_at, valid_until/recheck_at, result, and version; transaction events preserve the evaluated snapshot.
- **VerificationProfile / VerificationClaim:** independent claim-specific verification dimensions for Person and Organization (e.g. email, phone, identity, business, bank account), each with source, scope, status, and timestamps.
- **ContactPoint:** independently managed email/phone/bank endpoint, verification state, purpose, visibility, and retention; not embedded as immutable Person identity.
- **ServiceAccount:** non-human Account principal for integrations/automation, with owner/operator, narrow scopes, credentials lifecycle, and audit attribution; no Person link required.
- **ActingParty / ActingPartySnapshot:** normalized `(Party, optional Representation)` action context and immutable event-time snapshot of actor Account/Person, Party, role/membership/representation references and versions, relevant verification/TaxStatus versions, authorization decision, and timestamp.
- **PersonTombstone:** minimal pseudonymous historical reference retained only where a lawful retention basis requires transaction/audit continuity; identifying data is separately erased or pseudonymized under approved retention policy.
- **Membership:** Person-to-Organization relationship with invitation, status, validity, scope, and audit history.
- **RoleDefinition / PermissionDefinition:** versioned platform authorization vocabulary.
- **RoleAssignment:** scoped grant of a role to a Person or Party for an Organization, Project, Case, or platform-operational scope.
- **Representation:** scoped and time-bound authority to act on behalf of another Party, with evidence reference, verification state, grantor, revocation, and audit history.
- **ProviderBusinessStatus:** declared legal/tax form or status (including NPD or individual entrepreneur status where applicable), source, check time, result, expiry/recheck policy. This is separate from Party kind, identity assurance, and qualification.
- **Qualification:** category, credential type, issuer, reference, verification status, validity period, and evidence.
- **AuthorizationContext:** evaluated context for an action; may be transient rather than a persisted business entity. It combines actor Account/Person, acting Party, resource, action, Membership/Representation, Role Assignment, relationships, and policy attributes.

Detailed invariants and open decisions are in IDENTITY_AND_ORGANIZATION_MODEL.md.

## 3. Other core entities

- **ServiceCategory / CategoryVersion:** category schema, rules, workflow, and version.
- **ServiceArea / Availability:** provider coverage and schedule.
- **Request:** initial customer need, attachments, owner Party, and visibility policy.
- **Offer:** provider proposal, validity, submitting Party, and terms.
- **AgreementSnapshot:** immutable agreed scope, price, deadlines, cancellation, warranty, acceptance criteria, and contracting Parties.
- **Order:** agreed service engagement, lifecycle, and references to its customer/provider Parties and agreement versions.
- **ChangeRequest / ChangeApproval:** proposed change and each required Party's decision.
- **Milestone / Deliverable:** work stages and outputs.
- **EvidenceItem:** file/reference, author Account/Person, acting Party, source, timestamps, hash, classification, access, and retention.
- **PaymentReference:** external provider reference, amount/currency, status events, reconciliation status, and related Parties; no card number.
- **Complaint / Case / ExpertAssignment / ExpertOpinion:** dispute and review records with scoped Party access.
- **Review:** feedback tied to a completed eligible order and an identified reviewing Party.
- **Project / ProjectTask / ProjectParticipant:** later multi-order project model with explicit Party membership, role, task scope, and dependency.
- **ConsentOrLegalBasisRecord:** purpose, data category, notice/version, legal basis, timestamp, withdrawal where applicable.
- **AuditEvent:** actor Account/Person, acting Party, action, target, time, correlation ID, and before/after references where appropriate.
- **NotificationDelivery:** channel, template version, provider reference, delivery state.
- **SupportTicket / SecurityIncident:** operational and incident records.

## 4. Relationships and ownership

- An Account authenticates a Person; an Account does not own an Organization.
- A Person may have an Individual Party and may hold Memberships in multiple Organizations. MVP permits one active Account per Person; account merge is out of scope.
- A Person may act as customer in one workflow and provider in another.
- An Organization acts through a Person with an active Membership and the required scoped Role Assignment and, where required, valid Representation.
- Membership does not automatically prove legal signing authority.
- TaxStatus is a time-bound attribute of the relevant Party, separate from Party kind and OrganizationType. An individual entrepreneur may act as an Individual Party or use an Organization(SOLE_PROPRIETOR) workspace without implying separate legal personality; an informal team may use Organization(TEAM) without implying a legal entity or single contracting party.
- Every tenant-owned resource resolves to an owning Party. Public/platform-global resources are explicitly classified as such.
- Cross-Party access requires an explicit relationship or resource grant. A shared Project does not erase the distinct ownership, agreements, or responsibilities of its participants.
- Every consequential action records a normalized ActingParty and immutable ActingPartySnapshot; historical snapshots are not rewritten when memberships, representatives, contact points, or statuses later change.

A customer Party can create many Requests. A Request can receive many Offers and become one or more Orders only through explicit acceptance rules. An Order has one or more immutable AgreementSnapshots, ChangeRequests, Milestones, EvidenceItems, PaymentReferences, and Complaint cases. A Project groups tasks and Orders but does not erase separate Parties and agreements. An ExpertOpinion belongs to one Case and one ExpertAssignment.

## 5. Data classification

- **Public:** published category descriptions and deliberately public provider profile fields.
- **Internal:** operational metadata, non-public provider availability, support workflow.
- **Personal:** names, contact details, addresses, account identifiers, messages, order history, membership and representation records.
- **Sensitive/high-risk:** identity verification artifacts, access credentials, payment-related tokens, private case materials, precise location, confidential client files, authority evidence.
- **Special-category data:** do not collect unless a documented legal basis and strict necessity are established.

## 6. Data minimization and identity boundaries

Collect only what is necessary for a defined purpose. External authentication does not establish tax status, qualification, legal capacity, or authority to represent an Organization unless the provider's documented assurance and the relevant legal review support that exact claim. Store minimal verification outcomes and references where possible. Avoid passport scans and card data in MASTER by default.

## 7. Russian data localization

Before processing personal data of Russian citizens, design and verify database placement and data flows against applicable Russian localization requirements and exceptions. The production architecture must be reviewed by qualified counsel; do not assume foreign SaaS storage is acceptable.

## 8. Lifecycle and retention

Every entity/data category needs: purpose, legal basis, source, responsible role, access roles, retention period/event, deletion/anonymization process, backup expiry, and legal-hold exception. Retention periods are not guessed in code; they are approved in the data inventory and policy. Membership or Representation revocation does not erase historical records of actions validly performed.

## 9. Integrity and audit

Use immutable agreement snapshots and append-only audit records for material actions. Corrections are new events/versions, not silent edits. For consequential actions, record actor Account, Person, acting Party, and the Membership/Representation/Role Assignment used. Store cryptographic hashes for evidence files where useful, while recognizing a hash proves file identity after capture, not truth of the content or when the depicted event occurred.

## 10. Open design questions

Resolve before schema freeze: identity assurance levels; account linking and recovery; exact Organization attributes; legal proof for Representation; detailed role-permission matrix; tenant partitioning strategy at database/storage/search/cache layers; identity and membership retention; legal role of MASTER for each data flow; address precision; messaging content retention; expert case access; payment reconciliation fields; cross-border provider flows; deletion vs statutory retention; project/tenant model.


## 11. Transaction and financial model extension
The proposed financial object boundaries and lifecycle contracts are defined in TRANSACTION_FINANCIAL_MODEL.md. PaymentIntent, Payment, Transaction, Refund, Payout, Settlement, and Ledger must remain distinct. Financial events retain immutable ActingParty and TaxStatus/verification snapshots. This is a conceptual proposal pending legal/accounting/provider review; no production schema is approved.


## 12. Financial accounting model extension
The proposed chart-of-accounts seed, posting conditions, reconciliation evidence and lifecycle independence are specified in TRANSACTION_FINANCIAL_MODEL.md §8. No statutory chart, custody model, revenue-recognition rule or production posting configuration is approved. Ledger entries must reference authoritative source events and accounting-policy versions; suspense entries require ownership and ageing, never silent balancing.
