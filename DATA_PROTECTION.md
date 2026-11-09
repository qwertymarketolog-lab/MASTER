# MASTER — Personal Data Protection Baseline v1.0

**Status:** planning baseline, not legal advice or proof of compliance.

## 1. Principle
Minimize collection and processing, define purposes before collection, provide transparent notices, restrict access, set retention periods, and support applicable data-subject rights.

## 2. Legal role
Using VK ID, bank identity, MAX, or another intermediary does not automatically remove MASTER's operator obligations. If MASTER determines purposes and means of processing customer/provider profiles, addresses, requests, messages, or order history, it may be an operator under applicable law. Roles of processors and independent operators must be mapped contractually and factually.

## 3. Pre-launch legal review
Qualified Russian counsel must assess, at minimum:
- applicability of Federal Law 152-FZ and required notification/exception analysis;
- Russian database localization and all cross-border data flows;
- lawful basis, notices, consent where required, and withdrawal;
- operator/processor agreements and vendor due diligence;
- security measures and access logging;
- data-subject request handling;
- retention, deletion, anonymization, and backup expiry;
- messenger, identity, analytics, maps, storage, and support vendors;
- requirements of Federal Law 289-FZ on platform economy, if applicable to actual platform functions;
- consumer protection and service-category obligations.

## 4. Data inventory
Maintain a live inventory: data element, purpose, source, subject, legal basis, recipient, storage location, access, retention, deletion trigger, and vendor. Include Account identifiers, identity-link metadata, Person attributes, Party and Organization profiles, Membership and Representation records, role assignments, and authority-verification evidence as distinct data classes. No new field or integration enters production without an inventory update and review.

## 5. Privacy by design
- Do not collect passport scans by default.
- Do not store payment-card data.
- Do not request bank passwords or one-time banking codes.
- Keep identity-provider credentials with the provider.
- Store only the minimum verification result/reference needed.
- Separate public profile data from private operational and dispute data.
- Avoid precise location until needed for a specific service workflow.
- Do not use private case material for AI training by default.
- Provide clear user-facing controls and support route for access/correction/deletion requests.

## 6. Russian localization and vendor selection
For personal data of Russian citizens, assess primary collection, recording, systematization, accumulation, storage, updating, and retrieval requirements under current law. Choose Russian-hosted production databases/storage where required. A vendor's marketing claim is not sufficient; obtain architecture, subprocessor, location, security, and contract evidence.

## 7. Governance
Name the responsible operator and internal owner before launch. Maintain approved privacy policy, notices, processing records, vendor register, incident procedure, and periodic review schedule. Do not promise users that external login makes processing invisible to regulators or removes statutory duties.
