# Chapter 10 — Checkpoints and External Anchors

> **A Checkpoint creates a stable commitment to defined TrustAgentAI history; an External Anchor carries that commitment across a separate trust or control boundary.**

## Purpose

This chapter defines the architectural specification for TrustAgentAI **Checkpoints (CP)**, **Checkpoint Authorities (CA)**, **External Anchors (EA)**, and **Anchor Evidence (AE)**.

Checkpoints create compact, governed historical boundaries that can be retained, compared, witnessed, externally published, and used as starting points for later Verification.

External Anchors strengthen resistance to unilateral history rewrite by placing a TrustAgentAI commitment into a system governed through another administrative, technical, organizational, or public trust boundary.

This chapter establishes:

- Checkpoint scope and target-state semantics;
- Checkpoint Authority identity and responsibility;
- Checkpoint construction, cadence, issuance, and publication;
- Chain, range, Witness quorum, Registry, and profile commitments;
- Checkpoint conflict, correction, succession, and Verification;
- External Anchor selection and trust-boundary analysis;
- Anchor Evidence and publication-state semantics;
- Publication Time, confirmation, finality, rollback, and reorganization handling;
- multiple-anchor, privacy, Preservation, and portability behavior;
- Checkpoint and anchor invariants;
- architectural requirements for interoperable implementations.

This chapter applies the common Protocol Object rules defined in [06-Protocol-Objects.md](06-Protocol-Objects.md).

It builds upon the Hash Chain model in [08-Hash-Chain-Specification.md](08-Hash-Chain-Specification.md) and the Witness model in [09-Witness-Observation-Specification.md](09-Witness-Observation-Specification.md).

It also applies the Evidence Record model in [07-Evidence-Record-Specification.md](07-Evidence-Record-Specification.md), the system model in [05-System-Overview.md](05-System-Overview.md), and the principles in [04-Design-Principles.md](04-Design-Principles.md).

It uses the canonical terms defined in [Terminology.md](Terminology.md) and [Acronyms.md](Acronyms.md).

This chapter defines architectural semantics.

It does not require blockchain and does not define final field names, concrete schemas, media types, wire encodings, canonicalization algorithms, Signature suites, ledger-specific transaction formats, confirmation rules, transport endpoints, or jurisdiction-specific legal conclusions.

Those details belong to TAIP, Trust Profiles, governed registries, cryptographic profiles, anchor bindings, APIs, and test vectors.

---

# 10.1 Checkpoint Definition

A **Checkpoint (CP)** is a cryptographically protected Protocol Object that commits to defined historical TrustAgentAI state at a governed boundary.

A Checkpoint may bind:

- one or more Chain Identifiers;
- Chain Heads;
- Chain Entry positions or ranges;
- Witness Observations or Witness Quorum evidence;
- Registry, Policy, profile, or governance state;
- historical version and algorithm context;
- time semantics;
- Verification dependencies;
- prior Checkpoint state;
- extensions.

```text
Historical Protocol State
          │
          ▼
  Checkpoint Candidate
          │ validate and sign
          ▼
      Checkpoint
```

A Checkpoint is a compact commitment to a bounded state.

It does not replace the underlying state or dependencies.

---

# 10.2 External Anchor Definition

An **External Anchor (EA)** is a system, service, registry, ledger, publication channel, or accountable record outside the protected history's relevant Control Domain into which a TrustAgentAI commitment is placed.

**Anchor Evidence (AE)** is the governed evidence that relates the TrustAgentAI commitment to the external representation.

```text
TrustAgentAI Checkpoint
          │ typed commitment
          ▼
   External Anchor System
          │ publication / confirmation evidence
          ▼
      Anchor Evidence
```

The External Anchor may use a non-TAIP-native format.

Anchor Evidence provides the interpretable bridge between the external record and the TrustAgentAI state it is claimed to protect.

---

# 10.3 Scope

This chapter applies to Checkpoints and External Anchors used to protect:

- one Hash Chain;
- multiple Hash Chains;
- a Chain range or epoch boundary;
- an aggregate of shard or tenant Chain Heads;
- Witness Quorum evidence;
- a Registry or governance-state boundary;
- a migration or closure boundary;
- another historical Protocol Object set defined by TAIP.

An implementation may create Checkpoints without external anchoring.

It may externally anchor a Chain Head or another governed commitment without first creating a Checkpoint where the applicable binding permits that behavior.

The Trust Profile determines which layers are required and how their evidence composes.

---

# 10.4 Security Objectives

Checkpointing has two primary security objectives:

1. create a stable, compact, governed boundary for historical evaluation;
2. make later rollback, truncation, fork substitution, or state rewrite detectable relative to that boundary.

External anchoring adds a third objective:

3. place the commitment beyond the unilateral control of the party whose history is being protected.

The relevant architectural statement is:

> A verifier can determine which historical state was checkpointed, by which Authority, under which rules, when, whether and where it crossed another trust boundary, and which limitations remain.

Achievement depends upon correct target binding, historical Authority and keys, time evidence, anchor independence, availability, and Preservation.

---

# 10.5 What a Checkpoint Establishes

Subject to successful Verification, a Checkpoint may establish that:

- an identifiable Checkpoint Authority committed to a defined historical state;
- the target state was bound under a defined Checkpoint scope and version;
- a specific Chain Head, range, aggregate, Witness Quorum, or Registry state was included;
- the Checkpoint followed a defined predecessor or cadence rule;
- the Checkpoint was created under applicable historical identity, key, and governance state;
- the target state existed no later than a supported later publication or anchor boundary;
- a verifier may use the Checkpoint as a trusted or evaluated historical boundary under an applicable Trust Profile.

The exact conclusion depends upon the Checkpoint type and available Verification dependencies.

---

# 10.6 What a Checkpoint Does Not Establish

A valid Checkpoint does not by itself prove:

- truth of the underlying Evidence Records;
- valid business Authority or Policy;
- evidence Completeness;
- availability of all committed objects;
- eligibility or independence of referenced Witnesses;
- absence of an undisclosed competing Checkpoint;
- independence of the Checkpoint Authority;
- accuracy of a self-asserted Checkpoint Time;
- external publication;
- Legal Validity or Regulatory Compliance.

```text
Valid Checkpoint
≠
Complete Evidence
≠
Independent Checkpoint
≠
Externally Anchored State
≠
Business Truth
```

---

# 10.7 What an External Anchor Establishes

Subject to successful Verification, an External Anchor and valid Anchor Evidence may establish that:

- a defined TrustAgentAI commitment was represented in a defined external namespace;
- the external record was published, accepted, confirmed, finalized, or otherwise reached a governed state;
- the commitment existed no later than a supported external time boundary;
- unilateral rewrite would require compromise or cooperation across additional trust or Control Domains;
- an external record can be retrieved or proven through governed evidence;
- later TrustAgentAI state can be compared with the externally retained commitment.

The strength of the conclusion depends upon the external system's control, finality, time, availability, and proof model.

---

# 10.8 What an External Anchor Does Not Establish

An External Anchor does not by itself prove:

- meaning or validity of the committed TrustAgentAI content;
- Authority of the Checkpoint Authority or Chain operator;
- Witness eligibility or quorum;
- completeness of the underlying history;
- correctness of Event Time or Commitment Time;
- permanence of external availability;
- independence merely because the system is called external;
- absence of reorganization, rollback, deletion, or censorship;
- Legal Validity or Regulatory Compliance.

```text
External Publication
≠
Checkpoint Validity
≠
Underlying Evidence Validity
```

---

# 10.9 Lifecycle Separation

The following states must remain distinct:

- Chain Commitment;
- Witnessing;
- Checkpoint candidate creation;
- Checkpoint issuance;
- Checkpoint publication;
- anchoring submission;
- external acceptance;
- external publication;
- external confirmation or finality;
- Preservation;
- Verification.

```text
Committed
≠
Witnessed
≠
Checkpointed
≠
Anchored
≠
Preserved
≠
Verified
```

A system must not infer later lifecycle states from completion of an earlier state.

---

# 10.10 Checkpoint Authority

A **Checkpoint Authority (CA)** is the Protocol Identity responsible for issuing a Checkpoint under defined governance.

Its responsibilities may include:

- selecting a Checkpoint candidate;
- resolving target state;
- validating required Chain and Witness evidence;
- enforcing Checkpoint scope and cadence;
- constructing the canonical Checkpoint;
- signing and issuing the Checkpoint;
- publishing or distributing the Checkpoint;
- initiating external anchoring where required;
- preserving Checkpoint and Anchor Evidence;
- reporting failures, conflicts, and degradation;
- supporting historical Verification.

The Checkpoint Authority may be internal or external to the Chain operator.

Its actual control relationships determine the assurance it contributes.

---

# 10.11 Authority Identity, Keys, and Purpose

Every Checkpoint must identify the Checkpoint Authority according to governed Protocol Identity semantics.

Historical Verification may require:

- stable Authority identity;
- responsible Organization or operator;
- Key Identifier;
- Key Purpose;
- Historical Key State;
- algorithm and Signature suite;
- Checkpoint-issuance Authority;
- effective governance interval;
- suspension, revocation, compromise, or migration evidence.

```text
Valid Checkpoint Signature
≠
Authorized Checkpoint Authority
≠
Independent Checkpoint Authority
```

Current keys and governance state must not silently replace historical state.

---

# 10.12 Checkpoint Scope

Every Checkpoint must define a bounded **Checkpoint scope**.

Scope may identify:

- target namespace or CIDs;
- Chain positions, ranges, or epochs;
- included Chain Heads;
- Witness Quorum requirements;
- Registry or profile state;
- included and excluded object classes;
- coverage time or position boundary;
- aggregation method;
- predecessor Checkpoint;
- required interpretation dependencies;
- critical extensions.

Checkpoint scope is security-critical and must be versioned and cryptographically bound.

Free-form prose may supplement scope but must not replace machine-interpretable coverage semantics.

---

# 10.13 Checkpoint Types

TAIP or governed registries may define Checkpoint types.

Representative types include:

## Single-Chain Checkpoint

Commits to one Chain Head at a defined position.

## Range Checkpoint

Commits to a defined entry range and ending state.

## Multi-Chain Checkpoint

Commits to an authenticated aggregate of multiple CIDs and Chain Heads.

## Witnessed Checkpoint

Commits to target state together with governed Witness Quorum evidence.

## Registry-State Checkpoint

Commits to defined identity, key, profile, Policy, or governance Registry state.

## Migration or Closure Checkpoint

Commits to a terminal or transition boundary.

Different types must not be treated as interchangeable merely because they contain the same digest.

---

# 10.14 Checkpoint Target State

A Checkpoint must bind unambiguously to its target state.

The target binding may include:

- target type and namespace;
- CID;
- Chain Head;
- Chain position or range;
- epoch;
- batch or aggregate root;
- Witness Quorum commitment;
- Registry-state commitment;
- canonicalization and algorithm context;
- target version;
- source or operator identity where required.

A locator may assist retrieval.

It must not replace an integrity-bound target commitment.

---

# 10.15 Single-Chain Checkpoint

A Single-Chain Checkpoint binds one CID and one Chain Head at a defined historical position.

It should make it possible to determine:

- the CID and Chain version;
- the exact Chain Head;
- the covered position or boundary;
- the predecessor Checkpoint, if required;
- relevant Chain closure or epoch state;
- required Witness evidence;
- applicable Checkpoint Time semantics;
- the Checkpoint Authority and Signature.

The Checkpoint does not need to embed every Chain Entry.

The entries, proofs, and dependencies needed for later Verification must remain resolvable or be reported as unavailable.

---

# 10.16 Range Checkpoint

A Range Checkpoint commits to a governed interval of historical state.

Its semantics must define:

- starting boundary;
- ending boundary;
- included and excluded positions;
- continuity proof or Chain material;
- gap treatment;
- ordering scope;
- aggregation behavior;
- covered object classes where relevant;
- relationship to earlier and later ranges.

A Range Checkpoint must not imply validity of history preceding its starting boundary unless that boundary is independently trusted or verified.

A valid ending Chain Head does not prove availability of every entry in the range.

---

# 10.17 Multi-Chain and Aggregate Checkpoint

A Multi-Chain Checkpoint commits to multiple Chain states through a governed aggregate.

The aggregate must define:

- member CIDs;
- member Chain Heads and positions;
- deterministic member ordering;
- membership and omission rules;
- duplicate handling;
- aggregate construction;
- inclusion proofs;
- missing or unavailable Chain behavior;
- maximum cardinality and resource bounds.

```text
CID-A / Head-A ─┐
CID-B / Head-B ─┼──► Authenticated Aggregate ──► Checkpoint
CID-C / Head-C ─┘
```

The aggregate does not create a total order among member Chains unless its governed semantics explicitly define one.

---

# 10.18 Witness Quorum in a Checkpoint

A Checkpoint may bind Witness Observations or a Witness Quorum result.

The binding must identify or resolve:

- target state observed;
- applicable Observation Scope;
- quorum policy and version;
- contributing Witness Observations;
- eligibility and Control Domain evidence;
- threshold and timing result;
- excluded, late, duplicate, or conflicting observations;
- unresolved limitations.

A Checkpoint Signature does not transform an invalid or unsupported Witness set into a valid quorum.

Witness and quorum evaluation remains governed by Chapter 9 and the applicable Trust Profile.

---

# 10.19 Registry, Profile, and Governance State

A Checkpoint may commit to historical Registry, Trust Profile, Policy, or governance state needed for future interpretation.

Examples include:

- Witness Registry version;
- Protocol Identity Registry state;
- algorithm Registry state;
- Trust Profile definition;
- Checkpoint Authority configuration;
- Control Domain declarations;
- extension Registry state;
- migration or emergency governance state.

Committing to such state preserves integrity and historical placement.

It does not make the Registry assertion true or the Policy lawful.

The committed material must remain interpretable and available according to Preservation requirements.

---

# 10.20 Checkpoint Dependencies

A Checkpoint may depend upon material not embedded in the Checkpoint object.

Dependencies may include:

- Chain Entries and range proofs;
- Evidence Records or typed commitments;
- Witness Observations;
- quorum policies;
- historical schemas and registries;
- Checkpoint Authority identity and key history;
- algorithm and canonicalization rules;
- prior Checkpoints;
- Anchor Evidence;
- migration and correction records.

Mandatory dependencies must be integrity-governed and either resolvable or explicitly reported as unavailable.

A compact Checkpoint without recoverable interpretation dependencies may preserve a digest while failing its intended long-term assurance purpose.

---

# 10.21 Checkpoint Candidate

A **Checkpoint candidate** is proposed historical state prepared for validation and possible Checkpoint issuance.

Candidate state may include:

- target CIDs and Chain Heads;
- range or aggregate material;
- Witness Quorum evidence;
- applicable Checkpoint policy;
- expected predecessor Checkpoint;
- target versions and algorithms;
- proposed time boundary;
- completeness and availability status.

A candidate is not a Checkpoint.

```text
Checkpoint Candidate
≠
Issued Checkpoint
≠
Published Checkpoint
≠
Externally Anchored Checkpoint
```

Failed candidates may require accountable failure evidence under the applicable Trust Profile.

---

# 10.22 Checkpoint Logical Information Model

The logical Checkpoint model contains governed properties applicable to its type and version.

| Logical property group | Purpose |
|---|---|
| Type and version | Determines Checkpoint semantics |
| Checkpoint identifier | Provides stable governed reference |
| Checkpoint Authority | Attributes issuance |
| Checkpoint scope | Bounds coverage and meaning |
| Target state | Binds exact Chain, range, aggregate, or Registry state |
| Predecessor | Supports cadence and succession where defined |
| Witness context | Binds required observation or quorum evidence |
| Time properties | Distinguishes target, creation, issuance, and publication time |
| Policy and profile context | Identifies governing requirements |
| Dependencies | Preserves required interpretation material |
| Extensions | Adds governed optional or critical semantics |
| Cryptographic protection | Protects integrity and attribution |

The model is logical rather than a final wire schema.

---

# 10.23 Checkpoint Identifier

Every interoperable Checkpoint should possess a stable identifier under a governed namespace.

Identifier rules must establish:

- namespace and assignment responsibility;
- uniqueness and comparison behavior;
- relationship to Checkpoint Authority;
- relationship to target state;
- relationship to the canonical Checkpoint digest;
- persistence across publication, export, and migration;
- collision handling.

```text
Checkpoint Identifier
≠
Checkpoint Digest
≠
Chain Head
≠
External Transaction Identifier
```

Different protected Checkpoints associated with the same identifier must be surfaced as a conflict.

---

# 10.24 Canonical Representation

Every finalized Checkpoint participating in digest, Signature, publication, anchoring, Preservation, or Verification must have a deterministic canonical representation.

The applicable definition must address:

- Checkpoint type and version;
- identifier and Authority identity;
- scope and target state;
- predecessor relationship;
- Witness and policy context;
- time semantics;
- dependency commitments;
- absent, null, default, and unknown values;
- extension handling;
- domain separation;
- collection ordering;
- size and nesting limits.

Independent conforming implementations must derive equivalent cryptographic input from equivalent conforming Checkpoints.

---

# 10.25 Checkpoint Signature

A Checkpoint Signature protects the canonical Checkpoint statement and attributes use of a Checkpoint Authority signing capability.

Signature input must bind all substitution-sensitive semantics, including:

- object type and version;
- Checkpoint identifier;
- Checkpoint Authority identity;
- Key Identifier and Key Purpose;
- scope and target state;
- predecessor Checkpoint;
- Checkpoint Time semantics;
- policy and profile context;
- critical extensions.

A valid Signature does not prove correct candidate validation, Authority independence, underlying object validity, or external publication.

---

# 10.26 Checkpoint Time

**Checkpoint Time** is the time associated with Checkpoint creation, finalization, or issuance under defined semantics.

The applicable Checkpoint type must identify which meaning is used.

Its assurance depends upon:

- the Checkpoint Authority clock;
- precision and synchronization;
- target-state acquisition time;
- creation and signing delay;
- whether time is cryptographically bound;
- Witness, timestamp, publication, or anchor evidence;
- Trust Profile rules.

```text
Event Time
≠
Commitment Time
≠
Observation Time
≠
Checkpoint Time
≠
Publication Time
```

A Checkpoint Authority-supplied time is an attributable assertion unless strengthened by other evidence.

---

# 10.27 Checkpoint Cadence

A Checkpoint policy may require Checkpoints according to:

- time interval;
- Chain Entry count;
- data volume;
- epoch or batch boundary;
- risk threshold;
- Trust Profile;
- administrative event;
- emergency trigger;
- migration or closure.

Cadence rules must define:

- trigger semantics;
- permitted delay;
- grace interval;
- missed Checkpoint behavior;
- retry and recovery;
- publication and anchoring deadlines;
- effect on Achieved Trust Profile.

A later successful Checkpoint must not silently erase evidence that an earlier required cadence boundary was missed.

---

# 10.28 Atomic Checkpoint Creation

Checkpoint creation must bind one internally coherent target state.

For a single Chain, the Checkpoint must not combine a Chain Head from one position with metadata, Witness evidence, or range claims from another incompatible state.

For a multi-Chain Checkpoint, the applicable construction must define how member states are sampled or bounded.

Possible approaches include:

- one transactional snapshot;
- independently signed member snapshots with a governed collection window;
- a barrier protocol;
- a prior aggregate commitment;
- another deterministic method.

The sampling method and consistency limitations must be explicit.

---

# 10.29 Issuance, Publication, and Commitment

Checkpoint lifecycle states must remain distinct.

## Finalized

Canonical Checkpoint content was fixed.

## Issued

The Checkpoint Authority signed the Checkpoint.

## Published

The Checkpoint became available through a defined publication channel.

## Committed

The Checkpoint itself entered protected TrustAgentAI history where required.

## Anchored

The Checkpoint commitment reached the external state required by an anchor binding.

Publication through an Authority-controlled endpoint does not automatically constitute external anchoring.

---

# 10.30 Checkpoint Succession and Predecessors

A Checkpoint series may bind each Checkpoint to a predecessor Checkpoint or prior boundary.

Predecessor binding can support:

- cadence continuity;
- rollback detection;
- explicit sequence;
- Authority transition;
- policy and algorithm migration;
- closure and recovery.

The predecessor relationship must bind the exact prior Checkpoint commitment and applicable namespace.

Missing predecessor state must be distinguished from a governed genesis, reset, migration, or emergency recovery boundary.

Checkpoint succession does not replace the underlying Hash Chain.

It creates a separate compact history of selected boundaries.

---

# 10.31 Correction, Revocation, and Replacement

An issued Checkpoint must not be modified in place.

If a Checkpoint is later found to be incorrect, incomplete, compromised, or unsuitable, additional accountable evidence must identify:

- the affected Checkpoint;
- correction, revocation, supersession, or dispute semantics;
- responsible Authority;
- reason and effective boundary;
- corrected target state, if any;
- effect on dependent Checkpoints and anchors;
- unresolved historical uncertainty.

```text
Original Checkpoint
        │
        ├── corrected-by ──► Correction Checkpoint
        ├── superseded-by ─► Successor Checkpoint
        └── reliance-revoked-by ─► Revocation Evidence
```

The original Checkpoint remains part of history.

Externally published commitments cannot be made nonexistent through an internal status update.

---

# 10.32 Conflicting Checkpoints and Equivocation

Checkpoint conflict may exist when:

- the same identifier binds different protected content;
- one exclusive sequence position has incompatible Checkpoints;
- the same Chain position is associated with incompatible Chain Heads;
- overlapping ranges contain inconsistent state;
- different audiences receive incompatible Checkpoint series;
- an Authority publishes inconsistent target, policy, or Witness evidence.

Conflicts must remain visible.

A later Checkpoint must not silently select one branch and discard the other.

Witness publication, external anchoring, cross-observer comparison, and independent Preservation can strengthen detection of Checkpoint equivocation.

A Checkpoint series controlled and viewed only by one party cannot by itself prove absence of a split view.

---

# 10.33 Checkpoint Validation Layers

Checkpoint evaluation is layered.

## Availability and Parsing

Can the Checkpoint and mandatory dependencies be obtained and parsed safely?

## Structural and Semantic Validation

Do type, version, scope, target, predecessor, time, and extension values satisfy applicable rules?

## Canonicalization and Signature Validation

Can the protected input be reproduced and the Authority Signature validated under Historical Key State and Key Purpose?

## Target-State Validation

Does the Checkpoint bind the exact Chain, range, aggregate, Witness, or Registry state claimed?

## Coverage and Consistency Validation

Are member states, ranges, sampling rules, gaps, and aggregate commitments internally coherent?

## Authority Validation

Was the issuer authorized to create this Checkpoint under historical governance?

## Succession and Conflict Validation

Does the Checkpoint follow the required predecessor and remain consistent with known historical boundaries?

## Profile and Completeness Validation

Are cadence, Witness, publication, anchoring, availability, and other Trust Profile requirements satisfied?

Successful validation at one layer does not imply success at later layers.

---

# 10.34 Checkpoint Verification and Outcomes

A representative Checkpoint Verification procedure is:

1. identify the Accountability Claim and Verification Context;
2. resolve the Checkpoint type, scope, identifier, and version;
3. validate canonicalization and Signature;
4. resolve historical Authority identity, key, and governance state;
5. validate target Chain, range, aggregate, Witness, or Registry commitments;
6. validate predecessor and cadence relationships;
7. surface gaps, conflicts, corrections, and unavailable dependencies;
8. evaluate Checkpoint Time within its assurance limits;
9. validate publication and anchor evidence required by the Trust Profile;
10. report Checkpoint validity, coverage, Authority, independence, availability, and profile achievement separately.

Checkpoint outcomes may include:

- valid;
- invalid;
- indeterminate;
- incomplete;
- conflicting;
- unsupported;
- unavailable;
- valid with limitations.

A Checkpoint may be cryptographically valid while insufficient as a trusted Verification starting boundary.

---

# 10.35 Checkpoint Availability and Preservation

Checkpoint Preservation must cover the object and dependencies required for future interpretation.

Relevant material may include:

- canonical Checkpoint;
- Checkpoint Signature and algorithms;
- Authority identity and Historical Key State;
- scope and target-state definitions;
- Chain and range proof material;
- aggregate membership evidence;
- Witness Observations and quorum policy;
- historical Registry and Trust Profile state;
- predecessor, correction, and conflict evidence;
- publication and Anchor Evidence.

A Checkpoint digest without the scope, algorithm, target, or historical Authority context may be cryptographically recognizable but semantically unusable.

Replication controlled by one Authority improves resilience without automatically providing independent Preservation.

---

# 10.36 External Trust Boundary

An External Anchor contributes additional assurance only when the external record is meaningfully outside the unilateral control relevant to the protected history.

Trust-boundary analysis may consider:

- ownership and governance;
- administrative privileges;
- signing and deletion Authority;
- infrastructure and account control;
- software and data-source dependence;
- legal or contractual control;
- publication visibility;
- independent retention;
- censorship and rollback capability;
- failure correlation.

An anchor service operated in another process or cloud region under the same administrators may be external to one component but not independent of the protected Control Domain.

The assurance claim must reflect the actual boundary crossed.

---

# 10.37 External Anchor Classes

Possible External Anchor classes include:

## Transparency Service

An append-only or auditable publication service operated under separate governance.

## Timestamp Authority

A service that binds a commitment to governed time evidence.

## Public Ledger

A broadly replicated ledger with defined transaction and finality rules.

## Permissioned Ledger

A multi-operator ledger with governed membership and consensus.

## Regulated Archive or Registry

An accountable archival or publication system with retention and audit obligations.

## Counterparty Record

A record retained or signed by another party to the accountable action.

## Public Publication Channel

A widely observable publication mechanism whose content and time can be authenticated.

TrustAgentAI does not prescribe one class.

Each binding must define the assurance and limitations of the selected environment.

---

# 10.38 Anchor Selection Criteria

Anchor selection should consider:

- control-domain separation;
- authenticated publication model;
- immutability or rewrite resistance;
- availability and retrieval;
- time and ordering semantics;
- confirmation or finality model;
- reorganization and rollback risk;
- censorship and submission risk;
- longevity and Preservation;
- cost and throughput;
- privacy and metadata exposure;
- algorithm agility;
- legal and jurisdictional dependencies;
- exit and migration capability.

No external system is trusted merely because it is public, distributed, regulated, or uses blockchain.

The applicable Trust Profile must identify which properties are required.

---

# 10.39 Anchor Target Commitment

The external publication must bind a well-defined TrustAgentAI commitment.

The anchor target may be:

- a Checkpoint digest;
- a typed Checkpoint commitment;
- a Chain Head with CID and version context;
- an aggregate Checkpoint root;
- a migration or closure commitment;
- another TAIP-defined historical commitment.

A bare digest may be insufficient when type, domain, version, or namespace ambiguity permits substitution.

```text
Anchor Target =
    Domain
  + Type
  + Version
  + TrustAgentAI Commitment
  + Binding Context
```

TAIP or the anchor binding defines the exact canonical representation.

---

# 10.40 Anchor Evidence

**Anchor Evidence (AE)** is a Protocol Object or governed evidence package that supports Verification of the relationship between TrustAgentAI state and an External Anchor record.

Anchor Evidence may identify or bind:

- anchor target commitment;
- External Anchor type and namespace;
- external record or transaction identifier;
- submission method and account or issuer;
- publication payload or commitment;
- external acceptance state;
- confirmation or finality state;
- Publication Time and supporting evidence;
- inclusion, retrieval, or consistency proof;
- external system version and rules;
- applicable Trust Profile;
- limitations and failure state.

Anchor Evidence must state what it proves and at which external lifecycle state.

---

# 10.41 Anchor Evidence Logical Information Model

The logical Anchor Evidence model contains governed properties applicable to its binding and version.

| Logical property group | Purpose |
|---|---|
| Type and version | Determines Anchor Evidence semantics |
| Evidence identifier | Provides stable governed reference where defined |
| Anchor target | Binds the TrustAgentAI commitment |
| External system | Identifies anchor class, network, namespace, and rules |
| External record | Identifies the publication, transaction, or record |
| Method | Defines encoding, submission, and proof construction |
| Lifecycle state | Distinguishes submitted, accepted, published, confirmed, and final |
| Time evidence | Supports Publication Time and related boundaries |
| Proof material | Supports inclusion, retrieval, and consistency evaluation |
| Issuer or submitter | Attributes relevant external action where required |
| Policy and profile | Identifies applicable assurance requirements |
| Extensions | Adds governed optional or critical semantics |
| Cryptographic protection | Protects the TrustAgentAI interpretation of the bridge |

The model is logical rather than a final wire schema.

---

# 10.42 External Identifiers and Namespaces

An external record identifier must be interpreted within its exact external namespace.

Namespace context may include:

- system or service identifier;
- network, ledger, region, or environment;
- account, channel, log, or registry;
- transaction or record type;
- version or protocol era;
- test, staging, or production designation;
- fork or chain identifier;
- normalization and comparison rules.

```text
External Record Identifier
≠
Checkpoint Identifier
≠
Anchor Evidence Identifier
```

A locator alone is insufficient when it cannot be authenticated or related unambiguously to the target commitment.

---

# 10.43 Submission, Acceptance, Publication, and Confirmation

External anchoring lifecycle states must remain distinct.

## Submitted

The anchor target or encoded commitment was sent to the external system.

## Accepted

The external system accepted it for defined processing.

## Published or Recorded

The commitment became part of an externally observable record.

## Confirmed

The record satisfied a governed confirmation condition.

## Final

The record satisfied the binding's declared finality condition, if such a state exists.

```text
Submitted
≠
Accepted
≠
Published
≠
Confirmed
≠
Final
```

A client response or transaction identifier is not automatic evidence of publication or finality.

---

# 10.44 Publication Time

**Publication Time** is the time associated with an externally observable publication or record under defined external semantics.

Its assurance depends upon:

- the external system's time model;
- block, log, registry, or server time semantics;
- ordering and clock assumptions;
- confirmation or finality state;
- independent observation;
- authenticated retrieval;
- applicable anchor binding and Trust Profile.

Publication Time may support an existence-by boundary.

It must not be silently interpreted as Event Time, Commitment Time, Observation Time, or Checkpoint Time.

If the external system supplies only order and no reliable wall-clock time, the conclusion must be limited accordingly.

---

# 10.45 Confirmation and Finality

External systems use different confirmation and finality models.

Possible models include:

- immediate authoritative record;
- append-only log inclusion;
- multi-party consensus finality;
- probabilistic ledger confirmation;
- delayed regulatory or archival acceptance;
- publication with later consistency proof;
- no formal finality state.

The anchor binding must define:

- which state is required;
- how that state is proven;
- expected delay;
- reorganization or revocation risk;
- later status changes;
- Verification behavior before finality;
- effect on Trust Profile achievement.

An unconfirmed publication may still provide limited evidence without satisfying the intended profile.

---

# 10.46 Inclusion, Retrieval, and Consistency Proofs

Anchor Evidence may require proof that the target commitment is included in or corresponds to the external record.

Proof material may include:

- authenticated record contents;
- transaction or log inclusion proof;
- block or consensus proof;
- signed timestamp token;
- registry receipt;
- consistency proof between external log states;
- independently retained publication;
- authenticated archival record.

Proof verification must bind:

- exact target commitment;
- external namespace;
- external record state;
- method and algorithm version;
- publication or confirmation boundary.

Successful retrieval from an unauthenticated endpoint is not sufficient by itself.

---

# 10.47 Reorganization, Rollback, Revocation, and Deletion

An External Anchor record may later be reorganized, rolled back, revoked, corrected, hidden, expired, or deleted.

The binding must define how such events affect earlier conclusions.

Relevant evidence may include:

- former inclusion proof;
- later non-inclusion or reorganization evidence;
- external revocation or correction record;
- independent retention of the earlier state;
- affected Publication Time and finality status;
- re-anchoring evidence;
- downgrade or conflict outcome.

An earlier valid publication fact may remain historically relevant even if the external system later changes.

The current external view must not silently rewrite previously preserved anchor evidence.

---

# 10.48 Multiple External Anchors

A Trust Profile may require or permit anchoring into multiple external systems.

Multiple anchors can provide:

- control-domain diversity;
- availability diversity;
- different time or finality properties;
- resilience to one system's censorship or failure;
- cross-checking of target commitments.

Multiple anchors do not automatically provide independence if they share:

- ownership or administration;
- infrastructure or identity provider;
- one submission gateway;
- one underlying ledger or data source;
- correlated legal or economic control;
- the same unverified target construction.

Anchor quorum or diversity rules must be governed explicitly.

---

# 10.49 Anchor Independence and Control Domains

Anchor assurance must evaluate which Control Domain boundary was crossed.

Relevant questions include:

- Can the protected-history operator alter or delete the external record?
- Can one administrator control both Checkpoint issuance and anchoring?
- Does the anchor operator independently retain or publish the commitment?
- Are keys and accounts separately controlled?
- Are submission and retrieval paths independent?
- Do the systems share common failure or governance?
- Can the anchor censor or backdate records?
- Is the publication broadly observable?

An external vendor relationship may add a separate Organization without eliminating contractual or technical dependence.

Assurance claims must identify the independence dimensions actually satisfied.

---

# 10.50 Checkpoint, Chain Head, and Anchor Separation

The principal historical artifacts remain distinct.

| Artifact | Primary meaning |
|---|---|
| Chain Head | Cryptographic state of a defined Hash Chain boundary |
| Witness Observation | Attributable observation of defined state |
| Checkpoint | Authority commitment to a governed historical scope |
| Anchor Evidence | Bridge from a TrustAgentAI commitment to external state |
| External record | Native representation in the external system |

```text
Chain Head
≠
Checkpoint
≠
Anchor Evidence
≠
External Record
```

An implementation may store them together.

Their semantics and Verification requirements must remain separate.

---

# 10.51 Batch and Aggregate Anchoring

One external publication may anchor multiple Checkpoints through an authenticated aggregate.

The aggregate binding must define:

- member Checkpoint commitments;
- deterministic member ordering;
- duplicate and omission rules;
- aggregate construction;
- inclusion proof format;
- publication lifecycle state;
- partial-failure handling;
- member-level outcome semantics;
- maximum cardinality and resource limits.

A published aggregate root does not prove member inclusion without a valid proof.

If member order matters, an unordered set commitment is insufficient.

---

# 10.52 Failure and Graceful Degradation

Checkpointing or anchoring may fail because of:

- unavailable target state;
- incomplete Witness Quorum;
- Checkpoint Authority failure;
- conflicting Chain or Checkpoint state;
- external submission rejection;
- censorship or outage;
- fee or capacity limits;
- delayed confirmation;
- unsupported version;
- proof or retrieval failure.

Governance may permit operations to continue with reduced assurance.

Degradation rules must define retry, alternate anchors, escalation, maximum delay, later reconciliation, and effect on Achieved Trust Profile.

A later successful Checkpoint or anchor must not erase the historical fact that the required boundary was missed or delayed.

---

# 10.53 Privacy and Confidentiality

Checkpoints and anchors can expose identifiers, timing, activity volume, tenant relationships, risk level, and organizational topology.

Privacy design should consider:

- whether a direct Chain Head or a derived commitment should be published;
- whether the target has sufficient entropy;
- whether repeated commitments create correlation;
- whether cadence reveals transaction volume;
- whether multi-Chain membership exposes tenants or counterparties;
- whether Witness or Control Domain information should be public;
- whether external transaction accounts create linkability;
- whether public proof material reveals sensitive metadata;
- whether retention and deletion obligations can be honored.

Hashing sensitive low-entropy data does not reliably anonymize it.

The public commitment surface should be minimized before external publication.

---

# 10.54 Blinded and Privacy-Preserving Anchors

An anchor binding may use a blinded, salted, keyed, or otherwise privacy-preserving commitment.

The construction must define:

- committed TrustAgentAI state;
- blinding or opening material;
- domain and algorithm context;
- who can verify without disclosure;
- how later opening is authenticated;
- whether equality creates linkability;
- key and secret Preservation;
- effect of lost opening material;
- migration and algorithm transition.

Privacy protection must not destroy the ability to prove the intended relationship between the external record and Checkpoint.

Loss of opening material may leave publication verifiable while making target correspondence indeterminate.

---

# 10.55 Data Minimization for External Publication

External Anchor payloads should contain no more information than required for the anchor claim.

Direct publication should generally avoid unnecessary:

- Evidence Record content;
- personal or account identifiers;
- business amounts;
- Authority or Policy details;
- tenant names;
- workflow identifiers;
- internal Chain topology;
- Witness identities;
- proprietary schema data.

Typed domain-separated commitments can preserve interpretable binding without publishing plaintext.

Metadata emitted by the external transport, account, timing, or fee mechanism must also be considered.

---

# 10.56 Retention and Preservation

Long-term Verification requires Preservation of both TrustAgentAI and external interpretation material.

Relevant material may include:

- canonical Checkpoint and Anchor Evidence;
- target commitments and opening material;
- Checkpoint Authority identity and key history;
- Chain, range, aggregate, and Witness dependencies;
- external system identifiers and binding specifications;
- publication payloads and proofs;
- historical confirmation and finality rules;
- independently retained external state;
- reorganization, revocation, correction, and re-anchoring evidence;
- algorithm and canonicalization definitions.

An external system's expected permanence does not remove the need for Preservation planning.

Public availability today is not guaranteed availability throughout the Evidence Lifetime.

---

# 10.57 Portability and Export

A portable Checkpoint and anchor package should allow an independent verifier to reproduce intended historical conclusions outside the originating platform.

Depending upon scope, it may include:

- Checkpoints and identifiers;
- Chain Heads, ranges, and proofs;
- aggregate membership proofs;
- Witness Observations and quorum evidence;
- Checkpoint policies and historical Authority evidence;
- Anchor Evidence;
- external record contents or authenticated proofs;
- binding specifications and versions;
- confirmation, finality, and time evidence;
- conflicts, corrections, omissions, and unavailable dependencies.

An external URL alone is not a portable proof package.

---

# 10.58 Authority, Anchor, and Binding Migration

Checkpoint or anchor infrastructure may migrate because of key rotation, operator change, algorithm transition, service retirement, compromise, or governance change.

Migration evidence should bind:

- last supported prior Checkpoint;
- prior and successor Authority identities;
- prior and successor keys and Key Purposes;
- effective boundary;
- new Checkpoint namespace or continued identifier semantics;
- old and new anchor bindings;
- re-anchored transition commitment;
- unresolved conflicts or unavailable dependencies;
- historical Verification rules.

Changing an anchor destination must not imply that earlier external records disappeared or were invalid.

If continuity cannot be established, the discontinuity and assurance effect must be explicit.

---

# 10.59 Implementation Mapping and Illustrative Flow

Checkpoint and anchoring services may be implemented through:

- an Evidence Registry or ledger service;
- a separately governed Checkpoint service;
- a consortium authority;
- a customer or counterparty service;
- a transparency log;
- a timestamp authority;
- a public or permissioned ledger;
- a regulated archive;
- a hybrid system using periodic multi-anchor publication.

Implementation technology does not change the architectural requirements.

```text
Chain Operator      Witnesses      Checkpoint Authority      External Anchor
      │                 │                    │                       │
      │ Chain Head H    │                    │                       │
      │────────────────►│                    │                       │
      │                 │ Witness evidence   │                       │
      │                 │───────────────────►│                       │
      │ Chain state / proofs                 │                       │
      │─────────────────────────────────────►│                       │
      │                 │                    │ validate and issue CP │
      │                 │                    │                       │
      │                 │                    │ anchor commitment     │
      │                 │                    │──────────────────────►│
      │                 │                    │ Anchor Evidence       │
      │                 │                    │◄──────────────────────│
```

Each arrow represents a distinct claim and evidence boundary.

---

# 10.60 Common Anti-Patterns and Relationship to Other Specifications

## Checkpoint Equals Chain Head

Treating a raw Chain Head as a governed Checkpoint without Authority, scope, version, and time context.

## Checkpoint Equals Completeness

Claiming that a valid historical boundary proves every required event was recorded or remains available.

## Authority Signature Equals Independence

Treating a valid Checkpoint Authority Signature as proof of organizational or Control Domain separation.

## Current-State Verification

Using current keys, policies, registries, or external rules to interpret every historical Checkpoint and anchor.

## Missed Cadence Erasure

Treating a later successful Checkpoint as though an earlier required boundary was never missed.

## URL Equals Anchor Evidence

Providing an external locator without authenticated target binding, namespace, lifecycle state, or proof.

## Submission Equals Publication

Treating a transaction identifier or successful API response as external publication or finality.

## Blockchain Equals Truth

Treating a ledger commitment as validation of underlying Evidence Records, Authority, Policy, or business outcome.

## Same-Control Externality

Calling a service independent merely because it uses another product, process, region, or endpoint under common control.

## Confirmation Inflation

Reporting an unconfirmed or reversible external record as final.

## Silent Reorganization

Discarding earlier valid Anchor Evidence after the external system rolls back or changes state.

## Aggregate Without Membership Proof

Claiming member Checkpoints were anchored because an aggregate root was published.

## Sensitive Commitment Leakage

Publishing low-entropy or linkable commitments that reveal protected information through guessing or correlation.

## Key Transparency Substitution

Treating a Checkpoint or External Anchor as a substitute for historical identity-key lifecycle evidence.

This chapter defines Checkpoint, Checkpoint Authority, External Anchor, and Anchor Evidence semantics.

Other chapters and TAIP define related concerns, including:

- Key Transparency and Historical Key State;
- Trust Profiles and assurance levels;
- Preservation and cryptographic renewal;
- Verification Engines and Reports;
- Dispute Pack construction;
- security and privacy threat models;
- concrete schemas, cryptographic suites, anchor bindings, APIs, registries, and test vectors.

Those specifications may strengthen or specialize Checkpoint and anchor requirements.

They must preserve the invariants defined here.

---

# Checkpoint and Anchor Invariants

### INV-CP-001 — Bounded Checkpoint Scope

Every Checkpoint MUST define a governed scope that bounds the historical state and coverage to which it commits.

### INV-CP-002 — Exact Target Binding

Every Checkpoint MUST bind unambiguously to the exact Chain, range, aggregate, Witness, Registry, or other governed target state it concerns.

### INV-CP-003 — Authority Attribution

Every Checkpoint MUST identify the Checkpoint Authority responsible for its statement according to governed Protocol Identity semantics.

### INV-CP-004 — Identity/Key/Authority Separation

Checkpoint Authority identity, signing key, Key Purpose, governance Authority, operator, Organization, and Control Domain MUST remain distinguishable.

### INV-CP-005 — Deterministic Protected Input

Equivalent conforming Checkpoints under the same governed context MUST produce equivalent canonical cryptographic input.

### INV-CP-006 — Chain-Head/Checkpoint Separation

A Chain Head MUST remain distinguishable from the Checkpoint that commits to it.

### INV-CP-007 — Witness/Checkpoint Separation

Witness Observation validity and Witness Quorum achievement MUST remain separately evaluable from Checkpoint validity.

### INV-CP-008 — Checkpoint/Anchor Separation

Checkpoint issuance, External Anchoring, and Anchor Evidence validity MUST remain distinct states and claims.

### INV-CP-009 — Lifecycle Separation

Commitment, Witnessing, Checkpoint candidate creation, issuance, publication, anchoring submission, external publication, confirmation, finality, Preservation, and Verification MUST NOT be conflated.

### INV-CP-010 — Time Separation

Event Time, Commitment Time, Observation Time, Checkpoint Time, Publication Time, confirmation time, and Verification Time MUST remain distinguishable.

### INV-CP-011 — Cadence Visibility

Missed, delayed, failed, retried, and degraded Checkpoint or anchor boundaries MUST remain visible where required by Policy or Trust Profile.

### INV-CP-012 — Coherent Target State

A Checkpoint MUST commit to one internally coherent target state under the applicable sampling, range, or aggregation rules.

### INV-CP-013 — Governed Succession

Checkpoint genesis, predecessor, reset, recovery, closure, and migration boundaries MUST remain explicitly distinguishable.

### INV-CP-014 — Checkpoint Immutability

Protected content of an issued Checkpoint MUST NOT be silently modified.

### INV-CP-015 — Append-Only Correction

Correction, supersession, revocation, dispute, and limitation of an issued Checkpoint or Anchor Evidence object MUST create additional accountable evidence.

### INV-CP-016 — Conflict Visibility

Incompatible Checkpoints, target states, predecessor relationships, and external publication evidence MUST NOT be silently discarded or overwritten.

### INV-CP-017 — No Implied Completeness

Checkpoint validity or external publication MUST NOT be represented as proof that every required event was committed or remains available.

### INV-CP-018 — No Implied Truth

Checkpoint or anchor validity MUST NOT be represented as proof that underlying assertions are true, authorized, lawful, or Policy-compliant.

### INV-CP-019 — External Namespace Binding

Every Anchor Evidence claim MUST bind the external system, namespace, record, method, and version required to interpret the publication.

### INV-CP-020 — Anchor Target Binding

Anchor Evidence MUST preserve an unambiguous cryptographic relationship between the External Anchor record and the exact TrustAgentAI commitment.

### INV-CP-021 — External Lifecycle Visibility

Anchor submission, acceptance, publication, confirmation, finality, reorganization, revocation, and deletion MUST remain distinguishable.

### INV-CP-022 — Explicit Finality

External finality MUST be evaluated according to the applicable historical anchor binding and MUST NOT be inferred from a record identifier or current visibility alone.

### INV-CP-023 — Meaningful Trust Boundary

Claims of external or independent anchoring MUST identify the Control Domain boundary and independence properties actually satisfied.

### INV-CP-024 — Anchor Diversity

Multiple external records or providers MUST NOT automatically be interpreted as independent anchor assurance.

### INV-CP-025 — Proof-Scope Boundaries

Inclusion, consistency, retrieval, timestamp, and finality proofs MUST be interpreted only within their governed external system, target, and lifecycle scope.

### INV-CP-026 — Historical Interpretation

Checkpoint and anchor Verification MUST use the historical identities, keys, policies, schemas, algorithms, bindings, Registry state, and external rules applicable to the evaluated boundary.

### INV-CP-027 — Validity/Availability Separation

Checkpoint and Anchor Evidence validity MUST remain distinguishable from availability of underlying TrustAgentAI and external interpretation dependencies.

### INV-CP-028 — Intended/Achieved Separation

Configured Checkpoint cadence or anchor destination MUST NOT be represented as proof that the applicable Trust Profile was achieved.

### INV-CP-029 — Explicit Uncertainty

Unknown, missing, delayed, redacted, unavailable, malformed, unsupported, conflicting, reorganized, and unverifiable state MUST remain explicit where it affects conclusions.

### INV-CP-030 — Privacy Proportionality

Checkpoint and anchor designs SHOULD minimize unnecessary disclosure and linkability while preserving required historical proof.

### INV-CP-031 — Representation Independence

Normative Checkpoint and Anchor Evidence meaning MUST NOT depend upon one service, ledger, database, transport, vendor, or user interface.

### INV-CP-032 — Bounded Reproducible Verification

Independent conforming implementations SHOULD derive equivalent bounded Checkpoint and anchor conclusions from equivalent evidence under equivalent Verification Contexts.

---

# Architectural Requirements

### REQ-CP-001

TAIP MUST define or reference governed Checkpoint, Checkpoint Authority, External Anchor, and Anchor Evidence semantics.

### REQ-CP-002

Each Checkpoint type and version MUST define its scope, permitted target states, required protected properties, lifecycle, extension behavior, and validation rules.

### REQ-CP-003

Every interoperable Checkpoint MUST possess a stable identifier or governed content-derived identity with defined namespace, comparison, persistence, and collision behavior.

### REQ-CP-004

Every Checkpoint MUST identify the Checkpoint Authority responsible for issuance.

### REQ-CP-005

Checkpoint Signature evaluation MUST resolve the applicable Key Identifier, Key Purpose, Historical Key State, algorithm policy, protected scope, and Checkpoint-issuance Authority.

### REQ-CP-006

Every Checkpoint MUST bind the exact target type, namespace, identifier or position, cryptographic commitment, and version context required by its scope.

### REQ-CP-007

A target locator MUST NOT substitute for an integrity-bound target commitment where substitution could alter a Checkpoint claim.

### REQ-CP-008

Each Checkpoint version MUST define or reference deterministic canonicalization, domain separation, algorithm identification, and cryptographic input construction.

### REQ-CP-009

Substitution-sensitive identifier, Authority, scope, target, predecessor, time, policy, profile, dependency, and critical extension semantics MUST be cryptographically bound.

### REQ-CP-010

Checkpoint Time values MUST identify their exact meaning, source, precision, and supporting assurance and MUST NOT be treated as equivalent to Event, Commitment, Observation, or Publication Time.

### REQ-CP-011

Checkpoint policies MUST define applicable cadence triggers, permitted delay, grace, failure, retry, recovery, publication, anchoring, and downgrade behavior.

### REQ-CP-012

Missed or delayed required Checkpoints MUST remain visible and MUST affect Trust Profile evaluation according to the applicable historical policy.

### REQ-CP-013

Checkpoint creation MUST bind one coherent target state under deterministic snapshot, range, collection-window, or aggregate semantics.

### REQ-CP-014

Multi-Chain and aggregate Checkpoints MUST define membership, ordering, omission, duplication, sampling, proof, missing-member, and resource-bound behavior.

### REQ-CP-015

Range Checkpoints MUST define starting and ending boundaries, inclusion semantics, gap treatment, continuity evidence, and ordering scope.

### REQ-CP-016

Checkpoints containing Witness Quorum claims MUST bind the applicable target, Observation Scope, quorum policy, contributing observations, eligibility, Control Domain, timing, threshold, and conflict evidence.

### REQ-CP-017

A Checkpoint MUST NOT represent a Witness Quorum as achieved when required Witness evidence or historical policy evaluation is invalid, unavailable, or incomplete.

### REQ-CP-018

Registry, Policy, Trust Profile, or governance-state commitments MUST identify the exact state, namespace, version, and interpretation dependencies required by their claim.

### REQ-CP-019

Mandatory Checkpoint dependencies MUST be integrity-governed and either resolvable or explicitly reported as unavailable.

### REQ-CP-020

Checkpoint candidate, finalized, issued, published, committed, and anchored states MUST remain distinguishable.

### REQ-CP-021

Protected content of an issued Checkpoint MUST NOT be modified in place.

### REQ-CP-022

Correction, supersession, revocation, dispute, or changed reliance status MUST identify the affected Checkpoint and create additional accountable evidence.

### REQ-CP-023

Checkpoint predecessor and succession rules MUST distinguish governed genesis, normal succession, closure, reset, recovery, migration, and missing state.

### REQ-CP-024

Incompatible Checkpoints, duplicate exclusive positions, conflicting targets, and split-view evidence MUST remain available to Verification.

### REQ-CP-025

Checkpoint validation SHOULD distinguish availability, structural, semantic, canonicalization, Signature, target-state, coverage, Authority, succession, conflict, Completeness, and Trust Profile results.

### REQ-CP-026

Checkpoint Verification Reports MUST bound conclusions to the evaluated scope, target, Authority, time, evidence set, policy version, Trust Profile, and unresolved limitations.

### REQ-CP-027

TAIP or each external binding MUST define the canonical TrustAgentAI anchor target, domain separation, encoding, external representation, and comparison behavior.

### REQ-CP-028

Anchor Evidence MUST identify or bind the exact TrustAgentAI commitment, external system and namespace, external record, method, lifecycle state, applicable rules, and proof material required by its claim.

### REQ-CP-029

External record identifiers MUST be interpreted within an explicit network, ledger, registry, log, account, environment, fork, or other namespace sufficient to prevent ambiguous reuse.

### REQ-CP-030

External submission, acceptance, publication, confirmation, finality, reorganization, revocation, correction, and deletion states MUST remain distinguishable.

### REQ-CP-031

A client acknowledgment, transaction identifier, or locator MUST NOT be represented as external publication, confirmation, or finality without supporting governed evidence.

### REQ-CP-032

Publication Time claims MUST identify the external time and ordering model, precision, confirmation state, and supporting evidence and MUST NOT imply stronger time assurance than the external system provides.

### REQ-CP-033

Each anchor binding MUST define required confirmation or finality state, proof method, expected delay, rollback risk, and Verification behavior before and after that state.

### REQ-CP-034

Inclusion, retrieval, consistency, timestamp, and finality proofs MUST bind the exact target commitment, external namespace, external record state, method, algorithm, and version.

### REQ-CP-035

Reorganization, rollback, revocation, correction, deletion, or loss of an external record MUST affect Verification explicitly and MUST NOT silently erase preserved earlier Anchor Evidence.

### REQ-CP-036

Trust Profiles requiring external or independent anchoring MUST define the Control Domain and independence criteria the anchor must satisfy.

### REQ-CP-037

Common ownership, administration, infrastructure, identity, submission gateway, underlying ledger, or governance affecting anchor independence MUST remain visible.

### REQ-CP-038

Multiple-anchor policies MUST define eligible anchor classes, diversity criteria, target equality, timing, lifecycle state, threshold, conflict, and failure behavior.

### REQ-CP-039

Multiple external records MUST NOT be counted as independent anchors solely because they have different record identifiers, endpoints, accounts, or provider names.

### REQ-CP-040

Batch or aggregate anchoring MUST define member identity, deterministic ordering, omission, duplicate, construction, inclusion proof, partial failure, and member-level outcome semantics.

### REQ-CP-041

A published aggregate commitment MUST NOT be represented as proof of member inclusion without a valid governed inclusion proof.

### REQ-CP-042

Graceful degradation rules MUST preserve evidence of failed, delayed, censored, rejected, unconfirmed, or unavailable Checkpoint and anchor controls and MUST reflect them in Achieved Trust Profile evaluation.

### REQ-CP-043

Unknown critical Checkpoint or Anchor Evidence extensions MUST produce an unsupported or indeterminate outcome for affected claims.

### REQ-CP-044

Checkpoint and anchor designs SHOULD minimize unnecessary disclosure of underlying content, identifiers, topology, cadence, Witness composition, accounts, and external metadata.

### REQ-CP-045

Blinded, salted, keyed, encrypted, or selectively disclosed anchor commitments MUST define verification, opening, linkability, key or secret loss, Preservation, and migration behavior.

### REQ-CP-046

Implementations MUST bound Checkpoint size, aggregate cardinality, dependency depth, external proof size, parsing work, cryptographic operations, and retrieval effort.

### REQ-CP-047

Preservation planning SHOULD include Checkpoints, Anchor Evidence, target proofs, historical identities, keys, policies, scopes, external binding rules, publication records, finality evidence, and correction or reorganization history required for the Evidence Lifetime.

### REQ-CP-048

Portable Checkpoint and anchor packages SHOULD identify included, omitted, redacted, unavailable, delayed, unconfirmed, conflicting, reorganized, and externally resolved material required by the intended conclusion.

### REQ-CP-049

Checkpoint Authority, namespace, algorithm, and anchor-binding migration MUST preserve the last supported prior boundary, successor relationship, effective state, historical rules, and unresolved limitations.

### REQ-CP-050

Historical Verification MUST use applicable historical identities, keys, policies, registries, scopes, algorithms, external namespaces, bindings, confirmation rules, and Trust Profiles rather than current defaults.

### REQ-CP-051

Independent conforming implementations SHOULD derive equivalent Checkpoint, publication, confirmation, finality, and anchor conclusions from equivalent evidence under equivalent Verification Contexts.

---

# Security Considerations

Checkpoints and External Anchors create powerful historical boundaries but introduce Authority, sampling, publication, finality, and cross-system attack surfaces.

Major threats include:

- forging a Checkpoint Authority or Checkpoint;
- using a valid key without Checkpoint-issuance Authority;
- rewriting historical Authority, key, policy, or scope state;
- binding a Checkpoint to the wrong Chain Head, range, or aggregate;
- combining inconsistent member states into one apparent snapshot;
- suppressing gaps, failed cadence, or conflicting Checkpoints;
- presenting split Checkpoint views to different observers;
- substituting an invalid Witness set or quorum result;
- replaying a Checkpoint in another namespace, epoch, or profile;
- manipulating Checkpoint Time or Publication Time;
- substituting another external network, environment, fork, or record;
- treating submission or acceptance as publication or finality;
- presenting an unconfirmed or reorganized record as final;
- controlling both protected history and the alleged external anchor;
- using several correlated anchors as independent assurance;
- withholding aggregate membership proofs;
- exploiting unauthenticated external locators;
- denying or censoring anchor submission;
- causing unbounded external proof, retrieval, or dependency work;
- exposing sensitive low-entropy commitments;
- losing historical external rules, proof material, or opening secrets.

Implementations should apply controls including:

- governed Authority identity, Key Purpose, and Historical Key State;
- deterministic scope, target, and canonicalization rules;
- coherent snapshot and aggregation methods;
- conflict-preserving Checkpoint succession;
- explicit cadence and degradation evidence;
- exact external namespace and target binding;
- lifecycle-aware publication and finality validation;
- independently retained Anchor Evidence;
- reorganization and rollback monitoring;
- Control Domain analysis and anchor diversity rules;
- member-level aggregate inclusion proofs;
- bounded parsing, proof, and retrieval work;
- privacy-preserving commitments and data minimization;
- governed migration and algorithm agility;
- durable Preservation of interpretation dependencies;
- layered, reproducible Verification.

An attacker controlling both the Checkpoint Authority and alleged External Anchor may produce mutually consistent evidence without adding meaningful independent assurance.

The additional layer matters only to the degree that it crosses a real boundary and remains independently observable or preservable.

---

# Privacy Considerations

Checkpointing and external publication can expose durable patterns even when underlying content remains private.

Privacy analysis should consider:

- whether one public commitment can safely represent many internal Chains;
- whether cadence reveals activity volume or risk posture;
- whether member proofs reveal tenant or workflow relationships;
- whether Witness and Authority identities need public disclosure;
- whether external accounts correlate multiple Organizations;
- whether transaction fees, timing, and network metadata reveal sensitive operations;
- whether low-entropy target values can be guessed;
- whether selective disclosure can prove membership without exposing unrelated members;
- whether retained public records conflict with deletion obligations;
- whether migration creates new linkability.

Data minimization must occur before external publication because public or third-party records may be difficult or impossible to remove.

Privacy limitations must not be hidden by reporting successful cryptographic validation alone.

---

# Design Rationale

TrustAgentAI uses Checkpoints because replaying an entire history from genesis is not always necessary or practical for every future claim.

A governed Checkpoint creates a compact boundary that identifies which historical state an Authority committed to under defined rules.

It can bind Chain Heads, ranges, Witness Quorum evidence, Registry state, and the dependencies needed for historical interpretation.

Checkpoint scope prevents the boundary from being interpreted as broader than its actual coverage.

Cadence makes missing or delayed historical boundaries measurable rather than invisible.

Succession and append-only correction preserve how Checkpoint state changed over time.

External Anchors address a different limitation: an internally valid Checkpoint can still be rewritten or selectively presented if every available copy remains under the same control.

Placing the commitment into another meaningful trust domain increases the parties or systems that must be compromised to alter history undetectably.

Anchor Evidence is required because an external transaction identifier or URL does not explain which TrustAgentAI state was published, under which encoding, with which confirmation, or at which supported time.

Explicit finality and reorganization rules prevent temporary external visibility from becoming exaggerated historical certainty.

Privacy-preserving commitments allow cross-domain publication without exporting unnecessary sensitive evidence.

The stable objective is:

> **A verifier should be able to identify the exact historical boundary, the Authority that committed to it, the external trust boundary it crossed, the external state it reached, and every limitation affecting reliance.**

---

# Summary

Checkpoints and External Anchors strengthen TrustAgentAI historical assurance through two distinct layers.

A conforming model establishes a disciplined boundary:

1. every Checkpoint has governed type, scope, target state, identifier, and Authority;
2. Chain Heads, Witness Observations, Checkpoints, Anchor Evidence, and external records remain distinct;
3. target Chain, range, aggregate, quorum, or Registry state is cryptographically bound;
4. Checkpoint creation uses a coherent snapshot or collection model;
5. Checkpoint Time remains distinct from Publication Time;
6. cadence, delay, failure, retry, and degradation remain visible;
7. issued Checkpoints are immutable and corrected through additional evidence;
8. succession, reset, recovery, closure, and migration boundaries remain explicit;
9. incompatible Checkpoints and split views are preserved for Verification;
10. Checkpoint validity does not imply completeness, independence, or business truth;
11. Anchor Evidence binds an exact TrustAgentAI commitment to an exact external namespace and record;
12. external submission, acceptance, publication, confirmation, finality, and reorganization remain distinct;
13. Publication Time is interpreted through the external system's actual time model;
14. external proof scope and finality are governed by versioned bindings;
15. meaningful anchoring depends upon crossing a real Control Domain boundary;
16. multiple anchors count as diverse only when applicable independence rules are satisfied;
17. aggregate anchoring preserves member identity and inclusion proofs;
18. rollback, deletion, correction, and reorganization do not silently erase prior evidence;
19. historical identities, keys, policies, algorithms, and external rules remain preservable;
20. public commitments minimize sensitive disclosure and correlation;
21. portable evidence supports independent reproduction of bounded conclusions;
22. Verification separates cryptographic validity, coverage, Authority, independence, availability, Completeness, and Trust Profile achievement.

The foundational Checkpoint and anchor rule is:

> **Commit to the exact historical boundary, publish only the minimum necessary commitment across a real trust boundary, and preserve proof of every state transition and limitation.**

The broader architectural principle remains:

> **Proof, not logs.**
