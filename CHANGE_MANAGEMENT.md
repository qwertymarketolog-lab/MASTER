# MASTER — Change Management

## 1. Rule
No material requirement, architecture, data, API, security, payment, or legal-model change is implemented without a documented change proposal and impact analysis.

## 2. Change workflow
1. Identify the request, problem, expected outcome, and acceptance criteria.
2. Inspect current docs, code, issues, branches, and existing decisions.
3. Identify affected modules, data entities, APIs, integrations, users, security controls, legal obligations, operations, tests, and documentation.
4. Compare alternatives, including no change.
5. Assess compatibility, migration, rollback, and data repair.
6. Update or create an ADR when a material architectural decision changes.
7. Obtain required product, technical, security, and legal approval.
8. Implement in a scoped branch and reviewable PR.
9. Run focused tests, contract tests, security checks, and proportionate end-to-end verification.
10. Verify CI from the actual commit and inspect complete run/job/artifact evidence where applicable.
11. Update traceability and release notes; merge only after acceptance criteria pass.

## 3. Impact checklist
- Product behavior and user journeys
- Module ownership and dependencies
- Data schema, classification, retention, migration, deletion
- API contracts and external consumers
- Authentication, authorization, privacy, threat model
- Payment, fiscalization, refunds, fees
- Legal role, contracts, notices, regulatory duties
- Provider contracts, API limits, outage behavior
- Accessibility, localization, support and operations
- Tests, CI, deployment, monitoring, backup/restore
- Backward compatibility and rollback

## 4. Repository workflow
- Protect main; work in short-lived feature/docs branches.
- One PR per coherent change; do not create duplicate PRs for the same work.
- Never overwrite unrelated user changes.
- Keep PRs small and explain scope, risks, evidence, and rollback.
- Do not weaken tests, lint rules, branch protections, or security controls to obtain a green result.
- Do not merge solely because a subset of checks passed.
- Unknown or inconclusive critical evidence means HOLD.

## 5. Engineering safeguards
- Inspect repository instructions and git status before editing.
- Keep line-length/formatting rules explicit in tool configuration; do not treat 100 characters as a universal language limit.
- Validate fixture syntax and encoding; distinguish test-data errors from product failures.
- Pin and review dependencies; avoid broad unbounded upgrades.
- Keep secrets and real personal data out of commits, fixtures, logs, and screenshots.
- Use deterministic tests and stable test identifiers.
- Migrations must be tested on representative data and have rollback/forward recovery plans.
- Verify the final integrated commit, not just local or pre-merge results.

## 6. Decision records
If a change supersedes an ADR, append a new ADR and mark the old one SUPERSEDED with a link. Do not erase the historical rationale.
