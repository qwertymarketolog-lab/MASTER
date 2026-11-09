# MASTER

**MASTER** — modular digital platform for organizing professional services, matching customers with independent providers, coordinating work, recording evidence, and supporting transparent dispute handling.

The first pilot is planned for Lesnoy, Sverdlovsk Oblast, Russia. The platform is designed to support additional service domains over time without rewriting its core.

## Project status

**Stage:** foundation and requirements definition.  
**Implementation status:** no production application code yet.  
**Repository status:** documentation-first; architecture and legal/business assumptions must be reviewed before implementation.

Nothing in this repository should be interpreted as legal advice, a guarantee of compliance, or confirmation that a payment, identity, messaging, or verification provider is available for production use.

## Core principles

1. Define the product, operating model, legal roles, data model, and security boundaries before implementing product behavior.
2. Keep the platform core independent from individual service categories.
3. Separate Account, Person, Party, Organization, Membership, Role Assignment, and Representation.
4. Prefer replaceable provider adapters over provider-specific dependencies in the core.
5. Minimize personal data and do not store payment-card data or passport scans unless a documented, reviewed necessity exists.
6. Treat evidence, audit history, and explicit consent/confirmation as first-class product concepts.
7. Unknown or insufficient evidence must remain unknown; never fabricate verification or a dispute outcome.
8. Record architecture decisions. Analyze the impact of every material change before implementation.
9. Add new service domains through configuration and bounded modules wherever practical.
10. Start with a modular monolith; do not introduce distributed services without demonstrated operational need.
11. Build and release in small, reviewable changes with reproducible verification.

## Documentation map

- `SPECIFICATION.md` — product scope, actors, workflows, requirements, and non-goals.
- `ARCHITECTURE.md` — system boundaries, modules, integrations, and evolution strategy.
- `DATA_MODEL.md` — conceptual entities, data classification, ownership, and lifecycle.
- `IDENTITY_AND_ORGANIZATION_MODEL.md` — Account/Person/Party/Organization, Membership, authority, roles, and tenant boundaries.
- `GLOSSARY.md` — shared domain vocabulary and naming rules.
- `SECURITY.md` — security principles, threat boundaries, access control, and incident response.
- `DATA_PROTECTION.md` — personal-data governance and privacy-by-design requirements.
- `BUSINESS_AND_LEGAL_MODEL.md` — intended roles, service relationships, liability questions, and legal review gates.\n- `BUSINESS_VALIDATION_LAYER.md` — evidence-led business validation, customer discovery, revenue-model hypotheses, and pilot stop gates.
- `PAYMENT_MODEL.md` — payment flows, provider boundary, fees, refunds, and fiscalization questions.
- `ROADMAP.md` — staged delivery and explicit launch gates.
- `ARCHITECTURE_DECISIONS.md` — append-only decision register.
- `CHANGE_MANAGEMENT.md` — controlled change process and impact analysis.
- `REQUIREMENTS_TRACEABILITY.md` — requirement-to-design-to-test traceability.
- `THREAT_MODEL.md` — assets, threat scenarios, mitigations, and residual risks.

## Working status vocabulary

- **PROPOSED** — suggested, not approved.
- **ACCEPTED** — approved as a project decision, subject to stated conditions.
- **IMPLEMENTED** — present in the repository or deployed, with evidence.
- **VERIFIED** — acceptance criteria passed with recorded, reproducible evidence.
- **BLOCKED** — cannot proceed until a named dependency is resolved.
- **UNKNOWN / INCONCLUSIVE** — evidence is insufficient; do not convert to PASS.

A document describing a target design does not prove that the design is implemented or legally compliant.

## First release boundary

The initial pilot focuses on a small set of local home-service categories. Professional outsourcing, multi-provider projects, materials/logistics, AI assistance, and the three-expert review panel are planned capabilities; their detailed launch is gated by separate product, legal, operational, and safety reviews. Organization onboarding and delegated authority may be represented in the target model while remaining disabled in the pilot UI.

## Contribution and change control

Read `CHANGE_MANAGEMENT.md` before changing requirements or architecture. Material decisions must be recorded in `ARCHITECTURE_DECISIONS.md`. Do not weaken tests, controls, or acceptance criteria merely to obtain a green check.
