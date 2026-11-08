# MASTER — Architecture v1.0 (Foundation)

**Status:** target architecture; not implemented.

## 1. Architecture goals
- One coherent product with independent service domains.
- Core order, identity, evidence, trust, and audit capabilities reused across categories.
- External providers replaceable without leaking vendor-specific concepts into the domain core.
- Small operational footprint for the Lesnoy pilot.
- Explicit security, privacy, legal, and failure boundaries.

## 2. Initial deployment shape
Start with a **modular monolith**: one deployable backend, a relational primary database, background worker(s), object storage, and a web/PWA frontend. Keep module boundaries strict in code and data access. Do not start with microservices; extract a module only when scale, security isolation, team ownership, or operational evidence justifies the cost.

## 3. Logical modules
- Identity & Access: accounts, sessions, roles, external identity adapters.
- Customer & Provider Profiles: profiles, service areas, availability, business/qualification claims.
- Service Catalog: categories, domain-specific schemas, eligibility rules, workflow definitions.
- Matching & Dispatch: search, ranking, availability, notifications.
- Orders: offers, agreement snapshots, state machine, cancellation.
- Change Requests: versioned scope/price/deadline changes.
- Projects & Teams (later): project grouping, tasks, dependencies, participant permissions.
- Evidence: uploads, metadata, provenance, hashes, retention, access.
- Trust & Reputation: verified attributes, completed-order metrics, feedback, risk signals.
- Complaints & Expert Panel: case workflow, expert selection, recusals, opinions, appeal/review.
- Payments Adapter: payment intents/references/status webhooks; no card data.
- Compliance: notices, consent/legal-basis records, provider eligibility checks, retention/deletion workflows.
- Notifications: email/SMS/messenger/push adapters.
- Admin & Support: audited operational tools.
- Audit & Observability: append-only business audit events, technical logs, metrics, traces.

## 4. Domain extensibility
A service category is data/configuration plus a versioned domain package: request schema, required questions, eligibility rules, workflow/state extensions, deliverable types, acceptance checklist, evidence requirements, warranty/complaint guidance, and risk classification. The generic order engine owns identity, authorization, immutable agreement snapshots, event history, and common transitions. Category-specific code must not modify generic core behavior without an architecture decision.

## 5. Provider adapters
Define internal interfaces for identity, payment, messaging, storage, verification, maps/geocoding, and monitoring. Adapter contract tests must verify timeout, retry, idempotency, signature validation, error mapping, and provider outage behavior. Keep provider IDs and raw payloads at adapter boundaries; store only necessary normalized references in domain records.

MAX, VK, banks, and other services are candidates only. Availability of OAuth/ID, bot, notification, and business APIs must be verified against current official documentation and contractual access before committing to an integration. A messaging channel is not automatically an identity provider.

## 6. Data and transaction principles
- Relational database is the source of truth for accounts, agreements, orders, and state transitions.
- Object storage holds evidence files; database stores metadata, access policy, content hash, and lifecycle.
- Use transactional outbox or equivalent reliable event delivery for external side effects.
- Webhooks are authenticated, idempotent, replay-protected, and reconciled.
- Store immutable snapshots of agreed terms and append-only audit events.
- Do not rely on a notification or browser redirect as proof of payment.
- All timestamps are stored in UTC with explicit display timezone; money uses integer minor units and currency codes.

## 7. Security boundaries
Separate public web, application, database, object storage, background workers, and provider credentials. Enforce tenant/user/object-level authorization on every request. Admin access is least-privilege and audited. Secrets belong in managed secret storage, never in source code or logs.

## 8. API and compatibility
Version external APIs. Define schemas and compatibility policy. Use additive changes by default; breaking changes require versioning, migration plan, consumer inventory, deprecation window, and rollback strategy. Database migrations must be forward/backward considered and tested against realistic data.

## 9. Reliability
Define service-level objectives after pilot traffic is measured. Before launch implement health checks, structured logs, alerting, backups, restore drills, provider timeouts, bounded retries, rate limits, graceful degradation, and operational runbooks. A backup is not proven until restoration has been tested.

## 10. Deployment environments
Separate local development, test/CI, staging, and production. Production data must not be copied into development without approved anonymization. Use infrastructure-as-code when deployment topology stabilizes. Enforce protected main branch and reviewable pull requests.

## 11. Architecture evolution
Every material architecture choice is recorded in ARCHITECTURE_DECISIONS.md. Before changing a decision, use CHANGE_MANAGEMENT.md and inspect affected modules, data, APIs, security, legal obligations, integrations, tests, migration, and rollback. No new domain may require a rewrite of the shared core by default.
