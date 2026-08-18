# Chapter 12 — Preservation

> **Evidence is preserved only when its bytes, integrity, historical meaning, dependencies, and governed availability can survive the systems that created it.**

## Purpose

This chapter defines the architectural specification for TrustAgentAI **Preservation**, **Preservation Services**, **Preservation Evidence (PE)**, and long-term support of the **Verification Dependency Graph (VDG)**.

Preservation maintains Accountability Evidence and the material required to interpret it across the Evidence Lifetime. It addresses storage integrity, retention, availability, custody, encryption continuity, Write Once, Read Many controls, Legal Hold, recovery, migration, renewal, disposition, and documented erasure.

This chapter establishes:

- the distinction between storage, backup, replication, and Preservation;
- Evidence Lifetime and preservation-scope semantics;
- Preservation Plans, retention rules, and lifecycle states;
- preservation of canonical Protocol Objects and Verification Dependencies;
- immutability, WORM, object-lock, custody, and access-control behavior;
- encryption, key separation, recovery, and long-term decryptability;
- integrity monitoring, fixity checking, restoration, and disaster recovery;
- Legal Hold, disposition, deletion, erasure, and deletion evidence;
- archival, provider, representation, and cryptographic migration;
- Preservation Evidence, manifests, checkpoints, monitoring, and Verification;
- privacy, portability, and independent assurance;
- Preservation invariants and architectural requirements.

This chapter applies the common Protocol Object rules defined in [06-Protocol-Objects.md](06-Protocol-Objects.md).

It builds upon the Evidence Record model in [07-Evidence-Record-Specification.md](07-Evidence-Record-Specification.md), the Hash Chain model in [08-Hash-Chain-Specification.md](08-Hash-Chain-Specification.md), the Witness model in [09-Witness-Observation-Specification.md](09-Witness-Observation-Specification.md), the Checkpoint model in [10-Checkpoints-and-External-Anchors.md](10-Checkpoints-and-External-Anchors.md), and the Historical Key State model in [11-Key-Transparency.md](11-Key-Transparency.md).

It also applies the system model in [05-System-Overview.md](05-System-Overview.md), the principles in [04-Design-Principles.md](04-Design-Principles.md), and the canonical terms in [Terminology.md](Terminology.md) and [Acronyms.md](Acronyms.md).

This chapter defines architectural semantics.

It does not require one cloud provider, storage medium, archive format, database, blockchain, WORM product, encryption technology, legal-hold system, or archival institution. It does not define final field names, concrete schemas, media types, wire encodings, storage APIs, jurisdiction-specific retention periods, or legal conclusions.

Those details belong to TAIP, Trust Profiles, preservation profiles, governed policies, cryptographic profiles, storage bindings, APIs, and applicable law.

---

# 12.1 Preservation Definition

**Preservation** is the governed process of maintaining Accountability Evidence and required Verification Dependencies so that defined Accountability Claims can remain interpretable and verifiable across the applicable Evidence Lifetime.

Preservation combines:

- retained canonical bytes or governed successor representations;
- identity and integrity protection;
- historical semantic context;
- dependency availability;
- custody and lifecycle evidence;
- access and recovery capability;
- accountable migration or renewal;
- explicit disposition or erasure.

```text
Retained Bytes
      +
Historical Meaning
      +
Verification Dependencies
      +
Governed Availability
      =
Preservable Future Verification
```

Preservation is a protocol-supported assurance property, not a claim created merely by placing data in storage.

---

# 12.2 Evidence Lifetime

The **Evidence Lifetime** is the period during which defined evidence and dependencies may be required to support interpretation, Verification, dispute resolution, audit, investigation, or another governed accountability purpose.

The Evidence Lifetime may exceed the Operational Lifetime of the systems that created the evidence.

```text
Operational System Lifetime
        │
        └───────────────┐
                        ▼
Evidence Lifetime ─────────────────────────────►
```

During that interval:

- software and schemas change;
- keys rotate and algorithms weaken;
- Organizations merge or close;
- storage providers and formats disappear;
- Policy and Trust Profiles evolve;
- legal duties change;
- original operators may become unavailable or adversarial.

Preservation planning must address these transitions explicitly.

---

# 12.3 Scope

This chapter applies to Preservation of:

- canonical Protocol Objects;
- Evidence Records and externalized payloads;
- Chain Entries and Hash Chain material;
- Witness Observations and quorum evidence;
- Checkpoints and Anchor Evidence;
- Key Transparency Records and Historical Key State;
- Preservation Evidence and Migration Records;
- Dispute Pack Manifests and Verification Reports;
- schemas, canonicalization rules, algorithms, registries, Trust Profiles, Policy, Authority, and extension definitions;
- indexes, manifests, locators, and proof material necessary to resolve the Verification Dependency Graph.

The Preservation scope may be object-specific, claim-specific, tenant-specific, jurisdiction-specific, or profile-specific. It must remain explicit.

---

# 12.4 Security and Assurance Objectives

Preservation has six primary objectives:

1. prevent undetected alteration, substitution, truncation, or unauthorized deletion;
2. retain enough historical semantics to interpret evidence correctly;
3. maintain governed availability and recovery across the required lifetime;
4. preserve custody, retention, hold, migration, and disposition accountability;
5. reduce dependence upon one producer, product, provider, or operational system;
6. make preservation gaps and assurance degradation visible.

The architectural objective is:

> A later verifier can identify what was preserved, under which rules, by whom, for how long, with which dependencies, through which migrations, and with which unresolved limitations.

Achievement depends upon both cryptographic and operational controls.

---

# 12.5 What Preservation Establishes

Subject to successful Verification, Preservation Evidence may establish that:

- identified objects or dependencies entered a governed preservation scope;
- defined integrity commitments matched at one or more historical checks;
- retention, object-lock, Legal Hold, replication, or custody controls were represented as active;
- recovery or restoration tests succeeded under defined conditions;
- a migration preserved a verifiable relationship between source and target state;
- a deletion or erasure action occurred under an accountable rule;
- a Preservation Service issued a defined attestation or status record;
- required material remained available at a supported boundary;
- a preservation gap, failure, or exception was detected and recorded.

Each conclusion is bounded by the evidence type, observation scope, control domain, and Verification Context.

---

# 12.6 What Preservation Does Not Establish

Preservation does not by itself prove:

- truth of the preserved claims;
- valid Authority or Policy for the original Accountable Action;
- completeness of all evidence relevant to every possible dispute;
- that a WORM provider cannot fail, collude, or misconfigure controls;
- independent custody merely because multiple replicas exist;
- continuing decryptability unless encryption dependencies are preserved;
- availability at every instant within a retention interval;
- successful recovery merely because backup jobs reported success;
- lawful retention, disclosure, Legal Hold, or erasure in every jurisdiction;
- absence of an undisclosed copy after deletion;
- Legal Validity or Regulatory Compliance.

```text
Stored
≠
Preserved
≠
Valid
≠
Complete
≠
Legally Sufficient
```

---

# 12.7 Conceptual Roles

A Preservation architecture may involve:

- an **Evidence Owner** or accountable custodian defining legitimate preservation purpose;
- a **Preservation Authority** approving preservation policy and high-impact lifecycle actions;
- a **Preservation Service** retaining evidence and dependencies;
- a **Custodian** responsible for controlled possession or administration;
- a **Retention Administrator** configuring retention and disposition rules;
- a **Legal Hold Authority** activating or releasing holds under applicable governance;
- a **Key Custodian** maintaining encryption and recovery keys;
- a **Migration Authority** approving archival or cryptographic transitions;
- a **Monitor or Auditor** evaluating preservation state;
- a **Resolver** locating preserved objects and dependencies;
- a **Verification Engine** evaluating Preservation Evidence;
- a **Dispute Pack Assembler** exporting required material.

Role combination is permitted where a Trust Profile allows it. Combined control must remain visible.

---

# 12.8 Preservation Service

A **Preservation Service** is the logical role responsible for durable retention of Accountability Evidence and required Verification Dependencies.

Its responsibilities may include:

- authenticated ingestion;
- canonical object retention;
- retention and hold enforcement;
- encryption and access control;
- integrity monitoring;
- replication and recovery;
- dependency resolution;
- lifecycle and custody evidence;
- migration and renewal;
- portable export;
- accountable disposition.

A Preservation Service does not establish validity merely by retaining an object.

One service may preserve bytes while other services preserve schemas, Historical Key State, External Anchor proofs, or governance material. Distributed Preservation is conforming when the resulting dependency graph remains discoverable and sufficiently available.

---

# 12.9 Preservation Evidence Definition

**Preservation Evidence (PE)** is evidence supporting claims concerning durable retention, integrity, custody, archival state, recovery, migration, availability, hold, deletion, erasure, or future interpretability.

PE may identify or bind to:

- evidence type and version;
- Preservation Service and responsible Authority;
- preserved object or collection identifiers;
- integrity commitments;
- preservation scope and policy;
- retention and hold state;
- storage or archival class;
- custody and control domain;
- encryption context;
- check, recovery, migration, or disposition result;
- applicable times;
- predecessor and related evidence;
- cryptographic protection.

Operational telemetry becomes Preservation Evidence only when its semantics, provenance, integrity, and Verification behavior are governed.

---

# 12.10 Preservation Target and Scope

A **Preservation Target** is the exact object, collection, dependency, commitment, range, or governed state to which Preservation Evidence applies.

Target scope may be defined through:

- Protocol Object identifiers;
- content digests;
- manifest membership;
- Chain ranges or Checkpoints;
- Key Transparency checkpoints;
- registry or profile versions;
- named dependency collections;
- storage-object versions;
- another deterministic TAIP-defined selector.

The target must be bounded and reproducible.

A statement that `the archive is healthy` is insufficient when it does not identify which objects, versions, dependencies, time boundary, and verification method were evaluated.

---

# 12.11 Preservation Plan

A **Preservation Plan** defines how a preservation scope will remain verifiable across its Evidence Lifetime.

The plan should address:

- Accountability Claims and Trust Profiles in scope;
- object and dependency inventory;
- Evidence Lifetime and retention triggers;
- integrity and immutability controls;
- encryption and key continuity;
- custody and access governance;
- replication, independence, and recovery;
- monitoring and test cadence;
- migration and renewal thresholds;
- Legal Hold and exception behavior;
- disposition and erasure;
- provider exit and portable export;
- evidence produced by each control.

The plan is governed state. Material changes to it may themselves require Accountability Evidence.

---

# 12.12 Retention Policy

A **Retention Policy** defines the rules determining how long evidence remains within a preservation scope and which actions are permitted during that interval.

It may bind:

- policy identifier and version;
- governed evidence classes;
- retention duration or event-based rule;
- start and end triggers;
- minimum and maximum periods;
- jurisdiction or contractual context;
- hold override behavior;
- review and disposition requirements;
- authorized exceptions;
- responsible Authority;
- effective boundaries.

The applicable historical policy must remain resolvable. Replacing a policy document must not silently change the retention meaning of previously governed evidence.

TAIP can represent retention evidence but does not determine the legally required period.

---

# 12.13 Evidence Classification

Preservation controls may depend upon an evidence classification.

Classification dimensions may include:

- Protocol Object type;
- Accountability Claim or business process;
- Intended Trust Profile;
- sensitivity and disclosure restrictions;
- legal or contractual category;
- retention class;
- criticality and recovery priority;
- jurisdiction;
- encryption and custody requirements;
- required preservation independence.

Classification must be accountable and historically versioned where it changes lifecycle behavior.

An implementation must not use an informal storage tag as the sole source of a high-impact retention or deletion decision unless its namespace, Authority, history, and integrity are governed.

---

# 12.14 Preservation Lifecycle Model

Preservation lifecycle states may include:

- selected or scheduled;
- submitted;
- accepted;
- retained;
- integrity-verified;
- replicated;
- held;
- access-restricted;
- degraded;
- recovery-tested;
- migrated;
- pending disposition;
- deleted or erased;
- failed or indeterminate.

```text
Accepted
≠
Retained
≠
Integrity-Verified
≠
Recoverable
≠
Preserved for every required claim
```

Not every implementation uses every state. TAIP and the applicable profile define permitted transitions, responsible roles, required evidence, and terminal behavior.

---

# 12.15 Ingestion and Acceptance

Preservation ingestion transfers or references a target into a governed preservation workflow.

Acceptance should verify:

- target identity and integrity;
- object type and version;
- preservation scope and classification;
- applicable policy and Authority;
- expected dependencies;
- encryption and access requirements;
- retention or Legal Hold state;
- duplicate and conflict behavior;
- initial custody boundary;
- receipt semantics.

An ingestion receipt proves only the receipt's defined claim. It does not by itself prove durable retention, WORM enforcement, replication, recovery, or future interpretability.

Rejected, quarantined, incomplete, and pending targets must remain distinguishable from accepted preserved state.

---

# 12.16 Canonical Identity and Integrity

Preservation must retain the relationship between a Protocol Object's stable identifier, canonical protected representation, cryptographic digest, and any storage representation.

The archive may compress, encrypt, package, shard, or migrate stored bytes, but it must preserve enough evidence to recover or verify the canonical object defined by its applicable historical rules.

```text
Storage Object Identifier
≠
Protocol Object Identifier
≠
Canonical Object Digest
```

Database row IDs, bucket paths, filenames, tape positions, or provider version IDs are useful locators. They are not automatically protocol identity or integrity evidence.

If multiple objects reuse an identifier with conflicting protected content, the Preservation Service must preserve and surface the conflict rather than overwrite one version silently.

---

# 12.17 Verification Dependency Graph

The **Verification Dependency Graph (VDG)** identifies the evidence and material required to interpret and verify defined claims.

For a focal object, dependencies may include:

- schema and canonicalization definitions;
- algorithm and cryptographic-suite specifications;
- Protocol Identity and Historical Key State;
- Authority and Policy evidence;
- registries and Trust Profiles;
- Chain Entries, Witness Observations, and Checkpoints;
- Anchor Evidence and external proof material;
- related causal or execution evidence;
- extension definitions;
- migration and Preservation Evidence.

Preservation must consider the VDG rather than retain only the focal Evidence Record.

Loss of a mandatory dependency may make unchanged bytes Unsupported, Incomplete, or Indeterminate.

---

# 12.18 Dependency Inventory and Closure

A preservation inventory should identify which VDG dependencies are:

- embedded;
- preserved in the same service;
- resolved through another Preservation Service;
- maintained by an external registry or authority;
- derivable from a stable public specification;
- restricted or selectively disclosed;
- unavailable, unsupported, or intentionally omitted.

**Dependency closure** is the bounded set of dependencies required to evaluate the claims and Verification Context in scope. It does not mean copying every related object ever produced.

The closure algorithm must terminate safely, preserve typed references, detect cycles, and enforce governed depth and size limits.

A mutable URL without identity, integrity, version, and fallback semantics is insufficient for a mandatory long-term dependency.

---

# 12.19 Schemas and Canonicalization Rules

Long-term Verification requires the historical schema and canonicalization rules applicable to each preserved object.

Preservation should retain or durably resolve:

- schema identifier and version;
- authoritative schema content;
- canonicalization algorithm and version;
- field-order, number, string, time, and binary encoding rules;
- validation vocabulary and referenced schemas;
- critical-extension behavior;
- test vectors or conformance material where required.

Applying the newest schema or serializer to an old object may change its meaning or signed input.

Migration to a new representation must preserve the original canonical object or create explicit Migration Evidence binding the source, transformation, result, and limitations.

---

# 12.20 Cryptographic Algorithms and Specifications

Preservation must retain the cryptographic context required to evaluate historical integrity and Signatures.

That context may include:

- algorithm identifiers;
- parameter sets;
- public keys and certificate material;
- digest and Signature encodings;
- canonical input construction;
- validation constraints;
- algorithm status at relevant boundaries;
- implementation-independent specifications;
- renewal and migration evidence.

An identifier that resolves only through a discontinued proprietary product is a preservation risk.

Algorithm deprecation at Verification Time does not silently rewrite the historical cryptographic fact. The applicable Trust Profile determines whether historical acceptance, warning, renewal evidence, or failure applies.

---

# 12.21 Identity and Historical Key State

Preserved Signatures remain interpretable only when applicable Protocol Identity, Key Identifier, Key Purpose, and Historical Key State can be resolved.

Preservation may therefore include:

- Key Transparency Records;
- key registrations and lifecycle transitions;
- KT Checkpoints and proofs;
- identity-method and namespace definitions;
- historical resolver or registry snapshots;
- delegated signing relationships;
- compromise and recovery evidence;
- algorithm and custody context.

Current identity or key configuration must not replace historical state.

Preserving a public key alone is insufficient when the verifier cannot determine whose key it was, which purpose applied, or whether it was active at the relevant boundary.

---

# 12.22 Authority, Policy, Registry, and Trust Profile State

Authority and Policy context may change during the Evidence Lifetime.

Preservation should retain or resolve the historical versions necessary to determine:

- who possessed Authority;
- delegation and revocation state;
- which Policy governed the action;
- which registry entries and eligibility rules applied;
- which Trust Profile was intended;
- which controls were required;
- which exceptions or approvals existed;
- how achieved assurance must be calculated.

A current role directory or policy page is not a substitute for historical governance evidence.

The Preservation Service maintains the evidence. It does not independently create the business Authority or correctness of the preserved policy.

---

# 12.23 Chains, Witnesses, Checkpoints, and Anchors

Historical-integrity evidence must remain available together with the rules required to verify it.

Preservation may include:

- Chain Entries and range manifests;
- Chain Heads and predecessor relationships;
- Witness Observations and eligibility evidence;
- quorum definitions and Registry snapshots;
- Checkpoints and target-state dependencies;
- External Anchor evidence;
- external transaction, publication, or proof material;
- consistency and inclusion proofs;
- relevant time and finality semantics.

A retained Checkpoint without the committed object or membership proof may detect that material is missing but may not enable full Verification.

External Anchor identifiers must remain interpretable even if the original network interface or explorer disappears.

---

# 12.24 Write Once, Read Many Controls

**Write Once, Read Many (WORM)** is a storage property or mechanism intended to restrict modification or deletion after data is written.

WORM controls may be implemented through:

- hardware or media characteristics;
- provider-enforced object lock;
- immutable storage generations;
- append-only systems;
- regulated archival services;
- cryptographically committed history;
- another governed mechanism.

WORM can strengthen resistance to administrative alteration and premature deletion.

It does not by itself establish object validity, correct retention configuration, dependency preservation, independence, availability, recovery, or lawful disposition.

```text
WORM Storage
≠
Complete Preservation
```

---

# 12.25 Immutability and Object Lock

Immutability controls must identify their exact scope and enforcement semantics.

Relevant distinctions include:

- governance-mode and compliance-mode lock;
- time-bounded and indefinite lock;
- object version and current object name;
- provider control and customer control;
- retention lock and Legal Hold;
- protected bytes and mutable metadata;
- administrative override and non-overridable enforcement.

An object lock applied after an unprotected interval must not be represented as continuous protection from original creation.

Metadata that affects interpretation, access, retention, or object resolution requires protection appropriate to its role. Immutable payload bytes do not prevent a mutable index from redirecting the verifier to different content.

---

# 12.26 Replication and Independence

Replication creates multiple copies to improve durability and availability.

Independence reduces correlated control, compromise, censorship, or failure.

```text
Three replicas in one administrative account
≠
Three independent Preservation Services
```

Independence may consider:

- organizational ownership;
- administrative accounts;
- infrastructure and cloud providers;
- key custody;
- deployment pipelines;
- billing and economic control;
- jurisdiction;
- software and format diversity;
- recovery authority.

Trust Profiles determine which independence properties are required. Replication count alone must not be used as an independence claim.

---

# 12.27 Durability and Availability

**Durability** concerns the probability that retained state remains uncorrupted and unrecoverably lost. **Availability** concerns the ability of an authorized verifier or process to retrieve required state when needed.

The two are distinct.

```text
Durable but offline archive
≠
Currently available evidence
```

Preservation objectives may define:

- maximum tolerable data loss;
- recovery-time expectation;
- retrieval latency by archival tier;
- replica or erasure-coding policy;
- regional or domain diversity;
- integrity-check cadence;
- outage and exception reporting;
- evidence-access continuity after provider exit.

Marketing durability percentages are not Preservation Evidence unless their scope, period, observed controls, and relationship to the target are governed.

---

# 12.28 Custody

**Custody** describes accountable possession, administration, control, or stewardship of preserved evidence and its critical dependencies.

Custody evidence may identify:

- responsible Organization and service;
- physical or logical location;
- administrative control domain;
- encryption-key custodian;
- transfer, receipt, and release events;
- access and handling restrictions;
- archival tier;
- applicable policy and jurisdiction;
- separation-of-duty controls;
- custody exceptions or incidents.

Custody does not establish truth or ownership merely because a party holds a copy.

When multiple services share responsibility, the boundaries between byte custody, key custody, policy Authority, and Verification access must remain explicit.

---

# 12.29 Chain of Custody

A **Chain of Custody** is the accountable history of material transfers, handling, administrative control, and relevant access across the Evidence Lifetime.

A custody event should bind:

- source and destination custodians;
- preservation target;
- transfer or access action;
- time semantics;
- integrity values before and after transfer;
- encryption or packaging state;
- authorizing evidence;
- exceptions and verification results;
- receipt or rejection status.

Chain of Custody is not the same as a TrustAgentAI Hash Chain, though custody records may themselves be committed to one.

An undocumented gap does not automatically prove alteration, but it weakens or makes indeterminate the custody claim that depends upon continuous accountable handling.

---

# 12.30 Encryption at Rest

Preserved evidence may be encrypted to protect confidentiality during storage, replication, backup, and migration.

The encryption design must define:

- protected plaintext scope;
- authenticated encryption or integrity behavior;
- algorithm and parameter identifiers;
- key identifiers and versions;
- nonce or initialization-vector semantics;
- associated data;
- envelope and metadata coverage;
- authorized decryption roles;
- recovery and rotation behavior;
- long-term algorithm migration;
- the effect of missing keys on Verification.

Encryption does not replace canonicalization, Signatures, retention controls, access governance, data minimization, or Preservation Evidence.

The inability to decrypt mandatory evidence must remain visible.

---

# 12.31 Envelope Encryption and Key Separation

Envelope encryption commonly protects each object or collection with a data-encryption key and protects that key with one or more key-encryption keys.

```text
Canonical Evidence
       │ encrypted by
       ▼
Data-Encryption Key
       │ wrapped by
       ▼
Key-Encryption Key
```

The architecture should separate:

- evidence custody;
- ciphertext storage;
- data-encryption keys;
- key-encryption keys;
- recovery material;
- decryption Authority;
- key-management administration.

Separation can reduce unilateral disclosure and support controlled cryptographic erasure. It must not create a dependency graph so opaque that future authorized decryption becomes impossible.

Key wrapping, rewrapping, and custody changes require accountable evidence when they affect future interpretability or access.

---

# 12.32 Encryption Continuity and Key Lifecycle

Encryption continuity means maintaining authorized decryptability for the required Evidence Lifetime while controlling disclosure risk.

The plan should address:

- historical encryption algorithms and parameters;
- key identifiers and versions;
- rotation and rewrapping;
- backup and recovery keys;
- threshold or escrow arrangements where permitted;
- custodian succession;
- compromise response;
- lost-key detection;
- algorithm migration;
- authorized destruction.

Encryption-key rotation must not make older evidence inaccessible unless governed erasure is intended.

Current KMS configuration is not sufficient historical evidence when aliases, versions, providers, or access rules change. The necessary key metadata and lifecycle context must remain preserved without exposing secret key material publicly.

---

# 12.33 Access Control and Access Evidence

Preservation requires controlled availability, not unrestricted public access.

Access governance may consider:

- requester identity;
- Authority and purpose;
- object sensitivity;
- jurisdiction and contractual restrictions;
- Legal Hold and investigation context;
- least privilege;
- separation of duties;
- time-limited authorization;
- emergency or break-glass access;
- disclosure and export scope.

Accountability-relevant access, denial, privilege change, or administrative override may create Access Evidence.

An access log maintained solely by the same administrator it is intended to constrain provides limited independent assurance unless protected through additional commitments, Witnessing, or custody controls.

Access-control metadata affecting future availability must itself be preserved appropriately.

---

# 12.34 Integrity Monitoring

Integrity monitoring periodically or eventfully evaluates whether preserved state still matches authenticated commitments.

Monitoring may verify:

- canonical object digests;
- storage-object checksums;
- manifest membership;
- Signature validity;
- Chain continuity;
- Checkpoint or Anchor relationships;
- replica agreement;
- encryption-envelope integrity;
- dependency resolution;
- policy and lock configuration;
- unauthorized version or metadata change.

The monitoring scope, algorithm, cadence, sample method, target population, and result must remain explicit.

A successful sample does not prove the integrity of untested objects. A successful byte-level check does not prove semantic interpretability or dependency availability.

---

# 12.35 Fixity, Scrubbing, and Repair

**Fixity** is the property that a retained bitstream matches a defined integrity value. **Scrubbing** is the process of reading, checking, and where possible repairing stored state using trusted redundancy or source evidence.

Repair must preserve accountability.

A repair event should identify:

- detected corruption or loss;
- affected target and replica;
- expected and observed integrity values;
- trusted repair source;
- repair method;
- resulting value;
- responsible role;
- time and validation result;
- any unresolved uncertainty.

Repairing one corrupted replica from a verified copy need not change the canonical Protocol Object. Reconstructing missing or transformed evidence without a verified source is not ordinary repair and must not be represented as exact restoration.

---

# 12.36 Backup and Preservation Separation

A backup is a copy intended primarily for restoration after operational loss or corruption.

Preservation has broader requirements, including historical meaning, retention governance, custody, dependency availability, migration, and future independent Verification.

```text
Successful Backup Job
≠
Recoverable Backup
≠
Preserved Evidence
```

Backups may support Preservation when their identity, integrity, scope, retention, encryption, recovery, and lifecycle semantics are governed.

Short-lived rolling backups can silently expire before the Evidence Lifetime. Application-consistent backups can still omit external Verification Dependencies. Provider snapshots can be durable while remaining inaccessible after account closure.

The Preservation Plan must account for these limitations.

---

# 12.37 Restoration and Recovery Testing

Restoration returns preserved state to a form accessible for authorized use or Verification.

Recovery testing should evaluate more than file retrieval. It may verify:

- object identity and integrity;
- decryption capability;
- canonical representation recovery;
- manifest and index reconstruction;
- dependency resolution;
- schema and algorithm availability;
- access and Authority controls;
- portable Verification using a conforming engine;
- required recovery time;
- independence from the failed operational system.

A test must identify its sample scope, environment, dependencies, exclusions, and outcome.

One successful restore does not prove that every object is recoverable. Untested or failed classes must remain visible.

---

# 12.38 Disaster Recovery and Continuity

Preservation planning must consider failures that affect multiple components or Control Domains.

Relevant scenarios include:

- provider or region outage;
- account takeover;
- ransomware;
- credential loss;
- malicious administration;
- data-center destruction;
- key-management failure;
- software or format obsolescence;
- organizational dissolution;
- legal or geopolitical access restriction;
- correlated corruption across replicas.

The plan should define recovery priorities, alternate custodians, offline or cross-domain copies, key recovery, authority succession, communication, testing cadence, and evidence created during response.

Operational recovery must not silently rewrite committed historical evidence or erase the evidence of the failure.

---

# 12.39 Archival Media and Formats

Preserved representations must remain readable and interpretable despite media, format, and tooling obsolescence.

Archival planning may consider:

- open and documented formats;
- deterministic encodings;
- media lifetime and environmental controls;
- format and codec dependencies;
- reader hardware and software;
- checksums and error correction;
- periodic media refresh;
- geographic and administrative diversity;
- human-readable documentation;
- reference implementations and test vectors.

A proprietary archive format is not prohibited, but mandatory future interpretation must not depend upon undocumented behavior or a single unavailable vendor.

Media refresh copies bits to viable media. Format migration changes representation. Their evidence and risks differ.

---

# 12.40 Retention Calculation and Expiry

Retention may be calculated from:

- object creation or finalization;
- Commitment Time;
- completion of an Accountable Action;
- contract or account closure;
- a regulatory or business event;
- supersession or migration;
- an explicit fixed date;
- another governed trigger.

The trigger, time semantics, calendar rules, duration, policy version, and applicable overrides must be deterministic.

```text
Retention Expiry
≠
Immediate Automatic Deletion
```

Expiry normally makes evidence eligible for a governed disposition decision. Legal Hold, investigation, dependency relationships, contractual restrictions, or an authorized exception may prevent deletion.

Clock error or missing trigger evidence must produce explicit uncertainty rather than an invented expiry date.

---

# 12.41 Legal Hold

A **Legal Hold (LH)** is a preservation state or policy preventing normal disposition because evidence may be required for litigation, investigation, audit, regulatory review, or another formal process.

A Legal Hold record may bind:

- hold identifier and version;
- issuing Authority;
- target scope and selection method;
- reason category or protected reference;
- activation boundary;
- notification state;
- access restrictions;
- superseding holds;
- release Authority and boundary;
- applicable Policy.

Legal Hold must be distinguishable from ordinary retention and WORM lock.

TAIP defines accountable semantics, not jurisdiction-specific legal sufficiency. Implementations must apply applicable law and authorized legal governance.

---

# 12.42 Disposition Review

**Disposition** is the governed decision and process determining what happens when evidence becomes eligible to leave active retention.

Possible outcomes include:

- extend retention;
- place or maintain Legal Hold;
- migrate to another archival tier;
- retain a reduced lawful representation;
- transfer custody;
- delete physical or logical copies;
- perform cryptographic erasure;
- reject disposition because dependencies or Authority are incomplete.

Disposition review should evaluate policy, holds, Authority, dependency relationships, dispute status, copies, backups, encryption keys, and required evidence.

No one mutable expiry flag should authorize irreversible deletion where the Trust Profile requires dual control, review, or hold checks.

---

# 12.43 Deletion and Erasure

Deletion removes or makes a representation inaccessible within a defined system or scope. Erasure is a broader governed claim that specified data has been rendered inaccessible or destroyed according to defined semantics.

Relevant scopes include:

- active object;
- prior versions;
- replicas;
- backups and caches;
- indexes and derived views;
- externalized payloads;
- encryption keys;
- third-party copies;
- committed digests or metadata.

```text
Delete API succeeded
≠
All copies erased
```

The protocol must not overstate what a deletion mechanism can prove. An immutable commitment may remain even when the confidential payload is erased; that residual information and its privacy implications must remain explicit.

---

# 12.44 Cryptographic Erasure

**Cryptographic erasure** renders encrypted data infeasible to decrypt by destroying or irreversibly disabling all required decryption-key paths within a defined scope.

Its strength depends upon:

- sound encryption;
- complete key-path inventory;
- separation of object-specific keys;
- destruction of primary, backup, escrow, cached, and wrapped copies;
- absence of retained plaintext or alternate exports;
- accountable authorization;
- verifiable key lifecycle evidence;
- defined adversary and time horizon.

Destroying one KMS alias is insufficient when old key versions, backups, plaintext caches, exported keys, or another wrapping path remain.

Cryptographic erasure can make retained ciphertext unusable while preserving append-only evidence that the object and erasure event existed.

---

# 12.45 Deletion and Erasure Evidence

Deletion Evidence supports a bounded claim that a defined deletion or erasure action was authorized, attempted, completed, verified, or failed.

It may identify:

- target scope and inventory;
- policy and disposition decision;
- responsible and approving identities;
- method and system boundaries;
- relevant key identifiers;
- action and verification times;
- provider receipts or attestations;
- replica, backup, and cache coverage;
- residual commitments or metadata;
- exceptions, failures, and unverifiable copies.

Deletion Evidence does not prove a universal negative beyond its defined systems and controls.

Where a copy cannot be verified as erased, the result must remain partial, unavailable, or indeterminate rather than `complete`.

---

# 12.46 Migration Definition

**Migration** is an accountable transition between preservation-relevant states, representations, algorithms, providers, custodians, or protocol versions.

A Migration Record may bind:

- source and target identifiers;
- source and target representations or services;
- transformation rules;
- Authority and Policy;
- start, completion, and effective boundaries;
- integrity values;
- dependency changes;
- validation results;
- retained source state;
- rollback or exception behavior;
- cryptographic protection.

```text
Original Historical Evidence
          │
          ▼
Migration Record
          │
          ▼
Successor Preserved State
```

Migration must not rewrite history as though the target state always existed.

---

# 12.47 Representation and Media Migration

Representation migration transforms evidence into a new format, encoding, package, or canonical representation. Media migration moves retained state to new physical or logical media without necessarily changing the protocol representation.

The migration must distinguish:

- bit-preserving copy;
- lossless format transformation;
- semantic normalization;
- lossy or partial conversion;
- derived view;
- re-encryption or recompression;
- recreated index or manifest.

Where exact original bytes affect identifiers, Signatures, or Verification, they must remain retained or reproducibly recoverable.

A successor representation must not be presented as the original canonical object unless the applicable specification defines their equivalence.

---

# 12.48 Provider, Custody, and Authority Migration

Preservation may move between providers, Organizations, custodians, jurisdictions, or governance authorities.

The transition should preserve:

- complete target inventory;
- source and destination custody evidence;
- object and dependency integrity;
- encryption and key continuity;
- retention and Legal Hold state;
- access restrictions;
- historical policies and manifests;
- unresolved incidents or exceptions;
- portable Verification capability;
- source disposition after acceptance.

Transfer completion must be based on governed acceptance and validation, not merely on a copy job reporting success.

Authority migration must not allow the successor to rewrite the predecessor's attestations or erase evidence of earlier control.

---

# 12.49 Cryptographic Renewal

Cryptographic protections may weaken before the Evidence Lifetime ends.

**Renewal** adds new accountable protection to historical evidence without replacing the original evidence.

Renewal may include:

- a new digest over preserved canonical bytes;
- a renewed Signature;
- a new Checkpoint;
- a new External Anchor;
- a trusted-time or archival attestation;
- migration to a stronger algorithm;
- preservation of validation evidence from before deprecation.

The renewal record must bind the original commitment, algorithm context, renewal boundary, responsible Authority, and new protection.

```text
Original Signature
      +
Renewal Evidence
≠
Rewritten Original Signature
```

Renewal strengthens future interpretation only within the supported trust and time model.

---

# 12.50 Migration and Renewal Verification

A verifier evaluating migration or renewal should determine:

- whether source evidence was validly identified;
- whether the responsible role possessed Authority;
- whether the transformation was permitted;
- whether source and target integrity relationships verify;
- which semantics were preserved, changed, or lost;
- whether all required dependencies moved or remain resolvable;
- whether retention, hold, access, and encryption state continued;
- whether the result was independently checked;
- whether conflicts or gaps remain;
- which Trust Profile controls remain achieved.

A valid target object does not prove a valid migration path. A valid Migration Record does not make an invalid source claim true.

---

# 12.51 Service Closure, Exit, and Portability

A Preservation Service may close, terminate an account, change products, lose authorization, or become inaccessible.

A Preservation Plan should define:

- advance export capability;
- open inventory and manifest formats;
- bulk retrieval and proof export;
- encryption-key and metadata portability;
- historical policy and custody transfer;
- retention and Legal Hold continuity;
- alternate resolver and Verification paths;
- closure Checkpoint or final manifest;
- source deletion or residual-copy handling;
- responsible successor Authority.

Vendor availability must not be the only method for interpreting protocol semantics.

Exit testing should occur before an emergency. A theoretical export feature that has never been tested provides weaker assurance than a verified portable restoration.

---

# 12.52 Preservation Evidence Record Model

TAIP may define PE record types for:

- acceptance and retention placement;
- integrity or fixity check;
- replica creation or loss;
- custody transfer;
- access or disclosure event;
- object-lock or retention change;
- Legal Hold activation or release;
- recovery test;
- incident or degradation;
- migration or renewal;
- disposition approval;
- deletion or erasure;
- provider closure or final export.

Each type must define producer eligibility, target scope, required properties, time semantics, authorization, result states, and Verification procedure.

An opaque `archive status` record is insufficient when the underlying claims have different security and lifecycle consequences.

---

# 12.53 Preservation Manifest and Checkpoint

A **Preservation Manifest** identifies a governed collection of preserved targets, dependencies, integrity values, classifications, and lifecycle state.

A **Preservation Checkpoint** is a cryptographically protected commitment to a defined Preservation Manifest or preservation state at a boundary.

It may bind:

- manifest identifier and version;
- collection scope;
- aggregate commitment;
- item count or range;
- Preservation Service and Authority;
- applicable policy;
- integrity-check boundary;
- predecessor checkpoint;
- exceptions and omissions;
- Witness or External Anchor evidence.

A Preservation Checkpoint is distinct from an Evidence Hash Chain Checkpoint and a KT Checkpoint. Cross-commitment is permitted, but target semantics and proof rules remain explicit.

---

# 12.54 Monitoring, Audit, and Attestation

Preservation monitoring may evaluate:

- ingestion and retention coverage;
- lock and hold configuration;
- object integrity;
- replica health;
- dependency resolvability;
- recovery-test results;
- encryption-key continuity;
- access exceptions;
- migration deadlines;
- pending disposition;
- provider or control-domain concentration.

An audit or attestation must identify its period, scope, criteria, sample method, evidence source, responsible identity, exceptions, and limitations.

Certification of a service or control environment does not prove that a particular target was included and correctly configured. Target-specific Preservation Evidence and service-level assurance may compose, but remain distinct.

---

# 12.55 Partial Preservation, Gaps, and Degradation

Preservation may fail partially.

Examples include:

- focal object retained but schema missing;
- ciphertext retained but key unavailable;
- one replica corrupted;
- Chain retained without required Witness Registry snapshot;
- manifest preserved but external payload lost;
- Legal Hold applied after an unprotected interval;
- successful migration with some unsupported extensions;
- integrity verified but recovery untested.

The system must identify which targets, intervals, dependencies, and claims remain supported.

```text
Partial Preservation
≠
No Evidence
≠
Full Preservation
```

Graceful degradation is conforming only when the downgrade and its effect on Achieved Trust Profile remain explicit.

---

# 12.56 Verification Integration and Outcomes

Preservation Verification should evaluate distinct layers:

- PE structure and schema;
- target identity and integrity;
- Preservation Service identity and Historical Key State;
- Authority and Policy;
- target scope and time semantics;
- retention, lock, hold, custody, or migration state;
- integrity and recovery evidence;
- VDG availability;
- conflicts, gaps, and unsupported semantics;
- Trust Profile satisfaction.

Outcomes should distinguish at least:

- `valid`;
- `invalid`;
- `indeterminate`;
- `unsupported`;
- `unavailable`;
- `incomplete`;
- `conflicting`;
- `valid-with-warnings` where defined by TAIP.

The Verification Report must state the preservation claims actually evaluated rather than returning one ambiguous archive-health flag.

---

# 12.57 Trust Profiles and Achieved Preservation

A Trust Profile may define preservation requirements for:

- Evidence Lifetime;
- retained object and dependency classes;
- immutability or WORM mode;
- retention and Legal Hold controls;
- encryption and key custody;
- replica count and independence;
- integrity-check cadence;
- recovery objectives and testing;
- geographic or jurisdictional constraints;
- migration and renewal triggers;
- portable export;
- PE types and assurance evidence.

Intended Preservation is the selected plan or advertised target. Achieved Preservation is the state supported by actual controls and available evidence.

```text
Intended Preservation
≠
Achieved Preservation
```

Missing mandatory dependencies or failed controls must reduce or prevent the claimed profile achievement.

---

# 12.58 Privacy and Data Minimization

Preservation can increase privacy risk by retaining sensitive information, stable identifiers, access histories, incident details, and relationship graphs for long periods.

The architecture should apply:

- purpose limitation;
- minimum sufficient evidence;
- scoped identifiers;
- encryption and access separation;
- selective disclosure;
- controlled indexes;
- retention limits;
- accountable redaction;
- lawful disposition;
- query and export controls.

Minimization should begin before commitment. Encryption does not eliminate retention or breach risk if keys and access persist.

Privacy measures must not be represented as complete Preservation when they remove mandatory evidence. The Verification outcome must show the effect of redaction, erasure, or unavailable plaintext.

---

# 12.59 Dispute Packs and Independent Export

A Dispute Pack may carry preserved evidence and dependencies outside the originating environment.

A Preservation export may include:

- focal canonical objects;
- Preservation Manifests and PE;
- Chain, Witness, Checkpoint, Anchor, and Key Transparency material;
- schemas, registries, profiles, algorithms, and policies;
- custody and migration evidence;
- integrity values and proof material;
- decryption instructions or authorized key-access process;
- known omissions, redactions, and unavailable dependencies.

The export must distinguish embedded material from externally resolvable dependencies and must not expose secrets beyond the authorized Verification Context.

Package creation does not prove completeness or validity. Chapter 13 defines the full Dispute Pack model.

---

# 12.60 Anti-Patterns and Relationship to Other Controls

The following are architectural anti-patterns:

## Stored Equals Preserved

Treating successful object-store upload as sufficient long-term Preservation.

## Backup Equals Archive

Assuming rolling operational backups preserve historical semantics, dependencies, and retention state.

## WORM Equals Truth

Treating immutable storage as proof that preserved claims are valid or complete.

## Replica Count Equals Independence

Representing copies under one administrative and key-control domain as independent custodians.

## Payload-Only Retention

Retaining Evidence Records while losing schemas, key history, Policy, registries, or cryptographic specifications.

## Mutable Locator Dependency

Depending upon a URL, KMS alias, bucket path, or registry page that can change without version or integrity evidence.

## Encryption Without Key Continuity

Retaining ciphertext while losing authorized decryption and recovery capability.

## Keys Without Separation

Storing evidence, decryption keys, recovery material, and administrative credentials under one compromise domain while claiming separation.

## Untested Recovery

Treating backup or durability telemetry as proof of recoverability without representative restoration tests.

## Silent Repair

Replacing corrupted or missing evidence without preserving the incident, repair source, and validation result.

## Migration by Overwrite

Replacing original evidence with a new representation without a verifiable source-to-target relationship.

## Latest-Specification Substitution

Interpreting historical objects using current schemas, algorithms, profiles, or identity state without preserving applicable versions.

## Retention Expiry Equals Deletion Authority

Deleting automatically at calculated expiry without governed disposition, Legal Hold, dependency, and Authority checks.

## Delete API Equals Erasure

Claiming universal erasure from deletion in one system while replicas, backups, caches, keys, or exports remain unresolved.

## Legal Hold as Mutable Label

Using an unprotected application tag as the sole barrier against irreversible deletion.

## Privacy by Historical Falsification

Silently removing or rewriting committed state rather than recording bounded redaction, disposition, or erasure.

Preservation composes with other controls:

```text
Protocol Objects          what evidence exists
Hash Chains               how canonical history is linked
Witnesses                 who observed defined state
Checkpoints and Anchors   which boundaries were committed
Key Transparency          which keys applied historically
Preservation              whether evidence remains interpretable
Dispute Packs             how evidence becomes portable
Verification              which claims remain supported
```

No single layer substitutes for the others.

---

# Preservation Invariants

### INV-PRES-001 — Stored/Preserved Separation

Successful storage, backup, or replication MUST NOT by itself be represented as complete Preservation.

### INV-PRES-002 — Evidence/Operational Lifetime Separation

Evidence Lifetime MUST remain independently governable when it exceeds the Operational Lifetime of the producing system.

### INV-PRES-003 — Bytes/Meaning Composition

Preservation MUST consider both retained representations and the historical semantics required for interpretation.

### INV-PRES-004 — Dependency-Graph Scope

Preservation of a focal object MUST NOT imply preservation of its Verification Dependency Graph unless the required dependencies are identified and supported.

### INV-PRES-005 — Target Boundedness

Every Preservation claim MUST identify a bounded target, scope, time, and control meaning.

### INV-PRES-006 — Preservation/Validity Separation

Preservation of evidence MUST NOT establish truth, Authority, Policy compliance, or validity of the preserved claim by itself.

### INV-PRES-007 — Preservation/Completeness Separation

A preserved collection MUST NOT be represented as complete for claims outside its governed scope or without applicable completeness evidence.

### INV-PRES-008 — Protocol/Storage Identity Separation

Protocol Object identifiers, canonical digests, storage identifiers, and mutable locators MUST remain distinguishable.

### INV-PRES-009 — Canonical Historical Integrity

Storage transformation MUST NOT silently change canonical evidence or the integrity input used for historical Verification.

### INV-PRES-010 — Append-Only Correction

Correction, repair, migration, renewal, and disposition of committed evidence MUST create additional accountable state rather than erase the original history silently.

### INV-PRES-011 — Lifecycle Separation

Submitted, accepted, retained, integrity-verified, replicated, recoverable, held, migrated, deleted, and erased states MUST remain distinct where their semantics differ.

### INV-PRES-012 — Retention/Hold Separation

Retention, WORM lock, Legal Hold, and disposition eligibility MUST remain distinct controls.

### INV-PRES-013 — Expiry/Deletion Separation

Retention expiry MUST NOT by itself be represented as authorization or proof of deletion.

### INV-PRES-014 — WORM Scope

WORM or object-lock claims MUST remain bounded to the exact protected bytes, versions, metadata, interval, provider, and override semantics.

### INV-PRES-015 — Replication/Independence Separation

Replica count MUST NOT be represented as organizational or control-domain independence without applicable evidence.

### INV-PRES-016 — Durability/Availability Separation

Durability, current availability, and recoverability MUST remain separately evaluable.

### INV-PRES-017 — Custody/Ownership Separation

Custody of preserved evidence MUST NOT by itself establish ownership, truth, or Authority over the underlying action.

### INV-PRES-018 — Ciphertext/Decryptability Separation

Retention of ciphertext MUST NOT be represented as preservation of interpretable evidence when required decryption capability is unavailable.

### INV-PRES-019 — Data/Key Control Separation

Where a Trust Profile requires separation, evidence custody, encryption-key custody, recovery control, and decryption Authority MUST remain distinct.

### INV-PRES-020 — Backup/Recovery Separation

Backup creation MUST NOT be represented as successful recovery without a governed restoration result.

### INV-PRES-021 — Fixity/Semantics Separation

A successful bit-level integrity check MUST NOT by itself establish semantic interpretability or dependency availability.

### INV-PRES-022 — Repair Accountability

Repair of corrupted preserved state MUST retain the detected failure, trusted repair source, method, and validation result.

### INV-PRES-023 — Migration Non-Rewrite

Migration MUST preserve the source, target, transformation relationship, and resulting limitations without representing the target as the original historical state.

### INV-PRES-024 — Renewal Non-Substitution

Cryptographic renewal MUST add protection to historical evidence rather than replace or falsify the original protection.

### INV-PRES-025 — Historical-Version Interpretation

Historical evidence MUST remain associated with applicable schema, algorithm, identity, registry, Policy, and Trust Profile context.

### INV-PRES-026 — Deletion-Scope Boundedness

Deletion and erasure claims MUST remain bounded to the systems, copies, keys, methods, and verification evidence actually evaluated.

### INV-PRES-027 — Cryptographic-Erasure Completeness

Cryptographic erasure MUST NOT be represented as complete while an identified required decryption path or retained plaintext copy remains unresolved.

### INV-PRES-028 — Legal-Hold Accountability

Legal Hold activation, scope change, override, and release MUST be attributable to governed Authority and preserved evidence.

### INV-PRES-029 — Partial-Failure Visibility

Missing, corrupted, inaccessible, unsupported, redacted, or unverified preservation state MUST remain explicit.

### INV-PRES-030 — Intended/Achieved Separation

Intended Preservation controls and Achieved Preservation supported by evidence MUST remain distinct.

### INV-PRES-031 — Portability

Long-term Preservation MUST NOT depend exclusively upon undocumented proprietary semantics or the continued operation of one provider.

### INV-PRES-032 — Privacy Without False Assurance

Data minimization, redaction, restriction, deletion, or erasure MUST NOT be represented as full Preservation when mandatory verification material is no longer available.

---

# Architectural Requirements

### REQ-PRES-001

TAIP MUST define versioned semantics for Preservation Evidence, Preservation Targets, lifecycle states, and Verification Outcomes.

### REQ-PRES-002

Every PE object MUST identify its type, version, stable identifier, producer, target scope, and applicable time semantics.

### REQ-PRES-003

PE cryptographic protection MUST cover all properties whose alteration could change target, policy, custody, retention, hold, integrity, migration, recovery, or disposition meaning.

### REQ-PRES-004

Preservation Targets MUST be deterministically identifiable through Protocol Object identifiers, digests, manifests, ranges, commitments, or another governed selector.

### REQ-PRES-005

Preservation claims MUST identify the Preservation Service and responsible Authority applicable to the claimed control.

### REQ-PRES-006

Preservation planning MUST define the Evidence Lifetime or the governed rule by which it is determined.

### REQ-PRES-007

A Preservation Plan SHOULD identify the Accountability Claims, Trust Profiles, object classes, dependencies, controls, recovery objectives, and disposition behavior in scope.

### REQ-PRES-008

Material Preservation Plan and policy changes affecting future Verification SHOULD create accountable historical evidence.

### REQ-PRES-009

Retention Policies MUST be versioned and MUST define applicable classes, start triggers, duration or end rules, overrides, and responsible Authority.

### REQ-PRES-010

Historical evidence MUST remain associated with the retention-policy version applicable to its lifecycle decisions.

### REQ-PRES-011

Evidence classification affecting retention, access, encryption, or disposition MUST be governed, attributable, and historically interpretable.

### REQ-PRES-012

Preservation ingestion MUST distinguish submission, acceptance, rejection, quarantine, and pending states.

### REQ-PRES-013

An ingestion receipt MUST state its exact claim and MUST NOT imply unperformed retention, replication, integrity, or recovery controls.

### REQ-PRES-014

Preservation MUST retain or verifiably relate the Protocol Object identifier, canonical protected representation, digest, and storage representation.

### REQ-PRES-015

Conflicting protected content associated with one identifier MUST be preserved and surfaced as a conflict rather than silently overwritten.

### REQ-PRES-016

Preservation planning SHOULD identify the Verification Dependency Graph required for the Accountability Claims and Verification Contexts in scope.

### REQ-PRES-017

Mandatory dependencies MUST be embedded, preserved, or durably resolvable through authenticated identity, integrity, version, and availability semantics.

### REQ-PRES-018

Dependency traversal MUST preserve typed references, detect cycles, terminate safely, and enforce governed resource bounds.

### REQ-PRES-019

Historical schemas, canonicalization rules, extension behavior, and relevant test material MUST remain available for required future interpretation.

### REQ-PRES-020

Cryptographic algorithm identifiers, parameters, public keys, validation rules, and historical policy context required for Verification MUST remain preserved or durably resolvable.

### REQ-PRES-021

Historical Key State, Key Purpose, Protocol Identity, and required Key Transparency evidence MUST be preserved for Signatures within the applicable Evidence Lifetime.

### REQ-PRES-022

Authority, Policy, Registry, and Trust Profile versions required for historical evaluation MUST remain preserved or durably resolvable.

### REQ-PRES-023

Chain, Witness, Checkpoint, External Anchor, and proof material required by the applicable Trust Profile MUST remain associated with their governed verification rules.

### REQ-PRES-024

WORM and object-lock evidence MUST identify protected target, version scope, mode, activation boundary, expiry, administrative override semantics, and provider or control domain.

### REQ-PRES-025

Mutable metadata or indexes capable of changing preservation interpretation or target resolution MUST receive protection appropriate to their role.

### REQ-PRES-026

Trust Profiles claiming independent Preservation MUST define and evaluate organizational, administrative, infrastructure, key-custody, and correlated-failure criteria.

### REQ-PRES-027

Durability, availability, retrieval latency, recovery time, and acceptable data-loss objectives MUST remain separately representable where required.

### REQ-PRES-028

Custody evidence SHOULD identify responsible custodian, target, control domain, applicable policy, interval, and relevant transfer or exception events.

### REQ-PRES-029

Custody transfer MUST verify target identity and integrity before and after transfer and MUST preserve source, destination, authorization, and receipt state.

### REQ-PRES-030

Encrypted Preservation MUST identify encryption suite, key identifiers and versions, protected scope, authenticated metadata, and authorized recovery semantics.

### REQ-PRES-031

The Preservation Plan MUST address encryption-key rotation, backup, recovery, compromise, succession, migration, and authorized destruction for the required Evidence Lifetime.

### REQ-PRES-032

Loss of mandatory decryption capability MUST produce an explicit unavailable, incomplete, or indeterminate outcome rather than success.

### REQ-PRES-033

Where separation is required, evidence storage, key custody, recovery control, and decryption Authority MUST be assigned and evaluated as distinct roles or control domains.

### REQ-PRES-034

Accountability-relevant access, privilege change, denial, or administrative override SHOULD create protected Access Evidence under applicable policy.

### REQ-PRES-035

Integrity monitoring evidence MUST identify target population, sample scope, algorithm, expected value, cadence or boundary, result, and exceptions.

### REQ-PRES-036

Repair evidence MUST identify the corruption, expected and observed values, trusted repair source, method, result, and unresolved uncertainty.

### REQ-PRES-037

Backups used to support Preservation MUST possess governed identity, integrity, retention, encryption, dependency, and restoration semantics.

### REQ-PRES-038

Recovery tests MUST identify their target scope, environment, dependencies, exclusions, result, and applicable recovery objective.

### REQ-PRES-039

Disaster-recovery actions MUST preserve committed historical evidence and SHOULD record material failures, decisions, exceptions, and repairs.

### REQ-PRES-040

Archival representations and formats MUST remain sufficiently documented to support independent interpretation for the required Evidence Lifetime.

### REQ-PRES-041

Retention calculation MUST identify its governed trigger, time semantics, duration, policy version, calendar behavior, and applicable overrides.

### REQ-PRES-042

Retention expiry MUST lead to governed disposition eligibility and MUST NOT silently bypass Legal Hold, Authority, dependency, or dispute checks.

### REQ-PRES-043

Legal Hold records MUST identify issuing Authority, target scope, activation boundary, applicable restrictions, and authorized release behavior.

### REQ-PRES-044

Disposition decisions MUST identify the target inventory, Authority, policy basis, hold checks, chosen action, dependencies, and exceptions.

### REQ-PRES-045

Deletion and erasure evidence MUST identify the exact systems, versions, replicas, backups, caches, keys, and residual information within scope.

### REQ-PRES-046

Cryptographic erasure claims MUST evaluate all governed decryption-key paths, retained plaintext, exported keys, escrow material, caches, and backups within scope.

### REQ-PRES-047

Unverified or failed deletion targets MUST remain explicit and MUST NOT be counted as successfully erased.

### REQ-PRES-048

Migration Records MUST bind source, target, transformation, Authority, integrity results, dependency changes, times, and retained limitations.

### REQ-PRES-049

Representation migration MUST preserve original canonical bytes when required for historical identifiers, Signatures, or Verification, or provide a governed source-to-target relationship.

### REQ-PRES-050

Provider and custody migration MUST preserve retention, Legal Hold, access, encryption, policy, integrity, and unresolved-incident state.

### REQ-PRES-051

Cryptographic renewal MUST bind original evidence and protection to the new commitment without replacing the original historical protection.

### REQ-PRES-052

Migration and renewal Verification MUST identify preserved semantics, changed semantics, lost semantics, missing dependencies, and resulting Trust Profile achievement.

### REQ-PRES-053

Preservation Services SHOULD provide portable export of targets, manifests, PE, dependencies, proofs, encryption metadata, policies, and known omissions.

### REQ-PRES-054

Service closure procedures MUST address final inventory, portable export, custody transition, key continuity, hold continuity, source disposition, and successor resolution.

### REQ-PRES-055

Preservation Manifests MUST identify collection membership, target integrity, policy context, lifecycle state, and known omissions or exceptions.

### REQ-PRES-056

Preservation Checkpoints MUST identify their exact manifest or state target and MUST remain distinct from Evidence Hash Chain and Key Transparency Checkpoints.

### REQ-PRES-057

Service-level audits or certifications MUST NOT be represented as target-specific Preservation Evidence unless the target and control relationship are established.

### REQ-PRES-058

Partial Preservation MUST identify affected objects, dependencies, intervals, claims, and resulting assurance degradation.

### REQ-PRES-059

Verification Reports MUST distinguish structural, integrity, custody, retention, hold, recovery, migration, availability, completeness, and profile outcomes.

### REQ-PRES-060

Trust Profile achievement MUST be based upon actual Preservation Evidence and available dependencies rather than intended configuration or provider claims alone.

### REQ-PRES-061

Privacy, redaction, retention limitation, and erasure controls MUST state which mandatory Verification steps remain possible.

### REQ-PRES-062

Unsupported mandatory PE semantics, formats, algorithms, dependencies, or lifecycle transitions MUST produce an explicit non-success Verification Outcome.

---

# Security Considerations

Preservation extends the attack surface across long time periods, multiple custodians, migrations, and changing technologies.

## Unauthorized Deletion

An attacker or administrator may delete evidence before retention expiry or while a Legal Hold applies. Strong Authority checks, separation of duties, immutable retention, protected hold state, alerts, and independent copies reduce this risk.

## Malicious Retention Reduction

A policy administrator may shorten retention, change a classification, or backdate an expiry trigger. Historical policy versions, dual control, accountable transitions, and monitoring are required where the impact is material.

## Ransomware and Administrative Compromise

Online replicas under one credential or administrative account may be encrypted, deleted, or corrupted together. Offline, cross-account, cross-provider, immutable, and independently controlled copies can reduce correlated failure.

## False WORM Claims

An operator may describe ordinary versioning or a reversible governance lock as non-overridable immutability. PE must identify the actual mode, protected scope, override path, activation interval, and provider boundary.

## Mutable Index Redirection

Immutable payloads can be undermined by a mutable catalog, manifest, DNS record, KMS alias, or resolver that points verifiers elsewhere. Resolution metadata affecting target identity requires its own integrity and history protection.

## Dependency Stripping

An archive may retain focal Evidence Records while omitting schemas, Historical Key State, Policy, registries, or proof material. Dependency inventories, manifests, closure checks, and portable recovery tests expose this failure.

## Key Loss

Loss of encryption or recovery keys can convert durable ciphertext into permanently unavailable evidence. Key backup, succession, threshold recovery, test decryption, and explicit lifecycle evidence are necessary where lawful and appropriate.

## Key Theft

Preserving decryption keys for a long Evidence Lifetime increases exposure. Key separation, hardware protection, threshold access, rotation, rewrapping, monitoring, least privilege, and minimized plaintext reduce the risk.

## Plaintext Residue

Temporary files, search indexes, caches, exports, crash dumps, and restored copies may retain plaintext outside the governed encrypted archive. Inventory and erasure claims must include these paths where applicable.

## Cryptographic-Erasure Overstatement

Destroying one wrapping key does not erase data if another key version, escrow copy, plaintext replica, or export remains. Claims must remain bounded to verified key paths and storage scopes.

## Silent Corruption

Bit rot, media failure, software defects, or malicious change may remain undetected until a dispute. Periodic fixity checks, diverse replicas, Checkpoints, and tested repair procedures reduce detection latency.

## Corrupt Repair Source

Automated scrubbing can replicate corrupted state if the expected value or repair source is wrong. Repair must use authenticated commitments, source comparison, conflict handling, and preserved incident evidence.

## Backup Poisoning

Compromised or invalid state may be copied into every backup. Historical generations, integrity validation, malware controls, and Checkpoint comparison are required to distinguish durable corruption from valid history.

## Restore-Time Attack

Recovery credentials and break-glass paths may bypass normal controls. Restore environments should authenticate operators, verify evidence before use, limit network exposure, record access, and prevent recovered data from silently replacing canonical history.

## Split Custody Views

Different custodians may present incompatible inventories, manifests, or deletion status. Signed manifests, preservation checkpoints, cross-custodian comparison, Witnessing, and Dispute Packs help expose inconsistency.

## Provider Lockout

Account termination, billing disputes, sanctions, credential loss, or service closure may block access. Portable exports, alternate custody, contractual exit, key portability, and routine export testing reduce unilateral dependence.

## Format Obsolescence

Retained bytes may become unreadable when formats, codecs, hardware, or proprietary services disappear. Open documentation, migration triggers, reference tools, and test vectors support future interpretation.

## Algorithm Deprecation

Digest or Signature algorithms may weaken during the Evidence Lifetime. Renewal and migration must occur before the old protection becomes insufficient and must preserve the original evidence and time context.

## Migration Substitution

A malicious migration can omit, transform, reorder, or replace evidence while reporting success. Source inventory, deterministic transformation, before-and-after integrity, independent validation, and preserved source state constrain this attack.

## Legal Hold Suppression

An authorized or malicious actor may fail to apply, narrow, or prematurely release a hold. Hold Authority, target-selection evidence, notifications, monitoring, dual control, and release records strengthen accountability.

## Excessive Retention

Keeping sensitive evidence indefinitely expands breach, misuse, discovery, and surveillance risk. Preservation policy must balance accountability requirements with minimization, purpose, retention limits, and lawful disposition.

## Audit Scope Inflation

A service may use a general certification to imply that every object was retained correctly. Verifiers must distinguish system-level control evidence from target-specific placement, configuration, integrity, and recovery evidence.

## Time Manipulation

Incorrect or malicious clocks can change retention start, expiry, hold, migration, and deletion conclusions. Time semantics, authenticated ordering, external boundaries, and uncertainty handling reduce dependence upon one clock.

## Denial of Verification

An adversary may withhold schemas, keys, manifests, or proof material while leaving primary bytes available. Availability and dependency failures must be explicit and should be mitigated through distributed Preservation and portable packages.

## Resource Exhaustion

Deep dependency graphs, huge archives, decompression bombs, expensive cryptography, and excessive versions can exhaust verifiers or recovery systems. Implementations should enforce governed limits, streaming validation, safe parsing, bounded recursion, and partial-result reporting.

## Insider Collusion

Administrators of storage, keys, policy, and audit systems may collude. Trust Profiles requiring stronger assurance should distribute control across Organizations or independent domains and evaluate actual ownership and incentives.

---

# Privacy Considerations

Long-term retention can amplify privacy harm because data remains linkable and usable beyond the context in which it was created.

## Minimum Sufficient Evidence

Preserve evidence needed for defined Accountability Claims, not unlimited operational telemetry. Hidden chain-of-thought, unrelated personal data, and redundant payloads are not required merely because storage is inexpensive.

## Purpose Limitation

The Preservation Plan should identify the accountability, legal, contractual, security, or audit purposes supporting retention. New incompatible use may require new Authority, Policy, notice, or consent under applicable governance.

## Metadata Sensitivity

Identifiers, digests, filenames, access logs, retention categories, hold events, migration timing, and custody relationships can reveal sensitive facts even when payloads are encrypted.

## Long-Term Linkability

Stable identifiers and public commitments may enable correlation across systems and years. Scoped namespaces, minimized public metadata, opaque commitments, and controlled resolution may reduce unnecessary linkage.

## Encryption Limits

Encryption protects confidentiality only while algorithms, implementations, keys, and access governance remain sound. It does not eliminate data-subject, retention, breach, or disclosure obligations.

## Query Privacy

Archive searches and dependency resolution reveal investigative interests and relationships. Local replicas, private retrieval, access minimization, batching, and controlled audit disclosure may reduce query leakage.

## Legal Hold Sensitivity

The existence and scope of a hold may reveal a dispute or investigation. Public evidence may use a protected reference or commitment while authorized parties retain detailed scope.

## Redaction

A redacted or selectively disclosed view must remain distinguishable from canonical evidence. Redaction metadata should identify which claims cannot be verified without protected material.

## Erasure Residuals

Erasing a payload may leave digests, signed manifests, event timing, identifiers, and PE. The privacy impact of residual commitments must be assessed before original commitment when possible.

## Backups and Derived Copies

Privacy disposition must consider replicas, backups, caches, search indexes, analytics stores, test restorations, and exported Dispute Packs. A primary-store deletion is not a complete inventory.

## Custodian Disclosure

Transferring evidence to another Preservation Service can change jurisdiction, subprocessors, control, and breach exposure. Custody and policy evidence should preserve relevant restrictions.

## Access Transparency

Access evidence can deter misuse but also create a sensitive behavioral record. Retain the minimum access detail needed for accountability and protect it according to its own classification.

## Post-Quantum Confidentiality

Long-lived ciphertext may be collected now and attacked later. Evidence with long confidentiality requirements should use suitable algorithms, key lengths, migration plans, and where necessary post-quantum protection according to current cryptographic guidance.

## Tension Between Accountability and Erasure

Append-only accountability and data-erasure duties can conflict. Separating minimal commitments from encrypted payloads, using scoped keys, preserving erasure evidence, and reporting unavailable content can support both goals without claiming that the tension disappears.

TrustAgentAI represents preservation and erasure evidence. It does not determine which privacy or retention law applies.

---

# Design Rationale

## Why Stored Is Not Preserved

Storage answers where bytes are located now. Preservation asks whether evidence can still be authenticated, interpreted, retrieved, and governed after systems, providers, formats, keys, and Organizations change.

## Why Evidence Lifetime Is Separate

Operational systems optimize for current business activity and are routinely replaced. Accountability evidence may be needed years later, so its lifetime, dependencies, custody, and migration cannot be left to incidental application retention.

## Why the Verification Dependency Graph Is Central

A signed object without its schema, key history, Policy, algorithm definition, or historical registry may be intact but uninterpretable. Preserving the dependency graph protects meaning, not just bytes.

## Why WORM Is a Component

WORM can constrain alteration and deletion, but it cannot determine whether correct evidence was written, whether dependencies exist, whether data is decryptable, or whether recovery works. It is one control within a larger Preservation model.

## Why Replication and Independence Differ

Copies improve resilience against local failure. Independent control improves resilience against unilateral manipulation, account compromise, provider failure, or censorship. Strong profiles may require both.

## Why Encryption Keys Are Dependencies

Ciphertext is not usable evidence without authorized decryption. Encryption keys and their lifecycle therefore form part of the Verification Dependency Graph, even though secret material must be governed and disclosed differently from public evidence.

## Why Recovery Must Be Tested

Many failures appear only during restoration: missing keys, corrupt manifests, undocumented formats, incomplete dependencies, or expired credentials. Representative tests convert assumed recoverability into bounded evidence.

## Why Legal Hold Is a Protocol State

Hold activation and release can determine whether evidence survives a dispute. Treating hold as accountable state makes scope, Authority, timing, overrides, and failures visible without claiming to define applicable law.

## Why Retention Expiry Is Not Automatic Deletion

Expiry is one input to disposition. Holds, disputes, dependency relationships, exceptions, and dual-control rules may change the permitted action. Separating these states prevents one clock or tag from authorizing irreversible loss.

## Why Erasure Claims Are Bounded

No ordinary system can prove absence from every possible unknown copy. TrustAgentAI therefore requires an exact scope, method, key-path inventory, verification evidence, and honest residual limitations.

## Why Migration Preserves the Source

Historical evidence may depend upon exact bytes and original semantics. A Migration Record allows technology to evolve while keeping the source, transformation, result, and loss visible.

## Why Renewal Adds Evidence

Re-signing or rehashing cannot make a weak historical protection disappear. Additional renewal evidence preserves the original cryptographic fact and provides a later protected continuity boundary.

## Why Portability Is Mandatory for Longevity

An archive whose meaning or access depends forever upon one vendor cannot reliably outlive that vendor. Portable objects, manifests, proofs, metadata, keys, specifications, and testable exports reduce this dependency.

## Why Deletion Is Also Accountable

Premature deletion can destroy accountability, while excessive retention can create privacy harm. Recording authorized disposition, method, scope, result, and exceptions makes both preservation and erasure governable.

---

# Summary

Preservation maintains Accountability Evidence and required Verification Dependencies across an Evidence Lifetime that may greatly exceed the Operational Lifetime of the systems that created them.

The central distinction is:

```text
Stored
≠
Preserved
```

Future Verification requires more than retained bytes. It may require canonical representations, schemas, canonicalization rules, cryptographic specifications, Historical Key State, Authority and Policy evidence, registries, Trust Profiles, Chain and Witness material, Checkpoints, Anchor Evidence, encryption-key continuity, custody history, and migration evidence.

WORM, object lock, encryption, replication, backups, Legal Hold, recovery, and archival storage can support Preservation, but each has a bounded meaning. Durability is not availability. Replication is not independence. Backup is not tested recovery. Retention expiry is not deletion Authority. A delete operation is not universal erasure.

Preservation Evidence records accountable claims about retention, integrity, custody, recovery, migration, availability, hold, disposition, and erasure. Verification evaluates those claims under an explicit target, historical Policy, and Trust Profile while preserving missing dependencies, conflicts, and uncertainty.

Migration and cryptographic renewal create additional historical evidence rather than rewriting the original state. Portable exports and documented formats reduce dependence upon one provider. Privacy controls minimize unnecessary retention without misrepresenting redacted or erased material as fully available.

A conforming TrustAgentAI implementation therefore preserves both evidence and meaning, tests recovery rather than assuming it, records lifecycle changes rather than overwriting them, and reports Achieved Preservation from actual evidence rather than intended configuration.
