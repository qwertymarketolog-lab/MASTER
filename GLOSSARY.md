# MASTER — Ubiquitous Language Glossary

**Status:** proposed shared vocabulary. Definitions are product/domain language, not legal conclusions.

| Term | Definition | Must not be confused with |
|---|---|---|
| Account | Authenticated platform principal used to sign in and hold sessions. | Person, customer, provider, organization |
| Person | Human identity linked to one or more verified account access methods. | Account, Organization |
| Party | Participant acting in a request, agreement, order, project, or case. | Login identity |
| Individual Party | Party representing a natural person acting in their own name. | Individual Entrepreneur as a separate legal entity |
| Organization | Non-human organizational participant represented by authorized people. | Shared account or login |
| Membership | Relationship connecting a Person to an Organization with scoped platform participation. | Ownership or statutory signing authority |
| Role | Named bundle of candidate permissions within a defined scope. | Identity or unconditional access |
| Permission | Specific allowed action on a resource or operation. | Role label |
| Role Assignment | Grant of a Role to a Person/Party within an explicit scope and validity period. | Global account type |
| Representation | Recorded authority to act on behalf of another Party within a defined scope and period. | Membership |
| Tenant Scope | Security partition associated with an owning Party or explicit platform-global classification. | A client-supplied tenant_id |
| Resource Owner Party | Party to which a resource belongs for authorization and lifecycle purposes. | Person who uploaded a file |
| Identity Assurance | Evidence level supporting a claim about who controls an Account or is a Person. | Qualification, tax status, legal authority |
| Business/Tax Status | Time-bound legal or tax status of a provider, checked from an identified source. | Organization type or professional qualification |
| Qualification | Evidence that a person/provider meets a category-specific competence or credential requirement. | Identity verification |
| Acting Party | Party selected as the capacity in which a Person performs a particular action. | The Person's only or permanent role |
| Delegation | A scoped grant of authority from one Party to a representative. | Transfer of resource ownership |
| Platform Staff Role | Privilege for a staff/operational function, separately governed from customer/provider roles. | Organization membership |
| Authorization Context | Server-evaluated combination of actor, acting Party, action, resource, relationships, roles, attributes, and state. | Frontend button visibility |

## Language rules

1. Use **Account** only for authentication and session concepts.
2. Use **Person** for a human being and **Party** for a participant in a domain relationship.
3. Use **Organization** only for an organizational participant; an individual entrepreneur remains a natural person with a separately recorded business status unless legal review requires a more specific representation.
4. Use **Membership** for organizational participation and **Representation** for authority to act on behalf of another Party.
5. Use **Role** and **Permission** separately; never write “role = access to everything.”
6. Use **acting Party** whenever a person acts in a customer/provider/organization capacity.
7. Use **verified** only with a named claim, source, time, and verification scope. Identity, tax status, qualification, and authority are independent claims.
8. Preserve these definitions across API names, database schema, UI copy, event names, tests, and documentation. Any material vocabulary change requires impact analysis.
