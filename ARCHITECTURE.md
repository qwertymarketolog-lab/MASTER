# MASTER — Architecture v1.1 (Foundation)

**Status:** target architecture; not implemented. Identity & Organization details are proposed in IDENTITY_AND_ORGANIZATION_MODEL.md.

## 1. Architecture goals

- One coherent product with independent service domains.
- Core identity, Party, access, order, evidence, trust, and audit capabilities reused across categories.
- External providers replaceable without leaking vendor-specific concepts into the domain core.
- Small operational footprint for the Lesnoy pilot.
- Explicit security, privacy, legal, and failure boundaries.

## 2. Initial deployment shape

Start with a **modular monolith**: one deployable backend, a relational primary database, background worker(s), object storage, and a web/PWA frontend. Keep module boundaries strict in code and data access. Do not start with microservices; extract a module only when scale, security isolation, team ownership, or operational evidence justifies the cost.

## 3. Logical modules

- **Identity & Access:** Account, Person, IdentityLink, sessions, authentication adapters, authorization context, role/permission catalogue.
- **Parties & Organizations:** Individual Party, Organization, Membership, Role Assignment, Representation, business-status claims.
- **Customer & Provider Profiles:** public and operational profiles, service areas, availability, business/qualification claims.
- **Service Catalog:** categories, domain-specific schemas, eligibility rules, workflow definitions.
- **Matching & Dispatch:** search, ranking, availability, notifications.
- **Orders:** offers, agreement snapshots, state machine, cancellation.
- **Change Requests:** versioned scope/price/deadline changes.
- **Projects & Teams (later):** project grouping, tasks, dependencies, participant permissions and cross-Party resource grants.
- **Evidence:** uploads, metadata, provenance, hashes, retention, access.
- **Trust & Reputation:** verified attributes, completed-order metrics, feedback, risk signals.
- **Complaints & Expert Panel:** case workflow, expert selection, recusals, opinions, appeal/review.
- **Payments Adapter:** payment intents/references/status webhooks; no card data.
- **Compliance:** notices, legal-basis records, provider eligibility checks, retention/deletion workflows.
- **Notifications:** email/SMS/messenger/push adapters.
- **Admin & Support:** audited operational tools.
- **Audit & Observability:** append-only business audit events, technical logs, metrics, traces.

## 4. Identity, Party, and authorization boundaries

Account is an authentication principal; Person is the human identity; Party is the participant in a domain relationship; Organization is a non-human Party represented by people. Membership, Role Assignment, and Representation are separate relationships. A person may be both customer and provider and may participate in several Organizations.

The Identity & Access module owns authentication and authorization contracts. Parties & Organizations owns Party, Organization, Membership, and Representation lifecycle. Other modules must not infer access from profile fields or duplicate role logic. Domain modules ask the shared authorization interface to evaluate the actor, acting Party, action, resource, relationship, and policy attributes.

Use scoped RBAC combined with relationship checks and ABAC conditions. Deny by default. A role label alone never grants access to all resources. Platform staff privileges are separate from customer/provider/organization roles. Organization membership does not automatically prove legal signing authority.

Tenant isolation follows resource ownership by Party and explicit cross-Party grants. The tenant context is derived and validated server-side; a client-supplied tenant identifier is never trusted as authority. Apply equivalent boundaries to database queries, object storage, search, caches, exports, background jobs, and analytics.

## 5. Domain extensibility

A service category is data/configuration plus a versioned domain package: request schema, required questions, eligibility rules, workflow/state extensions, deliverable types, acceptance checklist, evidence requirements, warranty/complaint guidance, and risk classification. The generic order engine owns identity, authorization, immutable agreement snapshots, event history, and common transitions. Category-specific code must not modify generic core behavior without an architecture decision.

## 6. Provider adapters

Define internal interfaces for identity, payment, messaging, storage, verification, maps/geocoding, and monitoring. Adapter contract tests must verify timeout, retry, idempotency, signature validation, error mapping, and provider outage behavior. Keep provider IDs and raw payloads at adapter boundaries; store only necessary normalized references in domain records.

MAX, VK, banks, and other services are candidates only. Availability of OAuth/ID, bot, notification, and business APIs must be verified against current official documentation and contractual access before committing to an integration. A messaging channel is not automatically an identity provider.

## 7. Data and transaction principles

- Relational database is the source of truth for accounts, parties, memberships, agreements, orders, and state transitions.
- Object storage holds evidence files; database stores metadata, access policy, content hash, and lifecycle.
- Use transactional outbox or equivalent reliable event delivery for external side effects.
- Webhooks are authenticated, idempotent, replay-protected, and reconciled.
- Store immutable snapshots of agreed terms and append-only audit events.
- Do not rely on a notification or browser redirect as proof of payment.
- All timestamps are stored in UTC with explicit display timezone; money uses integer minor units and currency codes.

## 8. Security boundaries

Separate public web, application, database, object storage, background workers, and provider credentials. Enforce tenant/user/object-level authorization on every request. Admin access is least-privilege and audited. Secrets belong in managed secret storage, never in source code or logs.

## 9. API and compatibility

Version external APIs. Define schemas and compatibility policy. Use additive changes by default; breaking changes require versioning, migration plan, consumer inventory, deprecation window, and rollback strategy. Database migrations must be forward/backward considered and tested against realistic data.

## 10. Reliability

Define service-level objectives after pilot traffic is measured. Before launch implement health checks, structured logs, alerting, backups, restore drills, provider timeouts, bounded retries, rate limits, graceful degradation, and operational runbooks. A backup is not proven until restoration has been tested.

## 11. Deployment environments

Separate local development, test/CI, staging, and production. Production data must not be copied into development without approved anonymization. Use infrastructure-as-code when deployment topology stabilizes. Enforce protected main branch and reviewable pull requests.

## 12. Architecture evolution

Every material architecture choice is recorded in ARCHITECTURE_DECISIONS.md. Before changing a decision, use CHANGE_MANAGEMENT.md and inspect affected modules, data, APIs, security, legal obligations, integrations, tests, migration, and rollback. No new domain may require a rewrite of the shared core by default.
