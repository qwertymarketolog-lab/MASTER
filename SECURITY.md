# MASTER — Security Baseline v1.0

**Status:** required security design baseline; implementation and independent review remain outstanding.

## 1. Security objectives
Protect account access, personal data, confidential project material, payment references, evidence integrity, service availability, and trust in order/dispute history.

## 2. Minimum controls before pilot
- MFA for privileged/admin accounts; strong session management for all accounts.
- Secure password handling if local credentials exist; prefer established identity protocols and libraries.
- Object-level authorization on every API operation; deny by default.
- Role- and attribute-based access for customer, provider, expert, support, and admin actions.
- Encryption in transit; encryption at rest for databases, backups, and object storage.
- Managed secrets; rotation and access auditing.
- Input validation, output encoding, CSRF protections where applicable, secure cookies, rate limiting, abuse controls.
- File upload allowlists, size limits, malware scanning, private-by-default storage, short-lived signed access URLs.
- Webhook signature validation, idempotency, replay protection, and reconciliation.
- Dependency and container scanning; pinned/reviewed dependencies; timely security updates.
- Centralized security events and alerting without logging secrets or unnecessary personal data.
- Tested backups and restoration; documented incident response.
- Secure development review and automated tests in CI.

## 3. Privileged access
No shared admin accounts. Separate support, compliance, finance, and infrastructure permissions. Require reason/ticket for exceptional access to private case files. Log actor, target, purpose, time, and outcome. Review privileged access periodically and revoke promptly.

## 4. Evidence and case confidentiality
Evidence is private by default and visible only to parties and authorized reviewers. Experts receive case-scoped, time-limited access. Prevent bulk export by default. Record access and downloads. Redact unrelated personal data before sharing where appropriate.

## 5. Incident response
Define severity levels, on-call/owner, containment, evidence preservation, legal notification assessment, affected-user communication, recovery, and post-incident review. Do not claim an incident is resolved until recovery and integrity checks pass.

## 6. Security testing
Before launch: threat-model review, dependency scan, static analysis, authorization tests, upload tests, webhook tests, backup restore test, and an independent penetration/security review proportionate to risk. High-risk findings block launch.

## 7. Secure failure behavior
Provider outages, invalid signatures, authorization ambiguity, inconsistent payment status, and insufficient evidence fail closed for consequential actions. Do not silently downgrade identity, payment, or expert-review assurance.
