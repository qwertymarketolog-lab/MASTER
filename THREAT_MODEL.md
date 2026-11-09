# MASTER — Initial Threat Model

**Status:** initial qualitative model; must be revisited before pilot and after material changes.

## Assets
Accounts and sessions; contact/address data; provider status/qualification; orders and agreements; evidence files; private messages; payment references; expert case files; administrative credentials; API keys; backups; reputation and complaint records.

## Trust boundaries
Customer/provider devices ↔ public frontend; frontend ↔ backend; backend ↔ database; backend ↔ object storage; backend ↔ identity/payment/messaging vendors; support/admin ↔ private user data; expert ↔ assigned dispute case; CI/CD ↔ production secrets and deployment.

## Threat scenarios and controls
1. Account takeover: MFA for privileged accounts, secure sessions, anomaly detection, recovery safeguards.
2. IDOR / cross-account access: object-level authorization tests on every resource and role.
3. Fake provider status or qualification: source, timestamp, expiry, recheck, and explicit verified/unverified labels.
4. Payment webhook spoof/replay: signatures, idempotency, replay protection, reconciliation.
5. Evidence tampering/deletion: immutable metadata/audit, hashes, versioning, restricted storage, backup.
6. Malicious uploads: allowlists, limits, malware scanning, private storage, safe content delivery.
7. Insider misuse: least privilege, just-in-time case access, audit, access review.
8. Expert collusion/conflict: disclosures, recusals, independent assignment, separate opinions, audit.
9. Review manipulation: only eligible completed orders, abuse detection, appeal and moderation.
10. Data breach/vendor compromise: minimization, encryption, vendor due diligence, incident plan, access revocation.
11. Denial of service/scraping: rate limits, bot controls, quotas, monitoring.
12. AI prompt/data leakage or unsafe recommendation: isolate untrusted inputs, minimize context, no secrets, human review, uncertainty labels.
13. Supply-chain compromise: dependency pinning, scanning, review, protected CI, secret isolation.
14. Backup failure/ransomware: isolated backups, restore drills, recovery objectives.
15. Organization account takeover or shared credentials: individual human Accounts only, MFA for privileged users, no shared organization login, monitored recovery and account-linking controls.
16. Stale membership or representation after revocation: server-side authorization at action time, bounded cache/token lifetime, revocation events, and re-checks for delayed jobs.
17. Forged or over-broad representation: scoped authority evidence, independent verification proportionate to risk, explicit acting-Party confirmation, expiry/revocation, and no self-granted authority.
18. Cross-tenant leakage through search, cache, export, object storage, or background jobs: consistent Party ownership checks and isolation tests across every data path.
19. Privileged support misuse or unauthorized impersonation: no password impersonation, ticketed just-in-time access, narrow scope, audit, and periodic review.

## Required pre-launch work
Assign likelihood/impact and owner; document mitigations and residual risk; test authorization and upload controls; review vendors; exercise incident response; restore from backup; independently assess high-risk findings. An unassessed high-risk threat blocks launch.
