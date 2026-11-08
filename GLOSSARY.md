# MASTER — Ubiquitous Language Glossary

**Status:** proposed shared vocabulary. Definitions are product/domain language, not legal conclusions.

| Term | Definition | Must not be confused with |
|---|---|---|
| Account | Authenticated platform principal used to sign in and hold sessions; type is HUMAN or SERVICE. | Person, customer, provider, organization |
| Person | Human identity linked to one or more verified account access methods. | Account, Organization |
| Party | Participant acting in a request, agreement, order, project, or case. | Login identity |
| Individual Party | Party representing a natural person acting in their own name. | Individual Entrepreneur as a separate legal entity |
| Organization | Non-human platform participant/container represented by authorized people; OrganizationType does not itself determine legal personality. | Shared account or login |
| OrganizationType | Operating form of an Organization: LEGAL_ENTITY, SOLE_PROPRIETOR, TEAM, or OTHER. | Party kind or legal conclusion |
| TaxStatus | Time-bound legal/tax status claim attached to the relevant Party and checked for a transaction context. | OrganizationType, qualification |
| ActingParty | Normalized tuple `(Party, optional Representation)` identifying the capacity used for an action. | Actor Person alone |
| ActingPartySnapshot | Immutable event-time snapshot of actor, Party, authority/role references, and relevant claim versions. | Mutable current profile |
| VerificationProfile | Set of independently verified claims and levels for a Person or Organization. | One universal trust score |
| ContactPoint | Independently managed email, phone, or other contact/bank endpoint with verification and privacy metadata. | Person identity |
| Service Account | Non-human Account used by a controlled integration or automation. | Human Person or shared organization login |
| Tombstone | Minimal retained historical reference after lawful pseudonymization/deletion of identifying data. | Full personal profile |
| Membership | Relationship connecting a Person to an Organization with scoped platform participation. | Ownership or statutory signing authority |
| Role | Named bundle of candidate permissions within a defined scope. | Identity or unconditional access |
| Permission | Specific allowed action on a resource or operation. | Role label |
| Role Assignment | Grant of a Role to a Person/Party within an explicit scope and validity period. | Global account type |
| Representation | Recorded authority to act on behalf of another Party within a defined scope and period. | Membership |
| Tenant Scope | Security partition associated with an owning Party or explicit platform-global classification. | A client-supplied tenant_id |
| Resource Owner Party | Party to which a resource belongs for authorization and lifecycle purposes. | Person who uploaded a file |
| Verification Level | Claim-specific level such as none, email, phone, identity, business, or bank-account verification; dimensions are independent, not one ordinal score. | Universal trust score, qualification |
| Identity Assurance | Evidence supporting a claim about who controls an Account or is a Person. | Qualification, tax status, legal authority |
| Business/Tax Status | Time-bound legal or tax status of a provider, checked from an identified source. | Organization type or professional qualification |
| Qualification | Evidence that a person/provider meets a category-specific competence or credential requirement. | Identity verification |
| Acting Party | Party selected as the capacity in which a Person performs a particular action. | The Person's only or permanent role |
| Delegation | A scoped grant of authority from one Party to a representative. | Transfer of resource ownership |
| Platform Staff Role | Privilege for a staff/operational function, separately governed from customer/provider roles. | Organization membership |
| Authorization Context | Server-evaluated combination of actor, acting Party, action, resource, relationships, roles, attributes, and state. | Frontend button visibility |

## Language rules

1. Use **Account** only for authentication and session concepts.
2. Use **Person** for a human being and **Party** for a participant in a domain relationship.
3. Use **Organization** for a legal entity or an explicitly modeled business/team workspace. Organization(SOLE_PROPRIETOR) and Organization(TEAM) are platform containers, not automatic claims of separate legal personality. An individual entrepreneur acting alone may use an Individual Party.
4. Use **Membership** for organizational participation and **Representation** for authority to act on behalf of another Party.
5. Use **Role** and **Permission** separately; never write “role = access to everything.”
6. Use **ActingParty** as `(Party, optional Representation)` for consequential actions; persist an immutable event-time ActingPartySnapshot.
7. Use **verified** only with a named claim, source, time, and verification scope. Verification levels are claim-specific and non-substitutable. Identity, tax status, qualification, and authority are independent claims.
8. Preserve these definitions across API names, database schema, UI copy, event names, tests, and documentation. Any material vocabulary change requires impact analysis.


## Additional modeling rules

- A Person and their ContactPoints are separate records; email/phone/bank endpoints can change and have independent verification, access, and retention rules.
- A Service Account is a non-human Account and does not require a Person. It receives narrowly scoped machine permissions and explicit owner/operator attribution.
- Historical ActingPartySnapshots must survive lawful Person pseudonymization through a minimal tombstone/reference where retention is legally justified; deletion and retention policy must be approved per data category.
- MVP permits one active Account per Person. Account merging is a later, separately reviewed capability.
