# MASTER — Identity & Organization Model v1.0

**Status:** proposed domain model for product-owner review; not implemented and not a legal determination.

## 1. Purpose and boundaries

This model separates authentication, a human being, a platform participant, an organization, and the authority to act for another participant. These concepts must not be collapsed into one user record or one global role.

The model is intended to support:
- one person acting as both customer and provider;
- a self-employed person or individual entrepreneur acting in their own name;
- a legal entity acting through authorized people;
- a person participating in several organizations;
- project teams and temporary specialists;
- experts and support staff with narrowly scoped access;
- delegation and revocation of authority without transferring ownership of orders or data.

This is a conceptual contract. It does not decide whether a particular person or organization is legally eligible to provide a regulated service. Legal form, tax status, qualification, authority, and category eligibility remain separate checks.

## 2. Core concepts

### 2.1 Account

An **Account** is an authenticated platform principal used to sign in and maintain sessions.

- It is not a customer, provider, company, or legal party by itself.
- It has a stable internal identifier, `account_type` (`HUMAN` or `SERVICE`), lifecycle state, security settings, and authentication links. HUMAN Accounts resolve to a Person; SERVICE Accounts do not require a Person and must have a named owning operator, narrow machine scopes, credential rotation/revocation, and explicit audit attribution.
- An organization never signs in by sharing one organization password. People sign in with their own accounts and act under explicit authority.
- Account recovery, linking, suspension, and closure are security-controlled lifecycle operations.
- An account must not be merged with another account solely because an email address or phone number matches.

### 2.2 Person

A Person has a claim-specific VerificationProfile rather than one universal trust score. Verification claims are independently recorded with level/status, source, scope, checked_at, valid_until/recheck_at, and evidence reference. Initial dimensions may include email, phone, identity, and other legally justified identity assurance claims. A higher level in one dimension does not imply another claim is verified.

ContactPoints (email, phone, postal address, bank-account endpoint, or other approved contact channel) are separate records linked to a Person or Party as appropriate. Each has purpose, verification state/source/time, visibility, access policy, and retention. Changing a ContactPoint does not change the Person identity; bank-account verification does not prove identity, tax status, or legal authority.


A **Person** is the human identity to which one or more verified account access methods may be linked.

- Person identity attributes are minimized and classified as personal data.
- In the MVP, one Person has at most one active Account; multiple authentication methods may be linked to that Account. Account consolidation is a separately reviewed future capability.
- External identity-provider authentication proves only what the provider's documented assurance level supports. It does not automatically prove professional qualification, tax status, legal capacity, or authority to represent an organization.
- Account-to-Person linking and account recovery must prevent unauthorized account takeover and accidental identity merges.

### 2.3 Party

A **Party** is a participant in a platform relationship, request, offer, agreement, order, project, or case.

Party is the common reference used by domain records where a customer/provider/contracting participant is required. It has an explicit party kind:

- **INDIVIDUAL** — a natural person acting in their own name.
- **ORGANIZATION** — an organizational container represented by authorized people. Its `OrganizationType` describes its operating form; it does not by itself determine legal personality or contracting-party status.

A Person may be linked to an Individual Party. An Organization Party is not a login identity and acts through people with valid authority. Party taxonomy is `INDIVIDUAL | ORGANIZATION`; `OrganizationType` is a separate attribute, initially `LEGAL_ENTITY | SOLE_PROPRIETOR | TEAM | OTHER`.

A Party is not automatically a legal conclusion about who is a contracting party. The actual contracting parties for each workflow must match the approved legal model and agreement.

### 2.4 Organization

An **Organization** is a non-human organizational participant with its own stable platform identity and independently maintained legal/business attributes.

- Legal form, registration identifiers, jurisdiction, status, and verification source are separate attributes with restricted access and retention.
- Organization membership does not itself prove statutory representation or signing authority.
- Organization records must not be fabricated from an individual's profile.
- A sole proprietor acting alone may use an Individual Party with a time-bound `TaxStatus` of `SOLE_PROPRIETOR`.
- Where an individual entrepreneur needs a multi-person operating container, an Organization of type `SOLE_PROPRIETOR` may represent the business workspace, with the entrepreneur as owner/member. This is a platform modeling container, not a declaration that the business is a separate legal person; the actual contracting Party and payment recipient must be resolved by the approved legal scenario.
- An informal team may use an Organization of type `TEAM` as a collaboration container. This does not create a legal entity, employer relationship, partnership, or single contracting party. Independent team members retain their own legal/tax status and may require separate agreements and payment flows.
- A legal entity may have multiple people, roles, and delegated representatives.

### 2.6 Membership

A **Membership** connects a Person to an Organization and grants scoped platform participation.

Minimum attributes:
- organization_id and person_id;
- status: INVITED, ACTIVE, SUSPENDED, EXPIRED, REVOKED;
- start/end timestamps;
- invitation and acceptance evidence;
- assigned organization-scoped roles;
- scope/limitations, if any;
- created_by, changed_by, and audit references.

Rules:
- Membership is not ownership of the organization and does not automatically prove legal signing authority.
- Membership is not transferable.
- Revocation blocks future actions as soon as the authoritative membership state changes; cached permissions must have bounded lifetime and revocation-aware checks.
- The system must prevent accidental removal or disabling of the last required primary administrator without a controlled recovery path.
- Historical orders, agreements, and audit events remain attributed to the person and Party that acted at the time; revoking membership does not rewrite history.

### 2.7 Role, Permission, and Role Assignment

A **Role** is a named bundle of permissions. A **Permission** is a narrowly defined action on a resource or operation. A **Role Assignment** grants a role within an explicit scope.

- Roles are scoped, not global by default: organization, project, case, or platform-operation scope.
- Example organization roles: ORG_PRIMARY_ADMIN, ORG_MANAGER, ORG_MEMBER, ORG_FINANCE, ORG_VIEWER.
- Example project roles: PROJECT_LEAD, PROJECT_CONTRIBUTOR, PROJECT_VIEWER.
- Example case roles: CASE_PARTY, CASE_EXPERT.
- Platform staff roles (SUPPORT, COMPLIANCE, FINANCE_OPERATIONS, SECURITY_OPERATIONS) are a separate namespace and never arise from customer/provider membership.
- Role names are illustrative; the permission catalogue and exact role bundles require security review.
- No implicit permission inheritance is assumed. Role inheritance, if later introduced, must be explicit, versioned, and tested.
- User-defined custom roles are out of scope for the first pilot.

Authorization combines:
1. **RBAC** — role assignment grants candidate permissions;
2. **relationship checks** — the actor must have a valid relationship to the target Party/resource;
3. **ABAC** — attributes and conditions such as status, scope, time, case assignment, or resource classification further constrain access.

A role alone never authorizes access to every resource.

### 2.8 TaxStatus and Provider Business Status

`TaxStatus` is a time-bound legal/tax classification attached to the relevant Party and supported by a named source, check time, validity/recheck policy, and result. Examples include `INDIVIDUAL`, `SELF_EMPLOYED_NPD`, `SOLE_PROPRIETOR`, and other legally reviewed statuses. It is not inferred from `OrganizationType`, identity verification, or qualification. Where the same human participates through more than one Party, status is evaluated for the Party and transaction context involved. A tax-status change creates a new version/event; historical transactions retain the status snapshot used at the time. Tax status alone does not determine the contracting party, tax agent, receipt issuer, or merchant of record; those are scenario-level legal decisions.

### 2.9 Representation and Delegation

A **Representation** records authority for a Person or Party to act on behalf of another Party for a defined scope.

Minimum attributes:
- represented_party_id;
- representative_person_id and/or representative_party_id;
- authority_type and source;
- permitted actions and resource scope;
- effective_from and effective_until;
- status: PENDING, ACTIVE, SUSPENDED, EXPIRED, REVOKED, REJECTED;
- evidence reference and verification result, where required;
- grantor, verifier, timestamps, and audit references.

Rules:
- Representation is separate from Membership. Membership may be one prerequisite for organizational access, but does not automatically confer legal authority to sign contracts or make every decision.
- Authority evidence is verified proportionately to the action's risk. Store verification metadata and a reference where possible; do not retain identity-document copies by default.
- Revocation and expiry prevent new actions. Previously valid actions remain in the historical audit trail and are not silently invalidated.
- Delegation chains are disabled by default. If later enabled, maximum depth, permitted sub-delegation, and proof requirements must be explicit.
- A representative cannot grant themselves authority on behalf of another Party.
- The UI must show which Party the person is acting for before consequential actions and capture that Party in the resulting agreement/event.

## 3. Acting Party contract

Every consequential domain action refers to one normalized `ActingParty` value:

`ActingParty = (Party, optional Representation)`

- A person acting for themselves uses their Individual Party with explicit self-capacity; no fabricated delegation record is required.
- A person acting for an Organization or another Party uses that Party plus the applicable active Representation when the action legally or operationally requires it. Membership and Role Assignment remain separate authorization inputs.
- Request, Offer, Agreement, Order, Payment, Refund, Payout, Settlement, and Dispute events use the same ActingParty reference shape; the legal counterparty/beneficiary remains a separately resolved transaction role where required.
- At each consequential action, persist an immutable `ActingPartySnapshot`: actor Account and Person IDs, Party ID and kind/type, capacity, Membership/Role Assignment/Representation identifiers and versions used, relevant verification/tax-status claim versions, authorization decision reference, and event timestamp. Do not rewrite this snapshot after a membership ends, a representative leaves, or a profile changes.
- A snapshot records what the platform evaluated and who acted; it does not independently prove legal authority or override a later legal finding.

## 4. Relationship rules and invariants

1. In the MVP, one active Account resolves to one Person; account merging and multiple active Accounts per Person are out of scope.
2. A Person may have an Individual Party and memberships in multiple Organizations.
3. A Person may act as customer in one workflow and provider in another; customer/provider are contextual capabilities, not mutually exclusive account types.
4. An Organization acts only through an authenticated Person with an active Membership and the required Role Assignment and, where legally required, valid Representation.
5. TaxStatus is a time-bound attribute of the relevant Party (and is snapshotted for transaction events); it does not automatically change Party kind. A multi-person sole-proprietor workspace may use Organization(SOLE_PROPRIETOR) without asserting separate legal personality.
6. Qualification, license, tax status, identity assurance, and authority are distinct verification dimensions. Passing one does not imply passing another.
7. Every consequential action records a normalized ActingParty and immutable ActingPartySnapshot, including actor_account_id, actor_person_id, acting_party_id, relevant authority/role references and their versions.
8. A resource has an explicit owning Party or explicit platform ownership classification. Access by another Party requires an explicit relationship or grant.
9. Public catalogue data is not tenant-private data. Private organization/project/case data is never exposed merely because a user knows an identifier.
10. No organization may share login credentials. Individual accountability is mandatory.

## 5. Tenant and resource isolation

A **tenant scope** is a security partition for data owned by an Individual Party or Organization Party. It is not a claim that every database row must carry a simplistic tenant_id.

- Every tenant-owned resource must resolve to an owning Party and a tenant scope.
- Organization-owned resources are isolated from other organizations by default.
- Individual Party resources are isolated from other individuals by default.
- Cross-Party collaboration is granted explicitly at the resource or project boundary and limited to the minimum required actions.
- Shared resources (for example, public category definitions) are explicitly classified as platform-global.
- Database queries, object storage access, search indexes, caches, exports, background jobs, and analytics must preserve the same authorization boundary.
- Where practical, database constraints and composite ownership keys should make cross-tenant references difficult to create accidentally.
- Tenant context must be derived from authenticated relationships and server-side authorization, never trusted from a client-supplied tenant identifier alone.

## 6. Authorization decision contract

For each protected operation, the authorization layer evaluates:

- authenticated Account and active Person;
- requested action and resource;
- acting Party selected by the user;
- active Membership, Role Assignment, and/or Representation;
- resource ownership and explicit sharing relationships;
- resource state, sensitivity, and time constraints;
- any additional policy conditions.

Default outcome is DENY. Ambiguous identity, stale membership, missing authority, conflicting scopes, or unavailable authorization evidence must fail closed for consequential actions.

Authorization must be checked on the server for every request and again at execution time for delayed jobs or sensitive asynchronous actions. Frontend visibility is not an access control.

## 7. Lifecycle and audit

- Account, Person, Party, Membership, Role Assignment, and Representation have explicit lifecycle states.
- State changes are timestamped and attributed; material changes append audit events rather than silently rewriting history.
- Invitations are single-use, expiring, and bound to the intended recipient and organization.
- Membership and representation revocation must invalidate sessions/tokens or force a fresh authorization check within a documented bound.
- Account closure does not automatically delete records that must be retained for contracts, accounting, safety, dispute handling, or other lawful purposes. Retention and deletion are governed by DATA_PROTECTION.md.
- Where deletion is required but a minimal historical transaction reference must lawfully remain, identifying Person data may be pseudonymized and replaced with a minimal tombstone/reference. This is not a blanket retention exception: each retained field needs a documented legal basis, purpose, access restriction, and expiry/review rule. Backups and derived copies follow the approved deletion schedule.
- Administrative support access is separate from acting as the user. No password impersonation. Exceptional access requires a ticket/reason, narrow scope, time limit, audit, and review.

## 8. MVP boundary

For the Lesnoy pilot:
- The customer and an individual provider may each act through an Individual Party.
- A person may use the same Account for both customer and provider journeys.
- Organization profiles and multi-person Membership workflows may remain disabled in the user interface until their legal, operational, and security checks are complete.
- The conceptual model must nevertheless preserve the Organization, Membership, Role Assignment, and Representation boundaries so later support does not require redefining Account or Party.
- No production onboarding of organizations or delegation is authorized by this document alone.

## 9. Required verification before implementation freeze

- Identity/account linking and recovery threat model; enforce one active Account per Person in MVP, with merge excluded.
- Party/OrganizationType/TaxStatus matrix and transaction-specific legal counterparty mapping.
- ActingParty normalization and immutable snapshot tests across all consequential transaction events.
- Verification claims and ContactPoint lifecycle/privacy controls.
- Service Account ownership, scope, credential rotation, revocation, and attribution tests.
- Pseudonymization/tombstone behavior preserving lawful historical transaction attribution.
- Service Account permissions and non-human actor attribution.
- Pseudonymization/tombstone behavior preserving lawful historical transaction attribution.
- Permission matrix for each role, action, resource, and scope.
- Cross-tenant isolation tests for API, database access, object storage, search, caches, exports, and background jobs.
- Membership invitation, expiry, suspension, and revocation tests.
- Representation proof, expiry, revocation, and action-scope tests.
- Last-primary-administrator recovery tests.
- Audit attribution tests for actions performed on behalf of a Party.
- Data inventory, retention, localization, and legal-role review for identity and organization attributes.
- Review of legal evidence requirements for organization representation and signing authority.

**Open decisions:** exact verification claim catalogue and evidence sources; permitted proof sources for representation; detailed role-permission matrix; tenant partitioning strategy at database and storage layers; organization onboarding timing; legal mapping of Organization(SOLE_PROPRIETOR) and Organization(TEAM) to contracting parties and payment recipients. These remain PROPOSED until explicitly approved.
