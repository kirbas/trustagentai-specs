# Chapter 11 — Key Transparency

> **A signature can be interpreted historically only when the verifier can determine which key, purpose, lifecycle state, and governing evidence applied at the relevant boundary.**

## Purpose

This chapter defines the architectural specification for TrustAgentAI **Key Transparency (KT)**, **Key Transparency Records (KTRs)**, and **Historical Key State (HKS)**.

Key Transparency preserves accountable, append-only evidence concerning the relationship between Protocol Identities and cryptographic keys. It enables a verifier to reconstruct the key state applicable to a historical Signature without silently substituting current configuration for historical fact.

This chapter establishes:

- the separation of Protocol Identity, Key Identifier, Key Purpose, Authority, and Policy;
- Key Transparency Record types and common semantics;
- key registration, activation, rotation, suspension, reactivation, retirement, expiration, revocation, compromise, and recovery;
- effective-time, record-time, publication-time, and discovery-time semantics;
- append-only key history, conflict detection, and correction;
- Key Transparency logs, Checkpoints, inclusion proofs, and consistency proofs;
- monitor, Witness, gossip, and External Anchor integration;
- historical key resolution and Signature interpretation;
- privacy, availability, migration, and algorithm-agility behavior;
- Key Transparency invariants and architectural requirements.

This chapter applies the common Protocol Object rules defined in [06-Protocol-Objects.md](06-Protocol-Objects.md).

It builds upon the Evidence Record model in [07-Evidence-Record-Specification.md](07-Evidence-Record-Specification.md), the Hash Chain model in [08-Hash-Chain-Specification.md](08-Hash-Chain-Specification.md), the Witness model in [09-Witness-Observation-Specification.md](09-Witness-Observation-Specification.md), and the Checkpoint model in [10-Checkpoints-and-External-Anchors.md](10-Checkpoints-and-External-Anchors.md).

It also applies the system model in [05-System-Overview.md](05-System-Overview.md), the principles in [04-Design-Principles.md](04-Design-Principles.md), and the canonical terms in [Terminology.md](Terminology.md) and [Acronyms.md](Acronyms.md).

This chapter defines architectural semantics.

It does not require one universal identity provider, public-key infrastructure, certificate format, transparency-log implementation, distributed ledger, or key-management technology. It does not define final field names, concrete schemas, media types, wire encodings, canonicalization algorithms, Signature suites, resolver APIs, or jurisdiction-specific legal conclusions.

Those details belong to TAIP, Trust Profiles, cryptographic profiles, identity methods, governed registries, APIs, and test vectors.

---

# 11.1 Key Transparency Definition

**Key Transparency (KT)** is the TrustAgentAI capability that preserves independently verifiable historical evidence concerning identity-key relationships, permitted Key Purposes, and key lifecycle state.

Key Transparency enables a verifier to determine:

- which key was associated with a Protocol Identity;
- the interval or boundary for which that association applied;
- which Key Purpose or purposes were permitted;
- which accountable event created or changed the state;
- whether the transition was authorized under applicable rules;
- whether competing or inconsistent histories exist;
- which evidence supports the conclusion.

```text
Protocol Identity
        │
        ├── Key A: purpose P1, historical interval A
        ├── Key B: purpose P1, historical interval B
        └── Key C: purpose P2, suspended at boundary C
```

Key Transparency makes key history auditable. It does not make every published binding valid, authorized, or truthful.

---

# 11.2 Historical Key State Definition

**Historical Key State (HKS)** is the state of a cryptographic key and its applicable protocol authorization context at the historical boundary relevant to Verification.

Historical Key State may include:

- the Protocol Identity and Key Identifier;
- public-key material or an authenticated reference to it;
- permitted Key Purpose;
- lifecycle status;
- effective interval;
- transition history;
- compromise information;
- applicable algorithm constraints;
- supporting Key Transparency Records and proofs;
- unresolved conflicts or uncertainty.

Historical Key State is a derived, evidence-backed conclusion. It is not merely a field copied from a current directory.

```text
Historical Key State
≠
Current Key State
```

---

# 11.3 Scope

This chapter applies to cryptographic keys used for TrustAgentAI protocol actions, including keys used to protect:

- Evidence Records;
- Chain Entries and Chain administration events;
- Witness Observations;
- Checkpoints;
- Key Transparency Records;
- Migration Records;
- Preservation Evidence;
- Dispute Pack Manifests;
- Verification Reports where signed;
- Registry, profile, or governance state where a Trust Profile requires historical evaluation.

The same key may appear in another identity or certificate system. TrustAgentAI evaluates only the semantics and evidence brought into the applicable Verification Context.

Secrets used only for encryption, transport authentication, or ephemeral session establishment are outside this chapter unless a Trust Profile makes their history accountability-relevant.

---

# 11.4 Security Objectives

Key Transparency has five primary security objectives:

1. preserve durable identity-key history across rotation and organizational change;
2. prevent Current Key State from silently rewriting historical interpretation;
3. make unauthorized, conflicting, backdated, or suppressed key transitions detectable;
4. enable reproducible evaluation of Key Purpose and lifecycle state at a defined boundary;
5. retain enough evidence to explain both successful and indeterminate Signature conclusions.

The architectural objective is:

> A verifier can reproduce which historical key state was used, why that state was selected, which evidence supports it, and which uncertainty remains.

Achievement depends upon correct identity resolution, authenticated transitions, append-only history, publication discipline, time evidence, availability, and applicable Trust Profile rules.

---

# 11.5 What Key Transparency Establishes

Subject to successful Verification, Key Transparency may establish that:

- an identifiable key was registered for a Protocol Identity;
- a defined Key Purpose applied during a defined interval;
- an accountable transition changed the key's lifecycle state;
- the transition was signed or otherwise authorized by an eligible identity or role;
- a Key Transparency Record was included in a committed history;
- one published log state consistently extends another;
- a key was suspended, revoked, retired, expired, superseded, or reported compromised;
- competing key histories or unsupported gaps exist;
- a historical Signature can be evaluated against a reconstructed state.

Each conclusion is bounded by the available evidence and proof scope.

---

# 11.6 What Key Transparency Does Not Establish

Key Transparency does not by itself prove:

- the real-world identity of a key controller;
- possession of a private key at every moment in an interval;
- business Authority for an Accountable Action;
- Policy compliance beyond the evaluated key rules;
- truth of an Evidence Record's contents;
- absence of undisclosed compromise;
- completeness of a log or directory unless a governed completeness model supports that conclusion;
- independence of the Key Transparency operator;
- accuracy of self-asserted Event Time;
- Legal Validity or Regulatory Compliance.

```text
Valid Key Binding
≠
Valid Key Purpose
≠
Business Authority
≠
Policy Permission
≠
Business Truth
```

---

# 11.7 Conceptual Roles

A Key Transparency deployment may involve:

- a **Key Subject**, whose Protocol Identity is associated with a key;
- a **Key Controller**, possessing or administering private-key capability;
- a **Key Registrar**, accepting or issuing registration events;
- a **Transition Authorizer**, authorized to approve a lifecycle change;
- a **Key Transparency Operator**, maintaining and publishing transparent history;
- a **Monitor**, checking publication, consistency, conflicts, and policy compliance;
- a **Witness**, independently observing defined transparency state;
- a **Resolver**, retrieving identity, key, history, and proof material;
- a **Verification Engine**, deriving Historical Key State and evaluating Signatures;
- a **Preservation Service**, retaining records and dependencies.

One Organization may perform multiple roles, but role combination must not be represented as independence.

Role names do not create Authority. Eligibility and authorization require governed evidence.

---

# 11.8 Key Subject

The **Key Subject** is the Protocol Identity to which a key relationship or lifecycle state applies.

A Key Subject may represent:

- an Agent;
- a human Actor acting through an accountable interface;
- an Organization;
- a service;
- a Witness;
- a Checkpoint Authority;
- another protocol subject permitted by TAIP.

The Key Subject is not necessarily the device holding the private key, the Organization operating the Key Transparency service, or the person who approved a transition.

Where custody is delegated, the subject, controller, custodian, and authorizer remain separately identifiable.

---

# 11.9 Protocol Identity

A **Protocol Identity (PID)** is a stable TrustAgentAI identifier representing an Actor, Agent, Organization, service, or other protocol subject.

Protocol Identity provides continuity across key changes.

```text
Protocol Identity X
      ├── Key A
      ├── Key B
      └── Key C
```

A Protocol Identity must not be defined solely as whichever key is currently active when historical continuity requires the identity to survive rotation, revocation, or migration.

TrustAgentAI does not require one identity technology. A PID may be resolved through a governed identity method, registry, credential system, or other binding defined by TAIP and the applicable Trust Profile.

---

# 11.10 Key Identifier

A **Key Identifier (KID)** is a stable reference identifying specific cryptographic key material or a defined key record.

Key Identifier rules must define:

- namespace;
- comparison and normalization behavior;
- relationship to public-key material;
- collision handling;
- persistence across storage or resolver systems;
- whether the identifier represents one key, one version, or a governed key record.

A KID may be derived from key material, assigned by a registry, scoped to an issuer, or constructed by another governed method.

```text
Protocol Identity
≠
Key Identifier
```

Reusing one KID for materially different key material is prohibited unless the key method explicitly versions and disambiguates every representation.

---

# 11.11 Key Purpose

**Key Purpose (KP)** identifies the protocol purpose or purposes for which a key is permitted.

Examples include:

- Evidence Record signing;
- Chain administration;
- Witness Observation signing;
- Checkpoint signing;
- Key Transparency transition signing;
- Migration Record signing;
- Preservation attestation signing.

Possession of a valid private key does not imply permission for every protocol action.

Key Purpose may be represented through explicit identifiers, a governed registry, a constrained credential, a certificate extension, a policy binding, or another TAIP-defined mechanism.

A broad implementation label such as `signing-key` is insufficient where the Trust Profile requires narrower purpose separation.

---

# 11.12 Identity, Key, Purpose, Authority, and Policy Separation

The following concepts are distinct:

- **Protocol Identity** — who or what is represented;
- **Key Identifier** — which cryptographic key is referenced;
- **Key Purpose** — which protocol use is permitted for the key;
- **Authority** — which accountable actions the subject may perform;
- **Policy** — which rules govern the action and evidence;
- **key custody** — who or what controls private-key capability.

```text
Identity ──binds──> Key ──constrained by──> Key Purpose
    │                                      │
    └──────── Authority and Policy ────────┘
```

Verification must evaluate these dimensions independently and then compose the results.

A valid Signature by a historically active key with the correct Key Purpose may still fail Authority or Policy evaluation.

---

# 11.13 Key Transparency Record Definition

A **Key Transparency Record (KTR)** is a Protocol Object representing an accountability-relevant identity-key relationship or key lifecycle event.

A KTR may bind:

- record type and version;
- stable record identifier;
- Key Subject PID;
- affected KID and authenticated key material or reference;
- Key Purpose;
- prior and resulting lifecycle state;
- effective, record, and publication time semantics;
- predecessor or related KTR references;
- authorizing evidence;
- applicable Policy, registry, and Trust Profile context;
- cryptographic protection;
- extensions.

A KTR expresses a claim and transition. It becomes reliable evidence only after the applicable structural, cryptographic, authorization, historical, and transparency checks succeed.

---

# 11.14 Key Transparency Record Types

TAIP may define KTR types for:

- identity registration;
- key registration or binding;
- key activation;
- Key Purpose grant, restriction, or removal;
- key rotation or supersession;
- suspension;
- reactivation;
- retirement;
- expiration observation;
- revocation;
- compromise report or compromise-time update;
- recovery;
- custody or delegation change;
- correction or supersession of an earlier assertion;
- transparency-history checkpointing;
- migration or closure.

Record type determines permitted producers, required evidence, transition preconditions, and resulting semantics.

An opaque generic `update` event is insufficient when lifecycle consequences differ.

---

# 11.15 Logical State Model

Historical Key State is derived from an ordered set of valid KTRs and applicable external dependencies.

```text
Identity State
     +
Key Registration
     +
Purpose State
     +
Lifecycle Transitions
     +
Compromise and Recovery Evidence
     +
Transparency Proofs
     =
Historical Key State at boundary B
```

The state model must define:

- legal starting states;
- permitted transitions;
- authorization requirements;
- time and ordering rules;
- conflict behavior;
- terminal and reversible states;
- the effect of later-discovered evidence;
- how uncertainty is represented.

Derivation must be deterministic under a defined Verification Context.

---

# 11.16 Key Material and Fingerprints

A KTR that binds a key must identify the exact public-key material or an authenticated, persistently resolvable representation of that material.

The binding may include:

- canonical public-key bytes;
- algorithm and parameter identifiers;
- a cryptographic fingerprint;
- a certificate or credential reference;
- a key-version reference within a governed key-management system;
- proof material required to resolve the representation.

A fingerprint is only as durable as its algorithm, canonical input, and namespace definition.

An implementation must not treat two syntactically different encodings as the same key without a governed normalization rule. It must not treat a matching display label, device name, certificate subject, or KID alone as proof that the underlying key material matches.

---

# 11.17 Registration

**Key registration** creates an accountable historical assertion that defined key material is associated with a Key Subject under stated conditions.

Registration should bind:

- the Key Subject;
- the KID and public-key material;
- proposed Key Purpose;
- registrar and authorizer identities;
- proof of possession where required;
- applicable Policy and Trust Profile;
- initial lifecycle state;
- time semantics;
- predecessor or recovery evidence where applicable.

Registration does not necessarily activate the key.

```text
Registered
≠
Active
```

A Trust Profile may require independent approval, delayed activation, hardware attestation, identity proofing, or an inclusion proof before use.

---

# 11.18 Proof of Possession

**Proof of possession** demonstrates control of private-key capability corresponding to registered public-key material at a defined protocol step.

Proof of possession may reduce accidental or malicious registration of a key that the subject cannot use. It does not by itself prove:

- the controller's identity;
- authorized custody;
- permitted Key Purpose;
- continuing possession after the proof;
- absence of copied private-key material;
- Authority for later actions.

The proof must be bound to the registration context, challenge, identity, key, algorithm, and intended purpose so it cannot be replayed as authorization for another relationship.

Trust Profiles determine when proof of possession is mandatory and which proof methods are acceptable.

---

# 11.19 Activation

**Activation** changes a registered key into a state permitted for one or more defined Key Purposes.

Activation may require:

- a valid registration;
- proof of possession;
- approval by an eligible Transition Authorizer;
- satisfied custody or hardware controls;
- publication or log-inclusion evidence;
- a defined effective boundary;
- absence of conflicting terminal state;
- applicable Policy conditions.

Activation must identify the purposes activated. Activating one purpose must not silently activate every other purpose associated with the key.

A Signature created before the effective activation boundary must not be treated as valid merely because the key became active later, unless a governed rule explicitly permits and explains that result.

---

# 11.20 Transition Authorization

Every accountability-relevant key lifecycle transition must be attributable to authorized evidence.

Authorization may derive from:

- a currently eligible identity-management authority;
- a previously authorized subject key;
- a recovery authority or quorum;
- an organizational governance process;
- a hardware-backed administrative control;
- another method defined by TAIP and the applicable Trust Profile.

The verifier must evaluate the authorizer's Historical Key State, Key Purpose, Authority, and Policy at the transition's relevant boundary.

```text
Cryptographically Valid Transition Signature
≠
Authorized Transition
```

Circular reasoning is prohibited. A new key cannot establish its own authority to replace the only prior trust root unless the governing recovery or registration method explicitly permits that bootstrap and supplies independent evidence.

---

# 11.21 Rotation

**Key rotation** is an accountable transition from one key or key set to another while preserving Protocol Identity continuity.

A rotation record should identify:

- predecessor and successor KIDs;
- affected Key Purposes;
- authorization evidence;
- activation and retirement boundaries;
- any permitted overlap interval;
- custody or algorithm migration context;
- required transparency proofs.

Rotation does not erase or invalidate the predecessor's historical actions merely because the successor is current.

```text
Current key: B
Historical Signature boundary: A interval
Verification key: A, not B
```

The predecessor's resulting state—retired, superseded, revoked, or otherwise constrained—must remain explicit.

---

# 11.22 Overlap and Staged Rotation

A deployment may permit an overlap interval during which predecessor and successor keys are both active for defined purposes.

Overlap supports:

- distributed rollout;
- high-availability services;
- multi-region deployment;
- algorithm migration;
- offline signer replacement;
- recovery testing.

The overlap must be bounded and governed. The KTR history must identify which purposes, systems, or scopes each key may serve and when the overlap ends.

Unbounded overlap can turn rotation into permanent key accumulation and increase compromise exposure.

Where deterministic signer selection matters, TAIP or the Trust Profile must define whether multiple active keys are equally acceptable or whether priority, epoch, scope, or signer-selection rules apply.

---

# 11.23 Suspension

**Suspension** is a reversible lifecycle state that temporarily prohibits one or more key uses pending investigation, operational recovery, or another governed condition.

Suspension must identify:

- affected key and purposes;
- effective boundary;
- reason category where disclosure is permitted;
- authorizing evidence;
- expected review or expiry behavior where applicable;
- whether previously created Signatures remain interpretable;
- conditions for reactivation or escalation to revocation.

Suspension is not revocation.

```text
Suspended
≠
Revoked
```

A verifier must not infer that all activity before suspension was invalid. It must evaluate the historical boundary and any compromise evidence separately.

---

# 11.24 Reactivation

**Reactivation** returns a suspended key to an active state for defined Key Purposes.

Reactivation must not be modeled as deletion of the suspension event. It is a new accountable transition whose preconditions may include:

- resolution of the suspension cause;
- renewed proof of possession;
- custody validation;
- security review;
- authorization by an eligible role or quorum;
- a new effective boundary;
- publication in transparent history.

The period of suspension remains part of Historical Key State.

A Signature within the suspension interval must not become valid merely because the key was later reactivated, unless a specific governed correction establishes a different effective boundary and preserves the original and corrected evidence.

---

# 11.25 Revocation

**Revocation** is a governed transition that prohibits future reliance on a key for specified purposes from an applicable effective boundary and may also change interpretation of earlier activity when supported by compromise or corrective evidence.

Revocation should identify:

- the affected KID and purposes;
- revocation reason category;
- effective boundary;
- record and publication times;
- authorizer and supporting evidence;
- known compromise interval where applicable;
- successor or recovery information where permitted.

Revocation is normally terminal for the affected key-purpose relationship. Reuse after revocation requires a new, explicitly permitted relationship and must not obscure the earlier terminal event.

A later revocation does not automatically invalidate every earlier Signature.

---

# 11.26 Retirement, Supersession, and Expiration

**Retirement** is a planned cessation of key use. **Supersession** indicates replacement by another key or state. **Expiration** is the end of a validity interval under a defined rule.

These states differ from revocation and compromise:

```text
Retired     = planned end of use
Superseded  = replaced in a defined relationship
Expired     = validity interval ended
Revoked     = reliance prohibited by accountable action
Compromised = private-key security may have failed
```

A key may be retired without evidence of compromise. A key may expire while its historical Signatures remain valid for their original intervals. A superseded key may remain acceptable for another Key Purpose if that relationship continues.

Implementations must preserve the precise resulting state instead of collapsing all cases into `inactive`.

---

# 11.27 Compromise

**Compromise** is a condition in which confidentiality, integrity, exclusive control, or authorized use of private-key capability may have failed.

Compromise evidence may concern:

- confirmed private-key disclosure;
- suspected unauthorized signing;
- duplicated or exported key material;
- compromised hardware or key-management control;
- unauthorized administrative access;
- algorithmic or implementation failure;
- loss of custody assurance;
- an unknown-start exposure interval.

A compromise report is not identical to revocation. The report describes security-relevant knowledge; revocation changes governed reliance state.

Trust Profiles must define how confirmed, suspected, disputed, and bounded compromise claims affect Verification Outcomes.

---

# 11.28 Compromise-Time Semantics

Compromise may involve several distinct times:

- **Compromise Effective Time** — the earliest supported boundary at which exclusive or authorized control may have failed;
- **Discovery Time** — when the compromise was detected;
- **Report Time** — when the compromise claim was recorded;
- **Publication Time** — when the claim entered a transparency view;
- **Revocation Effective Time** — when governed reliance was prohibited.

```text
Compromise Effective Time
≠
Discovery Time
≠
Report Time
≠
Revocation Effective Time
```

Backdating a compromise boundary requires supporting evidence and an explicit confidence or uncertainty model. A party must not silently choose the time that produces its preferred Signature conclusion.

If the earliest compromise boundary is unknown, the uncertainty must remain visible.

---

# 11.29 Recovery

**Recovery** is the governed process for restoring trustworthy key control or identity continuity after loss, compromise, lockout, or administrative failure.

Recovery may use:

- a recovery key;
- a threshold or organizational quorum;
- an identity proofing process;
- hardware-rooted evidence;
- a regulated or contractually governed authority;
- an independently preserved recovery credential;
- a combination defined by a Trust Profile.

Recovery must identify the compromised or unavailable state, the successor state, authorizing evidence, effective boundary, and any restrictions.

Recovery power is high-impact Authority. Its holders, independence, custody, and historical eligibility must be transparent enough for the applicable assurance claim.

Recovery must not rewrite the prior history as though the incident never occurred.

---

# 11.30 Loss, Destruction, and Unavailability

Loss or destruction of private-key capability is distinct from compromise.

A key may be:

- unavailable but believed confidential;
- intentionally destroyed after retirement;
- inaccessible because a device or service failed;
- recoverable through an approved backup;
- suspected copied before destruction;
- publicly resolvable even though private material no longer exists.

Private-key destruction can reduce future misuse risk but does not delete public-key history or invalidate prior Signatures.

If loss prevents a required transition Signature, a governed recovery path may authorize the next state. The inability to prove a negative—such as absence of copied material—must not be represented as confirmed non-compromise.

---

# 11.31 Delegated Signing Relationships

A Protocol Identity may delegate signing capability to another identity, service, device, or custodian.

Delegation evidence should bind:

- delegator and delegate PIDs;
- affected KIDs;
- permitted Key Purposes and action scopes;
- start and end boundaries;
- delegation and revocation Authority;
- custody model;
- applicable Policy and Trust Profile;
- subdelegation rules;
- transparency and Preservation requirements.

Delegation does not merge the identities of delegator and delegate.

```text
Delegated signing capability
≠
Transfer of Protocol Identity
≠
Unlimited business Authority
```

Historical Verification must determine whether the delegation and the delegate key were both valid for the specific action at the relevant boundary.

---

# 11.32 HSM, KMS, and Custody References

Keys may be generated or held in a Hardware Security Module (HSM), Key Management Service (KMS), secure enclave, threshold system, or offline device.

Key Transparency may preserve accountability-relevant custody evidence, such as:

- logical key-version references;
- device or module attestation;
- exportability status;
- administrative control changes;
- backup and recovery configuration;
- custody-domain migration;
- key destruction evidence;
- provider or service version.

An internal KMS alias is not a durable KID unless its namespace, versioning, reassignment, and resolution semantics prevent historical ambiguity.

Vendor status and hardware attestation may support custody claims. They do not replace identity binding, Key Purpose, Authority, or transparent lifecycle history.

---

# 11.33 Multiple Keys, Key Sets, and Threshold Signing

A Key Subject may legitimately use multiple keys concurrently.

The historical model must distinguish:

- independent keys for different Key Purposes;
- redundant keys for one purpose;
- members of a governed key set;
- threshold-signature participants;
- an aggregate public key;
- an epoch-specific group configuration;
- emergency or recovery keys.

For threshold or multisignature arrangements, the KTR history must identify the governed verification key or key set, threshold rule, membership state, and transition authorization at the relevant boundary.

A change in membership, threshold, aggregation method, or group public key is an accountability-relevant transition even if the displayed service identity remains unchanged.

Counting cryptographic shares does not establish organizational independence unless control relationships satisfy the Trust Profile.

---

# 11.34 Algorithm and Parameter Constraints

Historical Key State includes the algorithm context required to evaluate the key.

That context may include:

- algorithm identifier;
- parameter set;
- curve or modulus constraints;
- Signature encoding;
- digest algorithm;
- cryptographic suite version;
- allowed Key Purposes;
- minimum strength and deprecation rules;
- applicable validation profile.

A key may be active in lifecycle terms but unacceptable under the algorithm policy applicable to the Signature or Verification Context.

```text
Lifecycle Active
≠
Cryptographically Acceptable
```

Algorithm identifiers must be unambiguous and governed. Guessing an algorithm from key length, encoding, or implementation defaults is prohibited where interpretation affects Verification.

---

# 11.35 Canonicalization and Cryptographic Protection

Every KTR must follow the common canonicalization, identifier, digest, and Signature-coverage rules applicable to its Protocol Object type.

Cryptographic protection must bind all fields whose alteration could change:

- Key Subject;
- KID or public-key material;
- Key Purpose;
- prior or resulting state;
- effective-time semantics;
- transition authorization;
- predecessor relationships;
- algorithm context;
- applicable Policy or profile;
- transparency scope.

Transport metadata, database timestamps, or resolver-added labels are not protected merely because they accompany a signed record.

Detached protection is permitted only when the binding to the exact canonical KTR remains deterministic and verifiable.

---

# 11.36 Time Semantics

Key Transparency must preserve distinct time claims where they affect interpretation.

Relevant times may include:

- Event Time asserted by a source;
- transition Effective Time;
- KTR Creation Time;
- Registry Acceptance Time;
- Log Integration Time;
- Publication Time;
- Observation Time;
- Checkpoint Time;
- compromise Discovery Time;
- Verification Time.

```text
Effective Time
≠
Record Time
≠
Publication Time
≠
Verification Time
```

The evidence supporting each time claim must be identifiable. A service clock alone may provide ordering within a Control Domain but does not automatically provide independent or trusted time.

---

# 11.37 Effective Boundaries and Retroactivity

A lifecycle transition may take effect at:

- a stated timestamp;
- a KTR sequence position;
- a log integration index;
- a signed epoch boundary;
- a Checkpoint;
- another deterministic boundary defined by TAIP.

The applicable boundary must be precise enough for reproducible Signature classification.

Retroactive transitions require explicit rules. They may be necessary when a compromise is discovered after unauthorized use, but they create substantial dispute and abuse risk.

A retroactive assertion must preserve:

- the originally published history;
- the later assertion and publication time;
- the claimed earlier effective boundary;
- evidence and confidence supporting that boundary;
- the rule governing its effect on earlier Signatures.

Silent retroactive mutation is prohibited.

---

# 11.38 Lifecycle Ordering

Key lifecycle interpretation depends upon authenticated ordering, not timestamps alone.

Ordering may derive from:

- predecessor references;
- per-subject sequence numbers;
- Key Transparency log indices;
- Hash Chain positions;
- Checkpoints;
- witnessed observations;
- another governed partial-order mechanism.

Concurrent transitions may be valid when they affect independent keys or purposes. Conflicting transitions affecting the same state require explicit resolution.

Implementations must detect impossible or unauthorized sequences, such as:

- activation before registration;
- reactivation without suspension;
- transition after a terminal state without a permitted recovery rule;
- overlapping sequence numbers;
- predecessor mismatch;
- purpose grant by an ineligible authorizer.

---

# 11.39 Append-Only Key History

Accountability-relevant key history must be corrected through additional evidence rather than silent replacement.

```text
Original KTR
      │
      ├── remains preserved
      ▼
Correction or Superseding KTR
```

Append-only behavior does not require every implementation to use one physical append-only database. It requires that the protocol representation make deletion, substitution, truncation, or conflicting succession detectable relative to available commitments and proofs.

A correction must identify what it corrects, which semantics change, why the change is authorized, and which historical interpretations are affected.

Administrative convenience, privacy operations, or storage migration must not silently convert an earlier state into a different historical fact.

---

# 11.40 Conflict, Fork, and Gap Detection

A Key Transparency conflict exists when available evidence supports incompatible histories for the same scope.

Examples include:

- two different keys assigned the same exclusive purpose and epoch;
- multiple successor transitions from one predecessor where only one is permitted;
- contradictory compromise boundaries;
- a record present in one committed view but absent from a claimed consistent successor;
- duplicate sequence positions;
- inconsistent identity or key material for one KID;
- an unexplained gap in a required sequence.

The verifier must not silently select the most convenient branch.

Conflicts must produce an explicit Verification Outcome, identify the competing evidence, and preserve any deterministic profile rule used to narrow or resolve the result.

---

# 11.41 Split Views and Equivocation

A **split view** occurs when a Key Transparency operator presents inconsistent histories or commitments to different parties.

**Equivocation** includes issuing incompatible signed views for what is represented as the same log size, epoch, subject sequence, or other exclusive state boundary.

Mitigations may include:

- signed Key Transparency Checkpoints;
- inclusion and consistency proofs;
- independent Monitors;
- Witness Observations;
- gossip among clients or operators;
- External Anchors;
- cross-logging;
- portable dispute evidence.

A locally valid proof does not exclude a split view that the verifier has never compared with another view.

Trust Profiles must define which anti-equivocation controls are required for the claimed assurance level.

---

# 11.42 Key Transparency Service and Log

A **Key Transparency Service** accepts, validates, orders, publishes, resolves, or proves KTR history.

It may use:

- an append-only Merkle tree;
- a Hash Chain;
- a verifiable map plus append-only history;
- a database with signed commitments and reproducible proofs;
- a federation or cross-log design;
- another mechanism satisfying TAIP requirements.

No particular data structure is mandatory at the architectural layer.

The service must define:

- log or history identifier;
- operator PID;
- accepted KTR types;
- ordering and integration rules;
- commitment and proof semantics;
- publication cadence;
- conflict behavior;
- availability and Preservation expectations;
- version and migration rules.

---

# 11.43 Key Transparency Checkpoints

A **Key Transparency Checkpoint (KT Checkpoint)** is a signed commitment to a defined Key Transparency history or state at a governed boundary.

It may commit to:

- log identifier and version;
- tree root, map root, Chain Head, or equivalent digest;
- log size, sequence range, or epoch;
- commitment algorithm;
- prior KT Checkpoint;
- issuance and publication time semantics;
- operator identity;
- Witness or external-anchor references.

A KT Checkpoint is distinct from a Checkpoint over TrustAgentAI Evidence Hash Chains.

```text
Key Transparency Checkpoint
≠
Evidence Hash Chain Checkpoint
```

The two may cross-commit or be packaged together, but their target scopes, authorities, proof rules, and Verification conclusions remain explicit.

---

# 11.44 Inclusion Proofs

An **inclusion proof** demonstrates that a defined KTR or key-state commitment is included in the history represented by a specific KT Checkpoint.

The proof must bind:

- the exact leaf or record commitment;
- canonical leaf construction;
- log identifier and version;
- commitment algorithm;
- position or key where relevant;
- target KT Checkpoint;
- proof path or equivalent verification material.

Successful inclusion proves membership within the target commitment. It does not by itself prove that the KTR was authorized, semantically valid, unique, timely, or complete.

```text
Included Record
≠
Valid Transition
```

Proof verification must reject ambiguous encoding, mismatched roots, unsupported algorithms, or proof reuse across incompatible log namespaces.

---

# 11.45 Consistency Proofs

A **consistency proof** demonstrates that a later append-only history commitment extends an earlier commitment without modifying or removing the earlier history, according to the log's defined construction.

The proof must identify:

- earlier and later KT Checkpoints;
- their log sizes, epochs, or ranges;
- log identifier and algorithm version;
- proof material;
- any migration boundary affecting comparison.

Successful consistency verification supports append-only extension between the two committed states.

It does not prove that no split view exists outside the compared checkpoints, that every expected KTR was submitted, or that included records are valid.

An operator must not call two checkpoints consistent merely because the later timestamp is greater or both Signatures verify.

---

# 11.46 Non-Inclusion and Completeness Claims

A non-inclusion proof may demonstrate that a defined key or record is absent from a particular committed map or set under defined semantics.

Non-inclusion does not automatically prove that:

- the key was never registered in any view;
- no unintegrated submission existed;
- the subject had no other identifier;
- no equivalent record existed under another encoding;
- the history is globally complete;
- the operator did not suppress a record.

Completeness claims require a governed closed-world scope, authenticated enumeration or map semantics, publication rules, and applicable monitoring evidence.

```text
Not found in view V
≠
Never existed anywhere
```

Resolvers must distinguish `proven absent`, `not found`, `unavailable`, `unknown scope`, and `unsupported proof`.

---

# 11.47 Monitors, Witnesses, and Gossip

A Key Transparency Monitor independently evaluates published KTRs, checkpoints, proofs, cadence, and consistency.

Monitoring may detect:

- unauthorized transitions;
- conflicting identity-key bindings;
- missed publication deadlines;
- inconsistent checkpoints;
- algorithm or schema violations;
- suspicious backdating;
- unresolved compromise reports;
- unavailable proof material.

A Witness may issue a Witness Observation over a KT Checkpoint or another precisely defined Key Transparency state, following the semantics of Chapter 9.

Gossip distributes signed checkpoints or comparison evidence among parties to expose split views.

Monitoring, Witnessing, and gossip are distinct. A service that merely receives a checkpoint is not necessarily an eligible Witness, and a Witness Signature does not prove full semantic monitoring unless its Observation Scope says so.

---

# 11.48 External Anchors and Cross-Commitments

A KT Checkpoint may be placed into an External Anchor or cross-committed into another independently governed history.

Anchoring can strengthen:

- evidence that a KT state existed by a supported boundary;
- resistance to unilateral checkpoint replacement;
- detection of delayed publication or rollback;
- cross-organizational comparison.

The Anchor Evidence must identify the exact KT commitment and follow Chapter 10 semantics.

An Evidence Hash Chain Checkpoint may reference a KT Checkpoint used to interpret its Signatures, and a KT history may record a reference to an Evidence Checkpoint. Such cross-links can strengthen historical composition without merging the object types.

External publication does not validate the underlying key transitions or prove log completeness.

---

# 11.49 Current Directory and Historical Resolution

A current key directory optimizes discovery of presently usable keys. It is not necessarily a historical source.

Historical resolution must identify:

- the Key Subject and target Signature;
- the relevant historical boundary;
- candidate KIDs and Key Purposes;
- ordered KTR history;
- applicable KT Checkpoint and proof material;
- algorithm and profile context;
- conflicts, gaps, or unavailable dependencies;
- the derived state and explanation.

```text
Current directory lookup
        └── answers: what is current?

Historical resolution
        └── answers: what was supported at boundary B?
```

A resolver response must distinguish original signed evidence from derived convenience fields and live operational metadata.

---

# 11.50 Historical Key Resolution Algorithm

A conforming historical resolver conceptually performs the following steps:

1. establish the Verification Context and target historical boundary;
2. resolve the Protocol Identity and Signature KID without assuming their equivalence;
3. retrieve authenticated key material and relevant KTRs;
4. validate object structure, canonicalization, identifiers, and Signatures;
5. validate transition authorization using the authorizers' historical state;
6. establish ordering and transparency proof state;
7. derive lifecycle and Key Purpose state at the boundary;
8. apply compromise, algorithm, Policy, and Trust Profile rules;
9. detect conflicts, gaps, unsupported semantics, and unavailable dependencies;
10. return the derived HKS with evidence references and a bounded outcome.

The algorithm must not convert missing evidence into a valid state by default.

---

# 11.51 Signature Verification Integration

Signature interpretation composes cryptographic verification with Historical Key State.

```text
Canonical Signed Input
          +
Signature Mathematics
          +
Exact Public Key
          +
Historical Lifecycle State
          +
Key Purpose
          +
Algorithm Policy
          =
Historical Signature Result
```

This result is then combined with Authority, Policy, object semantics, and other Trust Profile controls.

A mathematically valid Signature must not be reported as fully valid when the KID was unbound, inactive, suspended, revoked at the applicable boundary, unauthorized for the required purpose, algorithmically unacceptable, or historically indeterminate.

The Verification Report should identify which layer failed rather than reducing every failure to `bad signature`.

---

# 11.52 Authority and Policy Integration

Key Transparency supports but does not replace Authority evaluation.

The verifier may need evidence for:

- the subject's Authority to perform the Accountable Action;
- the Transition Authorizer's Authority to change key state;
- Policy rules applicable to both action and transition;
- delegation scope;
- organizational role or mandate;
- Trust Profile requirements;
- exceptions and approvals.

```text
Historically valid key
      +
Correct Key Purpose
      +
Valid Authority
      +
Applicable Policy satisfied
      =
Potentially authorized cryptographic action
```

The word `authorized` must not be used ambiguously for mere key activation when business Authority is a separate required conclusion.

---

# 11.53 Namespace, Federation, and Portability

TrustAgentAI permits multiple identity and Key Transparency namespaces.

Federated operation must define:

- globally or contextually unique log and PID namespaces;
- KID comparison behavior;
- authoritative and non-authoritative sources;
- cross-domain transition or delegation semantics;
- conflict handling;
- resolver trust boundaries;
- proof translation or preservation;
- migration and service-closure behavior.

A resolver must not merge records from different namespaces merely because display names, KIDs, or public-key bytes match.

Portable history should allow an authorized party to export the KTRs, checkpoints, proofs, dependencies, and metadata required for independent Verification without depending indefinitely upon one operator's proprietary API.

---

# 11.54 Algorithm Agility and Post-Quantum Migration

Key Transparency must support governed migration between cryptographic algorithms and suites.

Migration may involve:

- parallel classical and post-quantum keys;
- hybrid Signatures;
- new fingerprints or canonical representations;
- different key sizes and custody systems;
- a staged overlap interval;
- renewed Checkpoints or External Anchors;
- changed validation and Preservation dependencies.

A Migration Record or typed KTR must bind predecessor and successor state without rewriting the historical algorithm as though it had always been used.

Algorithm deprecation at Verification Time does not automatically mean an earlier Signature was invalid when created. The Trust Profile must define whether historical acceptance, warning, renewal evidence, or failure applies.

Long-lived evidence must retain algorithm identifiers and specifications needed for future interpretation.

---

# 11.55 Availability and Preservation

Historical key verification requires more than retaining public-key bytes.

Preservation may need to include:

- KTRs and their canonical representations;
- KT Checkpoints;
- inclusion and consistency proofs;
- identity and Authority dependencies;
- schemas and canonicalization rules;
- algorithm specifications;
- Policy and Trust Profile versions;
- registry snapshots;
- Witness Observations and Anchor Evidence;
- migration and correction records;
- resolver metadata needed to reproduce namespaces.

Cryptographic validity and availability remain distinct. A valid commitment to unavailable key history may detect loss without enabling complete Signature interpretation.

Retention, replication, recovery testing, format migration, and closure export must follow the Evidence Lifetime required by the applicable profile and law.

---

# 11.56 Privacy and Data Minimization

Key Transparency can reveal sensitive operational relationships even when it publishes no private keys.

Potentially sensitive information includes:

- identity membership;
- device or service inventory;
- rotation cadence;
- compromise and incident timing;
- organizational relationships;
- delegation and recovery structure;
- activity patterns inferred from lookups;
- long-term correlatable identifiers.

Architectures should minimize public disclosure while preserving required accountability through techniques such as opaque commitments, scoped namespaces, verifiable maps, selective disclosure, private retrieval, batched publication, or controlled resolution.

Privacy must not be implemented through silent deletion or falsification of committed history. The chosen design must state which parties can learn which facts and which completeness claims remain possible.

---

# 11.57 Selective Disclosure and Redaction

A KTR or proof may support selective disclosure when the cryptographic construction and Trust Profile permit it.

Selective disclosure must preserve the verifier's ability to determine:

- which claims are disclosed;
- which commitment binds the hidden claims;
- whether the disclosed claims were altered;
- which validation steps cannot be completed;
- whether an omission is intentional, unavailable, or unauthorized;
- which assurance conclusions remain supportable.

Redaction markers should be authenticated and typed. A blank field or missing record must not silently serve as proof of privacy-preserving omission.

A commitment to hidden identity or key data can still enable dictionary attacks when the value space is small. Salting, blinding, access control, or another mitigation may be required.

---

# 11.58 Validation Layers and Outcomes

Key Transparency Verification should report distinct validation layers:

- structural and schema validity;
- identifier and key-material integrity;
- KTR Signature validity;
- transition authorization;
- lifecycle ordering;
- Key Purpose state;
- transparency inclusion and consistency;
- conflict and completeness checks;
- time and compromise interpretation;
- algorithm acceptability;
- dependency availability;
- Trust Profile satisfaction.

Outcomes should distinguish at least:

- `valid`;
- `invalid`;
- `indeterminate`;
- `unsupported`;
- `unavailable`;
- `conflicting`;
- `valid-with-warnings` where defined by TAIP.

A successful lower-layer check must remain visible even when a higher-layer check fails.

---

# 11.59 Implementation Patterns

Conforming implementations may use different physical architectures.

Examples include:

- an identity registry with an append-only Merkle log;
- a verifiable key directory with signed map roots and history roots;
- per-identity Hash Chains aggregated into periodic KT Checkpoints;
- cross-organizational registries with Witnessed checkpoints;
- certificate issuance and revocation events imported into typed KTRs;
- offline organizational key books with signed epochs and external publication;
- threshold-controlled registries with portable proof bundles.

Interoperability depends upon shared semantics, not identical storage technology.

Each implementation must expose enough governed evidence for a verifier to reproduce its historical conclusion without trusting undocumented internal database state.

---

# 11.60 Anti-Patterns and Relationship to Other Controls

The following are architectural anti-patterns:

## Current-State Substitution

Using the key currently returned by a directory to verify every historical Signature.

## Identity Equals Key

Treating rotation as creation of an unrelated identity or treating a KID as permanent Protocol Identity.

## Registration Equals Activation

Permitting use immediately because a key record exists, despite a required activation control.

## Signature Equals Authority

Treating mathematical Signature validity as proof of business Authority or Policy compliance.

## Revocation Erases History

Declaring every earlier Signature invalid solely because the key is currently revoked.

## Mutable Status Row

Overwriting one database status field without preserving accountable transitions and historical boundaries.

## Timestamp-Only Ordering

Resolving conflicting transitions by comparing untrusted timestamps without authenticated ordering evidence.

## Log Inclusion Equals Validity

Treating inclusion in a transparency log as proof that the transition was authorized or semantically valid.

## No-Proof Not Found

Claiming a key never existed because one resolver returned no current record.

## Checkpoint Type Collapse

Treating a KT Checkpoint as an Evidence Hash Chain Checkpoint, or vice versa, without explicit target semantics.

## Same-Control Monitoring

Representing a replica or process under the operator's exclusive control as an independent Monitor or Witness.

## Silent Retroactivity

Changing an effective boundary without preserving the later assertion, supporting evidence, and resulting uncertainty.

## Opaque KMS Alias

Using a reassignable provider alias as a durable KID without version and key-material binding.

## Privacy by Deletion

Removing committed history without accountable erasure, retention, or legal-process evidence.

Key Transparency composes with other controls:

```text
Evidence Record Signature
          │
          ├── Key Transparency: historical key and purpose
          ├── Authority/Policy: permission for the action
          ├── Hash Chain: canonical protocol history
          ├── Witnesses: independent observation
          ├── Checkpoints/Anchors: historical commitments
          └── Preservation: future interpretability
```

No single layer substitutes for the others.

---

# Key Transparency Invariants

### INV-KT-001 — Historical/Current Separation

Current Key State MUST NOT silently replace Historical Key State when historical Verification requires the latter.

### INV-KT-002 — Identity/Key Separation

A Protocol Identity and a Key Identifier MUST remain distinct protocol concepts.

### INV-KT-003 — Key/Purpose Separation

Association of a key with an identity MUST NOT imply permission for every Key Purpose.

### INV-KT-004 — Key/Authority Separation

Key possession, key activation, and valid Signature creation MUST NOT by themselves establish business Authority.

### INV-KT-005 — Policy Separation

Historical key validity MUST NOT by itself establish satisfaction of the Policy governing an Accountable Action.

### INV-KT-006 — Exact Key-Material Binding

A KTR MUST bind the affected KID to exact key material or to an authenticated, unambiguous, persistently resolvable representation.

### INV-KT-007 — Registration/Activation Separation

Key registration and key activation MUST remain distinct where the applicable lifecycle model requires activation.

### INV-KT-008 — Typed Lifecycle State

Suspension, reactivation, retirement, supersession, expiration, revocation, compromise, recovery, and loss MUST NOT be collapsed when their semantics differ.

### INV-KT-009 — Purpose-Scoped Transition

A transition affecting one Key Purpose MUST NOT silently change another purpose.

### INV-KT-010 — Transition Attribution

Every accountability-relevant lifecycle transition MUST be attributable to its authorizing evidence and applicable historical context.

### INV-KT-011 — Non-Circular Bootstrap

A successor key MUST NOT establish its own replacement Authority without an independently governed registration, predecessor, delegation, or recovery rule.

### INV-KT-012 — Historical Rotation Continuity

Rotation MUST preserve the predecessor history and MUST NOT substitute the successor key into earlier Signature evaluation.

### INV-KT-013 — Append-Only Correction

Committed KTR history MUST be corrected through additional accountable state rather than silent in-place replacement.

### INV-KT-014 — Time-Dimension Separation

Effective Time, record time, publication time, discovery time, and Verification Time MUST remain distinct when the difference affects interpretation.

### INV-KT-015 — Authenticated Ordering

Lifecycle ordering MUST rely upon governed authenticated evidence rather than timestamps alone when order affects state.

### INV-KT-016 — Explicit Retroactivity

Retroactive lifecycle or compromise claims MUST preserve the original history, later assertion, claimed effective boundary, and supporting evidence.

### INV-KT-017 — Compromise Uncertainty

Unknown or disputed compromise boundaries MUST remain explicit and MUST NOT be converted into unsupported certainty.

### INV-KT-018 — Revocation Non-Erasure

A later revocation MUST NOT automatically invalidate every earlier Signature without an applicable historical rule and supporting evidence.

### INV-KT-019 — Terminal-State Integrity

A terminal state MUST NOT be silently reversed; any permitted recovery or new relationship MUST be represented as new accountable state.

### INV-KT-020 — Conflict Visibility

Incompatible key histories MUST produce explicit conflict evidence or a bounded non-success outcome rather than silent branch selection.

### INV-KT-021 — Checkpoint Type Separation

A Key Transparency Checkpoint and an Evidence Hash Chain Checkpoint MUST remain distinct even when cross-committed or co-packaged.

### INV-KT-022 — Inclusion/Validity Separation

KTR inclusion in a committed transparency history MUST NOT by itself establish semantic validity or transition authorization.

### INV-KT-023 — Consistency/Completeness Separation

Consistency between two KT Checkpoints MUST NOT by itself establish global completeness or absence of split views.

### INV-KT-024 — Non-Inclusion Scope

A non-inclusion result MUST remain bounded to its authenticated namespace, view, commitment, and proof semantics.

### INV-KT-025 — Split-View Awareness

Local proof validity MUST NOT be represented as proof that no incompatible view exists unless applicable anti-equivocation evidence supports that claim.

### INV-KT-026 — Delegation Separation

Delegation of signing capability MUST NOT merge delegator and delegate identity or imply Authority outside the delegated scope.

### INV-KT-027 — Custody Separation

HSM, KMS, or device custody evidence MUST NOT replace Protocol Identity, Key Purpose, Authority, or lifecycle evidence.

### INV-KT-028 — Algorithm Context

Historical Key State MUST preserve the cryptographic algorithm and parameter context required for reproducible evaluation.

### INV-KT-029 — Cryptographic/Availability Separation

Cryptographic validity of a KTR or KT Checkpoint and availability of the history it commits MUST remain distinct.

### INV-KT-030 — Deterministic Resolution

Equivalent evidence and Verification Context MUST produce equivalent Historical Key State or an explicit explanation of nondeterminism.

### INV-KT-031 — Explicit Missing Evidence

Missing, unavailable, unsupported, or redacted key-history dependencies MUST NOT be treated as satisfied controls.

### INV-KT-032 — Privacy Without Historical Falsification

Privacy controls MUST NOT silently falsify, delete, or mutate committed key history.

---

# Architectural Requirements

### REQ-KT-001

TAIP MUST define a versioned semantic model for Key Transparency Records and Historical Key State.

### REQ-KT-002

Every KTR MUST identify its Protocol Object type, version, stable identifier, and applicable namespace.

### REQ-KT-003

Every KTR affecting a key relationship MUST identify the Key Subject Protocol Identity and affected Key Identifier.

### REQ-KT-004

Key-binding records MUST authenticate the exact public-key material or a deterministic, persistently resolvable representation of it.

### REQ-KT-005

TAIP MUST define canonicalization, identifier, digest, and Signature-coverage rules for each KTR type.

### REQ-KT-006

KTR cryptographic protection MUST cover all properties whose alteration could change identity, key, purpose, lifecycle, time, authorization, or predecessor semantics.

### REQ-KT-007

The KTR model MUST represent Key Purpose independently of Protocol Identity and Key Identifier.

### REQ-KT-008

Trust Profiles MUST define which Key Purposes are required for each protected Protocol Object action.

### REQ-KT-009

The lifecycle model MUST define valid initial states, transitions, terminal states, reversible states, and transition preconditions.

### REQ-KT-010

Registration records MUST NOT imply activation unless the applicable profile explicitly defines registration and activation as one governed atomic transition.

### REQ-KT-011

Activation records MUST identify the purposes activated and the applicable effective boundary.

### REQ-KT-012

Transition Verification MUST evaluate the Transition Authorizer's Historical Key State, Key Purpose, Authority, and Policy where required.

### REQ-KT-013

Key rotation MUST identify predecessor and successor state, affected purposes, authorization evidence, and any overlap interval.

### REQ-KT-014

Rotation MUST preserve the predecessor key and its historical validity intervals for later Verification.

### REQ-KT-015

Suspension MUST be represented separately from revocation and MUST identify conditions or evidence governing any reactivation.

### REQ-KT-016

Reactivation MUST be a new accountable transition and MUST NOT remove the historical suspension interval.

### REQ-KT-017

Revocation MUST identify affected purposes, effective boundary, reason semantics, and authorizing evidence.

### REQ-KT-018

Retirement, supersession, expiration, revocation, and compromise MUST remain distinguishable in serialized and derived state.

### REQ-KT-019

Compromise records MUST distinguish supported compromise-effective, discovery, report, publication, and revocation boundaries where available.

### REQ-KT-020

Unknown or disputed compromise time MUST yield an explicit uncertainty range or indeterminate result under the applicable profile.

### REQ-KT-021

Recovery transitions MUST identify the recovery method, authorizing evidence, affected prior state, successor state, and effective boundary.

### REQ-KT-022

Delegation records MUST bind delegator, delegate, affected keys, purposes, scopes, intervals, and subdelegation behavior.

### REQ-KT-023

Threshold and key-set records MUST preserve membership, threshold, group-key, epoch, and transition semantics needed for historical evaluation.

### REQ-KT-024

Algorithm identifiers and parameters MUST be unambiguous, versioned where necessary, and evaluated under the applicable historical cryptographic profile.

### REQ-KT-025

The architecture MUST support algorithm migration without rewriting the original key or Signature history.

### REQ-KT-026

KTR time claims MUST identify their semantic type and MUST NOT be inferred from an unrelated storage or transport timestamp.

### REQ-KT-027

The applicable effective boundary for every lifecycle transition MUST be deterministically interpretable.

### REQ-KT-028

Retroactive transitions MUST identify the later assertion time, claimed earlier boundary, supporting evidence, and applicable interpretation rule.

### REQ-KT-029

Lifecycle histories MUST provide authenticated ordering sufficient to detect predecessor mismatch, duplicate positions, prohibited transitions, and relevant gaps.

### REQ-KT-030

Corrections MUST preserve the corrected KTR, identify the correction relationship, and state which semantics change.

### REQ-KT-031

Conflicting histories MUST be retained as dispute evidence and MUST produce an explicit Verification Outcome.

### REQ-KT-032

A Key Transparency Service MUST expose its log identifier, operator identity, version, accepted record types, ordering rules, commitment construction, and proof semantics.

### REQ-KT-033

KT Checkpoints MUST identify their exact target scope, log or history identifier, commitment value, size or epoch, algorithm, issuer, and applicable predecessor.

### REQ-KT-034

KT Checkpoint object typing and Verification MUST remain separate from Evidence Hash Chain Checkpoint typing and Verification.

### REQ-KT-035

Inclusion proofs MUST bind the exact record commitment, target KT Checkpoint, log namespace, position semantics, and proof algorithm.

### REQ-KT-036

Consistency proofs MUST identify the earlier and later KT Checkpoints and verify append-only extension under the defined log construction.

### REQ-KT-037

Implementations MUST NOT infer global completeness from inclusion or consistency proofs alone.

### REQ-KT-038

Non-inclusion results MUST distinguish cryptographically proven absence from ordinary resolver `not found`, unavailability, and unsupported proof semantics.

### REQ-KT-039

Trust Profiles claiming split-view resistance MUST define required Monitor, Witness, gossip, cross-log, or External Anchor controls.

### REQ-KT-040

Witness Observations over Key Transparency state MUST identify their exact Observation Scope and target KT commitment.

### REQ-KT-041

External anchoring of Key Transparency state MUST bind the exact KT commitment through verifiable Anchor Evidence.

### REQ-KT-042

Historical resolvers MUST accept or derive an explicit target boundary and MUST NOT default to Current Key State when historical state is required.

### REQ-KT-043

Historical resolution MUST return the derived lifecycle state, Key Purpose, supporting evidence references, conflicts, uncertainty, and missing dependencies.

### REQ-KT-044

Signature Verification MUST evaluate exact key material, Historical Key State, Key Purpose, and applicable algorithm policy in addition to Signature mathematics.

### REQ-KT-045

Verification Reports MUST distinguish cryptographic failure, inactive or prohibited lifecycle state, wrong Key Purpose, missing Authority, conflict, and unavailable history.

### REQ-KT-046

Key Transparency conclusions MUST NOT be reported as business Authority or Policy compliance unless those layers were independently evaluated.

### REQ-KT-047

Federated resolvers MUST preserve PID, KID, log, and proof namespaces and MUST NOT merge histories based only on display names or matching aliases.

### REQ-KT-048

Implementations MUST support portable export of KTRs, KT Checkpoints, proofs, and interpretation dependencies required by the applicable Trust Profile.

### REQ-KT-049

Preservation policy MUST cover key-history evidence and semantic dependencies for the required Evidence Lifetime.

### REQ-KT-050

Privacy controls, selective disclosure, and redaction MUST identify which checks remain possible and MUST preserve authenticated omission semantics.

### REQ-KT-051

Unsupported mandatory KTR semantics, algorithms, proof methods, or lifecycle transitions MUST produce an explicit non-success Verification Outcome.

---

# Security Considerations

Key Transparency addresses historical key interpretation but introduces its own high-value control plane.

## Unauthorized Registration

An attacker may attempt to bind its key to another Protocol Identity. Proof of possession does not prevent this attack because the attacker possesses the malicious key. Identity proofing, eligible registration Authority, subject notification, monitoring, and transparent inclusion may all be required.

## Malicious Rotation

A compromised active key may authorize a successor controlled by the attacker. Trust Profiles should define when rotation requires additional approval, recovery quorum, hardware evidence, delayed activation, or subject-visible monitoring.

## Recovery Takeover

Recovery mechanisms can bypass ordinary key controls. Recovery credentials and authorities therefore require strong custody, transparency, threshold design where appropriate, testing, and explicit historical eligibility.

## Key-Purpose Confusion

A valid key for one purpose may be replayed or misrepresented for another. Domain separation, typed KTRs, explicit Key Purpose identifiers, protocol-bound challenges, and purpose-aware Verification reduce this risk.

## Identifier Collision and Alias Reassignment

A KID collision or reassignable alias can cause Verification against the wrong key. Namespace, canonical key material, version, fingerprint algorithm, and collision behavior must be explicit.

## Backdated Activation

An operator or administrator may claim that a newly registered key was already active when a disputed Signature was produced. Authenticated ordering, publication evidence, Witnesses, and explicit retroactivity rules constrain this attack.

## Revocation Backdating Abuse

A party may backdate revocation or compromise to repudiate a legitimate earlier action. A compromise boundary must be supported by evidence and separated from discovery and publication time. Uncertainty should remain visible.

## Delayed Revocation

An operator may suppress or delay a revocation to preserve apparent key validity. Publication service-level rules, Monitors, subject receipts, Witnesses, and independently retained submissions can expose or bound delay.

## Split-View Attack

A transparency operator may show an authorized key history to the subject and a malicious history to a relying party. Signed KT Checkpoints, gossip, independent Monitors, Witness Observations, cross-logging, and External Anchors strengthen detection.

## Log Truncation and Rollback

An operator may present an earlier valid checkpoint as current after later revocations or corrections. Clients should enforce monotonic state where appropriate, compare checkpoint lineage, retain recent commitments, and distinguish stale from current views.

## Suppressed Submission

Append-only consistency does not prove that every valid submission was integrated. Submission receipts, maximum merge delay, Monitor access, non-inclusion proofs, escalation rules, and alternate publication paths may be required.

## Invalid but Included Transition

Transparency logs may record invalid or unauthorized data to make misbehavior visible. Verifiers must independently validate KTR semantics and authorization rather than treating inclusion as approval.

## Compromised Transparency Operator Key

Compromise of an operator checkpoint-signing key can support forged views. Operator keys themselves require Historical Key State, transparent rotation, Witness comparison, recovery rules, and Preservation of pre-compromise checkpoints.

## Circular Key Validation

A KTR may be signed by a key whose validity depends upon that same KTR. TAIP must define valid bootstrap trust roots and reject circular derivations that supply no independent foundation.

## Resolver Substitution

A resolver may substitute Current Key State, omit conflicting KTRs, or return a convenience object without proof. Verification should bind responses to authenticated records and commitments and report the resolver's trust role.

## Cache Staleness

Cached state may predate a suspension, revocation, or compromise report. Trust Profiles should define freshness, checkpoint cadence, offline Verification behavior, and the difference between `valid at cached boundary` and `currently valid`.

## Algorithm Downgrade

An attacker may replace an algorithm identifier, force acceptance of a deprecated suite, or remove hybrid components. Algorithm and parameter identifiers must be cryptographically bound and evaluated under applicable historical and current policy.

## Threshold-Control Correlation

Multiple threshold shares may be controlled by one administrator, cloud account, or compromised deployment. Cryptographic threshold and organizational independence are separate assurance properties.

## Denial of History

An operator may withhold KTRs, proofs, schemas, or old key material. Preservation, replication, portable exports, proof caches, and closure procedures reduce dependence on a single live service.

## Evidence Bombs and Resource Exhaustion

An attacker may submit deeply linked histories, oversized keys, excessive rotations, or expensive proof structures. Implementations should enforce governed bounds, streaming validation, recursion limits, algorithm cost limits, and safe failure outcomes without truncating evidence silently.

## Parser and Canonicalization Divergence

Different implementations may derive different KTR identifiers or signed inputs. Strict schemas, deterministic canonicalization, test vectors, duplicate-member rejection, and version-specific validation are required.

## Cross-Namespace Confusion

The same PID text, KID, or key bytes may occur in multiple namespaces. Every resolution and proof must preserve its namespace and operator context.

## Private-Key Exposure

Key Transparency publishes public-key and lifecycle evidence. It must never require publication of private keys, secret shares, recovery secrets, or unredacted custody credentials.

## Monitoring Blind Spots

A Monitor may validate checkpoint consistency but not transition authorization, or vice versa. Monitor scope, coverage interval, supported rules, and failures must be explicit.

## False Finality

An anchored or witnessed KT Checkpoint can later lose availability, be reorganized, or be exposed as one split view. Verifiers must apply the finality and proof limitations of the actual external system.

## Time-Source Manipulation

Operator timestamps may be moved to change validity intervals. Ordering commitments, independent observations, bounded-clock rules, and external publication can constrain but not automatically prove Event Time.

## Erasure and Legal-Process Conflict

Deletion duties may conflict with append-only accountability. Deployments should minimize committed personal data, separate payload from commitment, define retention authority, and use accountable tombstone, encryption, or erasure evidence where legally and technically appropriate. The protocol conclusion after erasure must remain honest about unavailable content.

---

# Privacy Considerations

Key Transparency improves accountability by making relationships inspectable. That same inspectability can create correlation and surveillance risk.

## Public-Key Data Is Not Necessarily Anonymous

A public key, fingerprint, KID, or stable opaque identifier can become personal or commercially sensitive when linked to a person, service, device, account, or transaction history.

## Rotation Patterns

Rotation cadence may reveal deployments, security incidents, employee changes, infrastructure migration, or operational tempo. Batching and scoped publication may reduce unnecessary timing precision.

## Incident Disclosure

Compromise reasons and recovery details can expose vulnerabilities. A KTR may disclose a governed reason category or commitment while restricting detailed incident evidence to authorized Verification contexts.

## Delegation Graphs

Public delegation and recovery relationships can reveal organizational structure and high-value recovery targets. Designs should disclose only the relationship detail required for the assurance claim.

## Query Privacy

Resolver lookups reveal which identities, disputes, or historical periods interest a verifier. Private information retrieval, local replicas, batched requests, proxies, or offline Dispute Packs may reduce query leakage.

## Namespace Separation

Context-specific identifiers can reduce cross-domain correlation. Federation must not silently defeat that protection by merging histories on public-key equality or display-name similarity.

## Commitment Guessing

Hashing a low-entropy identity, email address, device serial, or status does not make it private. Randomized commitments, salts, keyed constructions, or access-controlled proofs may be required.

## Selective Disclosure Limits

Selective disclosure can hide fields while revealing the existence, timing, and shape of an event. Privacy evaluation must consider metadata and traffic patterns, not only plaintext payloads.

## Data Subject Access and Correction

Where applicable, deployments should provide governed mechanisms to access, contest, correct, or restrict key-history assertions. Correction preserves the original accountability trail while adding the authorized corrected interpretation.

## Retention Proportionality

Preserving every operational detail indefinitely is not required merely because key history is append-only. Trust Profiles and legal governance should define the minimum semantics and Evidence Lifetime necessary for historical Verification.

## Private Logs

A Key Transparency service may be access-controlled rather than public. Private access does not remove requirements for append-only accountability, conflict detection, proof semantics, Preservation, or independent Verification within the governed audience.

## Disclosure in Dispute Packs

Dispute Packs should include only key-history evidence necessary for the focal claims and should identify redactions, handling restrictions, and unavailable dependencies. A verifier must be able to distinguish privacy-limited evidence from complete public history.

Privacy strength and transparency strength are not a single continuum. A carefully designed commitment and proof architecture can provide both bounded disclosure and strong historical accountability, but the supported claims must remain explicit.

---

# Design Rationale

## Why Protocol Identity Is Not the Key

Keys rotate, expire, fail, migrate, and become compromised. A durable Protocol Identity preserves accountable continuity across those events while allowing exact keys to remain historically identifiable.

## Why Current State Is Insufficient

A current directory answers an operational question: which key should be used now? Historical Verification asks a different question: which key state was supported at the relevant past boundary? Substituting the first answer for the second can both reject valid old Signatures and accept unauthorized ones.

## Why Registration and Activation Are Separate

Many deployments require approval, hardware validation, proof of possession, delayed publication, or a dual-control step between introducing and using a key. The model permits an atomic profile where appropriate but does not erase the architectural distinction.

## Why Lifecycle States Are Typed

Suspension, retirement, expiration, revocation, and compromise carry different implications for past and future reliance. A generic active/inactive flag cannot support reproducible disputes.

## Why Compromise Has Multiple Times

Security incidents are often discovered after they begin. Separating effective, discovery, report, publication, and revocation times prevents a later administrative timestamp from silently determining the whole historical conclusion.

## Why History Is Append-Only

Disputes concern not only the latest answer but also what parties were told and when. Preserving original and corrective evidence exposes repudiation, backdating, suppression, and operator error.

## Why Transparency Inclusion Is Not Approval

A log can provide useful accountability by recording even an invalid submission. Conflating inclusion with authorization would require the log operator to become the universal source of business truth and would hide the difference between publication and validation.

## Why KT Checkpoints Are Distinct

An Evidence Hash Chain Checkpoint commits protocol evidence history. A KT Checkpoint commits key-history state used to interpret cryptographic actors. Cross-commitment is useful, but type collapse would obscure target scope, authority, cadence, and proof meaning.

## Why No Universal Log Is Required

TrustAgentAI must operate across Organizations, jurisdictions, identity technologies, confidentiality regimes, and resilience models. Interoperability therefore rests on stable record, commitment, proof, and outcome semantics rather than one global operator.

## Why Authority Remains Separate

A key can be correctly bound, active, and purpose-eligible while its subject lacks permission for the business action. Keeping Authority and Policy separate prevents cryptographic control from silently becoming organizational power.

## Why Portability Matters

Historical Verification may occur after an operator closes, changes products, loses credentials, or becomes a dispute party. Portable KTRs, proofs, checkpoints, and dependencies reduce indefinite trust in one live resolver.

## Why Privacy Is Architectural

Retrofitting privacy after globally publishing stable identity-key graphs is often impossible. Namespace, commitment, access, proof, and retention choices must therefore be made as part of the Key Transparency architecture.

---

# Summary

Key Transparency preserves the historical evidence required to interpret cryptographic keys across registration, activation, rotation, suspension, reactivation, retirement, expiration, revocation, compromise, recovery, delegation, and migration.

The central distinctions are:

```text
Protocol Identity
≠
Key Identifier
≠
Key Purpose
≠
Authority
≠
Policy
```

and:

```text
Historical Key State
≠
Current Key State
```

A Key Transparency Record represents an accountable identity-key relationship or lifecycle transition. A transparent history makes corrections, conflicts, rollback, and equivocation detectable. KT Checkpoints, inclusion proofs, consistency proofs, Monitors, Witnesses, gossip, and External Anchors can strengthen that history, but none substitutes for semantic validation or transition authorization.

Historical Signature Verification must combine exact key material, lifecycle state, Key Purpose, algorithm context, and a defined historical boundary. Authority, Policy, evidence truth, and broader Trust Profile controls remain separate evaluations.

A conforming TrustAgentAI implementation therefore preserves key history rather than overwriting it, reports uncertainty rather than guessing, protects privacy without falsifying the record, and retains portable evidence sufficient for future independent Verification.
