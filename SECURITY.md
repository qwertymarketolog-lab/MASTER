# MASTER — Security Baseline v1.1

**Status:** required security design baseline; implementation and independent review remain outstanding.

## 1. Security objectives

Protect account access, personal data, confidential project material, payment references, evidence integrity, service availability, and trust in order/dispute history.

## 2. Minimum controls before pilot

- MFA for privileged/admin accounts; strong session management for all accounts.
- Secure password handling if local credentials exist; prefer established identity protocols and libraries.
- Object-level authorization on every API operation; deny by default.
- Scoped RBAC combined with relationship checks and ABAC conditions; no role-only or frontend-only authorization.
- Tenant isolation for database, object storage, search, caches, exports, analytics, and background jobs.
- Explicit acting Party selection for consequential actions; server-side verification of Membership and Representation.
- Immediate revocation semantics for suspended/revoked memberships and representations, with bounded cache/token lifetimes.
- Encryption in transit; encryption at rest for databases, backups, and object storage.
- Managed secrets; rotation and access auditing.
- Input validation, output encoding, CSRF protections where applicable, secure cookies, rate limiting, abuse controls.
- File upload allowlists, size limits, malware scanning, private-by-default storage, short-lived signed access URLs.
- Webhook signature validation, idempotency, replay protection, and reconciliation.
- Dependency and container scanning; pinned/reviewed dependencies; timely security updates.
- Centralized security events and alerting without logging secrets or unnecessary personal data.
- Tested backups and restoration; documented incident response.
- Secure development review and automated tests in CI.

## 3. Identity and account security

No shared organization credentials. Each human operator uses an individual Account. Account linking, recovery, and merging require verified control and explicit confirmation; matching email/phone alone is insufficient. External identity assurance must be recorded with source, time, and scope and must not be treated as proof of qualification, tax status, or organizational authority.

## 4. Authorization and tenant isolation

For each protected action, evaluate authenticated Account, Person, acting Party, requested action, target resource, resource owner, active Membership, scoped Role Assignment, Representation where required, resource state, and policy attributes. Default outcome is DENY. Client-supplied tenant identifiers and hidden UI controls are not authorization evidence.

Cross-tenant sharing must be explicit, scoped, time-bound where appropriate, and auditable. Test for IDOR and cross-tenant leakage across APIs, database access, object storage, search indexes, caches, exports, and background workers. A shared Project does not grant blanket access to every participant's data.

Membership or Representation revocation must block new consequential actions within a documented bound. Sensitive delayed jobs must re-check authorization at execution time rather than relying on the state at enqueue time.

## 5. Privileged access

No shared admin accounts. Separate support, compliance, finance, and infrastructure permissions. Require reason/ticket for exceptional access to private case files. Log actor, target, purpose, time, and outcome. Review privileged access periodically and revoke promptly. No password impersonation; exceptional support access is scoped, time-limited, and reviewed.

## 6. Evidence and case confidentiality

Evidence is private by default and visible only to parties and authorized reviewers. Experts receive case-scoped, time-limited access. Prevent bulk export by default. Record access and downloads. Redact unrelated personal data before sharing where appropriate.

## 7. Incident response

Define severity levels, on-call/owner, containment, evidence preservation, legal notification assessment, affected-user communication, recovery, and post-incident review. Do not claim an incident is resolved until recovery and integrity checks pass.

## 8. Security testing

Before launch: threat-model review, dependency scan, static analysis, authorization matrix tests, cross-tenant isolation tests, membership/representation lifecycle tests, upload tests, webhook tests, backup restore test, and an independent penetration/security review proportionate to risk. High-risk findings block launch.

## 9. Secure failure behavior

Provider outages, invalid signatures, authorization ambiguity, stale membership, missing authority, inconsistent payment status, and insufficient evidence fail closed for consequential actions. Do not silently downgrade identity, payment, or expert-review assurance.
