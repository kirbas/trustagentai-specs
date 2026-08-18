# Chapter 10 — Checkpoint and External Anchor Specification

> **A Checkpoint fixes a governed historical boundary; an External Anchor places a commitment to that boundary beyond the control domain whose history it protects.**

## Purpose

This chapter defines the architectural specification for TrustAgentAI **Checkpoints (CP)**, **Checkpoint Authorities (CA)**, **External Anchors (EA)**, and **Anchor Evidence (AE)**.

Checkpoints create compact, durable commitments to defined protocol history.

External Anchors can strengthen those commitments by publishing them into a separately governed system or trust boundary.

This chapter establishes:

- Checkpoint semantics and scope;
- Checkpoint Authority identity, keys, Authority, and Control Domain;
- covered Chain state, entry ranges, aggregates, and Witness quorum evidence;
- Checkpoint Time, cadence, triggers, and lifecycle;
- Checkpoint creation, publication, conflict, and Verification;
- External Anchor target, method, namespace, finality, and time semantics;
- Anchor Evidence structure and Verification;
- multi-anchor, failure, degradation, privacy, Preservation, and migration behavior;
- Checkpoint and anchoring invariants;
- architectural requirements for interoperable implementations.

This chapter applies the common Protocol Object rules defined in [06-Protocol-Objects.md](06-Protocol-Objects.md).

It builds upon the Hash Chain model in [08-Hash-Chain-Specification.md](08-Hash-Chain-Specification.md) and the Witness model in [09-Witness-Observation-Specification.md](09-Witness-Observation-Specification.md).

It also applies the Evidence Record model in [07-Evidence-Record-Specification.md](07-Evidence-Record-Specification.md), the system model in [05-System-Overview.md](05-System-Overview.md), and the principles in [04-Design-Principles.md](04-Design-Principles.md).

It uses the canonical terms defined in [Terminology.md](Terminology.md) and [Acronyms.md](Acronyms.md).

This chapter defines architectural semantics.

It does not mandate a blockchain, public ledger, timestamp authority, transparency service, consensus network, archival provider, final wire schema, specific hash function, or specific Signature suite.

Concrete formats, algorithms, registries, APIs, confirmation policies, test vectors, and deployment bindings belong to TAIP and governed profiles.

---

# 10.1 Checkpoint Definition

A **Checkpoint (CP)** is a cryptographically protected Protocol Object that commits to defined historical protocol state at a governed boundary.

A Checkpoint may bind:

- one or more Chain Identifiers;
- one or more Chain Heads;
- Chain Entry positions or ranges;
- batch or aggregate commitments;
- Witness Observations or Witness Quorum evidence;
- relevant time boundaries;
- protocol, Chain, and Checkpoint versions;
- dependencies required for Verification;
- Checkpoint Authority identity and Signature.

```text
Historical Protocol State
          │
          ▼
 Checkpoint Candidate
          │ validate and authorize
          ▼
       Checkpoint
          │
          ├── retained by verifiers
          ├── observed by Witnesses
          └── submitted to External Anchors
```

A Checkpoint is a stable historical boundary, not a replacement for the underlying history.

---

# 10.2 External Anchor Definition

An **External Anchor (EA)** is a system or publication environment in another relevant administrative, operational, or trust boundary that records a commitment to TrustAgentAI state.

Representative External Anchors include:

- an independent transparency service;
- a timestamp authority;
- a public or permissioned ledger;
- a regulated archival or publication service;
- a counterparty-controlled record;
- an independently governed registry;
- another durable publication channel defined by a Trust Profile.

The External Anchor may use a native representation unrelated to TAIP.

Anchor Evidence provides the governed bridge between that representation and the TrustAgentAI commitment.

---

# 10.3 Scope

This chapter applies to Checkpoints and Anchor Evidence concerning:

- Hash Chain Heads;
- Chain Entry ranges;
- multiple Chains or shards;
- Witness Quorum evidence;
- Registry or Policy state;
- key-transparency state;
- migration or closure boundaries;
- batch or aggregate commitments;
- another accountability-relevant historical state defined by TAIP.

A deployment may use:

- Checkpoints without External Anchoring;
- Checkpoints with one or more External Anchors;
- direct anchoring of another governed TrustAgentAI commitment where permitted;
- different cadence and anchor policies for different Trust Profiles.

The intended assurance must state which mechanisms are required and which were actually achieved.

---

# 10.4 Security Objective

The Checkpoint objective is to create a compact historical boundary against which later Chain or evidence presentations can be compared.

The External Anchor objective is to make unilateral rewrite more difficult or detectable by placing the protected commitment into another relevant control or trust domain.

The combined architectural statement is:

> A verifier can determine which historical state a Checkpoint committed to, who issued it, under which historical rules, whether required Witness evidence supported it, and whether that exact commitment was published into a verifiable external system by a supported boundary.

This objective requires governed semantics for:

- scope and covered state;
- Authority and identity;
- canonical commitment construction;
- time and cadence;
- publication and confirmation;
- external namespace and proof;
- conflicts and failure;
- historical dependencies and Preservation.

---

# 10.5 What a Checkpoint Establishes

Subject to successful Verification, a Checkpoint may establish that:

- an identifiable Checkpoint Authority issued a protected commitment;
- defined Chain state, ranges, aggregates, or other protocol state were covered;
- the covered state was interpreted under defined versions and algorithms;
- declared Witness Quorum evidence was bound into the boundary;
- the Checkpoint existed no later than a supported later observation or publication boundary;
- later presented history is or is not consistent with the checkpointed boundary;
- the Checkpoint satisfies defined cadence or Trust Profile rules.

The exact conclusion depends upon the Checkpoint Scope and available dependencies.

---

# 10.6 What a Checkpoint Does Not Establish

A valid Checkpoint does not by itself prove:

- truth of underlying Evidence Record assertions;
- valid Authority or Policy for the underlying actions;
- that every required event was committed;
- availability of every covered object;
- Witness eligibility or independence unless supporting evidence is evaluated;
- absence of another conflicting Checkpoint;
- independence of the Checkpoint Authority;
- external publication;
- correct business execution;
- Legal Validity or Regulatory Compliance.

```text
Valid Checkpoint
≠
Complete Evidence Set
≠
Independent Checkpoint Authority
≠
Externally Anchored State
≠
Business Truth
```

---

# 10.7 What External Anchoring Establishes

Subject to successful Verification, External Anchoring may establish that:

- a defined TrustAgentAI commitment was represented in a defined external namespace;
- the external representation was published or confirmed under governed semantics;
- the commitment existed no later than a supported external boundary;
- changing the TrustAgentAI history would require inconsistency with the independently retained external record;
- multiple trust domains would need to be compromised or reconciled to conceal a rewrite under the applicable threat model.

The strength of these conclusions depends upon:

- external-system integrity;
- control-domain separation;
- finality or confirmation rules;
- time evidence;
- retrieval and proof availability;
- correct binding between the TrustAgentAI commitment and external representation.

---

# 10.8 What External Anchoring Does Not Establish

External Anchoring does not by itself prove:

- meaning or validity of the anchored content;
- correctness of the Checkpoint Authority;
- validity of the Hash Chain;
- eligibility of Witnesses;
- evidence Completeness;
- permanent availability of underlying TrustAgentAI objects;
- universal or irreversible finality;
- trusted wall-clock time beyond the anchor's supported semantics;
- legal recognition of the external record.

```text
External Publication
≠
Underlying Semantic Verification
≠
Permanent Preservation
```

A transaction identifier or public URL alone is not sufficient Anchor Evidence.

---

# 10.9 Layered Assurance Composition

Hash Chains, Witness Observations, Checkpoints, and External Anchors contribute different properties.

| Layer | Primary contribution | Does not establish by itself |
|---|---|---|
| Hash Chain | Ordered historical continuity | Absence of split views or completeness |
| Witness Observation | Separately attributable observation | Truth of underlying business event |
| Checkpoint | Stable commitment to a defined historical boundary | Availability or external publication |
| External Anchor | Commitment across another trust boundary | Meaning of anchored content |

```text
Evidence Record
      │
      ▼
Hash Chain State
      │
      ├── Witness Observations / Quorum
      │
      ▼
Checkpoint
      │
      ▼
External Anchor + Anchor Evidence
```

No layer inherits another layer's assurance automatically.

---

# 10.10 Checkpoint Authority

A **Checkpoint Authority (CA)** is the logical role responsible for constructing, authorizing, signing, and issuing a Checkpoint.

Its responsibilities may include:

- selecting the applicable Checkpoint policy;
- obtaining candidate historical state;
- validating Chain, range, and aggregate commitments;
- validating required Witness Quorum evidence;
- enforcing cadence and trigger rules;
- constructing deterministic protected input;
- signing and issuing the Checkpoint;
- publishing or distributing the Checkpoint;
- initiating required External Anchoring;
- preserving Checkpoint and Anchor dependencies;
- reporting failure and conflicts.

The Checkpoint Authority may be internal or external to the Chain operator.

Its actual Control Domain affects the assurance it contributes.

---

# 10.11 Checkpoint Authority Identity, Key, and Authority

Every interoperable Checkpoint must identify the Checkpoint Authority according to governed Protocol Identity semantics.

Verification may require:

- stable Protocol Identity;
- responsible operator or Organization;
- Key Identifier and Key Purpose;
- Historical Key State;
- Authority to issue Checkpoints for the covered namespace;
- applicable Registry or governance state;
- algorithm policy;
- suspension, revocation, or compromise evidence.

```text
Valid Checkpoint Signature
≠
Valid Checkpoint Authority
≠
Independent Checkpoint Authority
```

Possession of a signing key does not automatically establish Authority over the covered Chains.

---

# 10.12 Checkpoint Authority Control Domain

Checkpoint assurance depends partly upon the relationship between the Checkpoint Authority and:

- Evidence Producers;
- Registries and Commitment Services;
- Hash Chain operators;
- Witness operators;
- External Anchor operators;
- Preservation Services;
- governance authorities.

A Checkpoint signed by the same component that operates the Chain may provide useful compact state and operational integrity.

It does not by itself add an independent Control Domain.

Separate process, hostname, key, region, or service name is not sufficient proof of independence when common administration or ownership remains.

Trust Profiles must evaluate the actual required separation.

---

# 10.13 Checkpoint Protocol Object

A Checkpoint is a Protocol Object governed by the common rules in Chapter 6.

Its logical semantics include:

- object type and versions;
- Checkpoint identifier;
- Checkpoint Authority identity;
- Checkpoint Scope;
- covered historical state;
- covered Chains, positions, ranges, or aggregates;
- Witness Quorum evidence where applicable;
- Checkpoint Time semantics;
- policy and Trust Profile context;
- referenced Verification dependencies;
- extensions;
- cryptographic protection.

The logical model is independent of storage layout and transport encoding.

---

# 10.14 Checkpoint Identifier

Every finalized interoperable Checkpoint should possess a stable governed identifier.

Identifier rules must define:

- namespace and assignment responsibility;
- syntax;
- uniqueness and comparison behavior;
- relationship to Checkpoint Authority;
- relationship to the Checkpoint digest;
- collision handling;
- persistence across publication and export;
- version and migration behavior.

```text
Checkpoint Identifier
≠
Checkpoint Digest
≠
External Transaction Identifier
≠
Retrieval Locator
```

Different protected Checkpoints associated with the same identifier must be surfaced as a conflict.

---

# 10.15 Checkpoint Scope

**Checkpoint Scope** defines exactly which historical state the Checkpoint commits to and which conclusions it is intended to support.

Scope may specify:

- covered CIDs;
- Chain Heads;
- positions or ranges;
- epoch or shard boundaries;
- aggregate construction;
- Witness Quorum policy and evidence;
- Registry, Policy, or key state commitments;
- time boundary;
- required dependencies;
- exclusions and limitations;
- intended Trust Profile.

Checkpoint Scope is security-critical.

It must be versioned, deterministic, and cryptographically bound.

Vague descriptions such as current ledger state are insufficient.

---

# 10.16 Covered Chain State

A Checkpoint covering a Hash Chain must bind at least the information needed to identify the exact historical boundary.

Depending upon the Chain construction, this may include:

- CID;
- Chain version;
- Chain Head;
- entry position or range endpoint;
- epoch or segment identifier;
- aggregate or shard context;
- algorithm and canonicalization versions;
- closure or migration status;
- known gap or conflict indicators.

A Chain Head without CID and version context is not an unambiguous checkpoint target.

The Checkpoint does not replace the Chain material required to validate how that head was produced.

---

# 10.17 Single-Chain Checkpoint

A single-Chain Checkpoint commits to a defined boundary of one CID.

It may support:

- efficient validation from an earlier trusted boundary;
- rollback detection;
- cadence evaluation;
- Witness comparison;
- external publication;
- Chain closure or migration.

```text
CID C
  Entry 1 ─► Entry 2 ─► ... ─► Entry n
                                  │
                                  ▼
                            Checkpoint CP-n
```

The Checkpoint's position and Chain Head must agree under the applicable Chain rules.

---

# 10.18 Multi-Chain and Aggregate Checkpoint

A Checkpoint may commit to several Chains, shards, tenants, or state families through a governed aggregate.

Aggregate semantics must define:

- member namespace and identity;
- member ordering, if meaningful;
- duplicate handling;
- omitted-member behavior;
- aggregate construction;
- inclusion proof format;
- partial failure behavior;
- maximum cardinality;
- privacy characteristics.

```text
Chain A Head ─┐
Chain B Head ─┼──► Aggregate Commitment ─► Checkpoint
Chain C Head ─┘
```

An aggregate Checkpoint does not create a total order across member Chains unless the construction explicitly defines one.

---

# 10.19 Entry Range Coverage

A Checkpoint may cover a Chain range rather than only a terminal Chain Head.

Range semantics must identify:

- inclusive or exclusive endpoints;
- starting and ending positions;
- starting and ending Chain states;
- expected continuity rules;
- gaps, sparse positions, or omitted material;
- relevant epoch or segment;
- proof required for covered entries;
- relationship to prior Checkpoints.

A terminal Chain Head may cryptographically depend upon earlier entries while the Checkpoint Scope still limits which range was evaluated or promised available.

Cryptographic dependency and declared coverage must not be conflated.

---

# 10.20 Witness Quorum Evidence

A Checkpoint may bind Witness Observations or a derived Witness Quorum result.

The Checkpoint must identify or commit to:

- target historical state observed;
- applicable quorum policy and version;
- contributing Witness Observations;
- Witness identities and Control Domains;
- timing window;
- threshold calculation or governed proof;
- excluded, late, duplicate, or conflicting observations;
- unresolved eligibility or independence limitations.

The Checkpoint Authority must not convert an invalid or incomplete Witness set into a valid quorum merely by including it in a signed Checkpoint.

Witness evaluation remains governed by Chapter 9 and the applicable Trust Profile.

---

# 10.21 Referenced Dependencies

A Checkpoint may reference rather than embed material required for later Verification.

Dependencies may include:

- Chain Entries and objects;
- Witness Observations;
- schemas and canonicalization rules;
- algorithm registries;
- Checkpoint Authority identity and key history;
- Witness Registry and quorum policy state;
- Trust Profile versions;
- migration records;
- prior Checkpoints;
- External Anchor Evidence.

Mandatory dependencies must be integrity-bound and either resolvable or explicitly reported as unavailable.

A locator alone does not protect dependency identity.

---

# 10.22 Time Semantics

Checkpoint and anchoring workflows may contain several distinct time values:

- Event Time;
- Commitment Time;
- Observation Time;
- Checkpoint candidate time;
- Checkpoint Time;
- Checkpoint Signature Time;
- anchor submission time;
- Publication Time;
- external confirmation time;
- Verification Time.

```text
Checkpoint Time
≠
Publication Time
≠
External Confirmation Time
```

Each time value must identify its source, meaning, precision, and supporting assurance.

No time value should imply stronger evidence than the mechanism actually provides.

---

# 10.23 Checkpoint Time

**Checkpoint Time** is the time associated with creation or issuance of the Checkpoint under defined semantics.

It may be:

- asserted by the Checkpoint Authority;
- derived from an authenticated signing process;
- supported by Witness Observations;
- bounded by Chain state;
- strengthened by later External Anchor publication;
- supported by another trusted time source.

A Checkpoint Authority clock is not automatically independently trusted.

A later External Anchor may support that the Checkpoint existed no later than the external publication boundary without proving the Checkpoint Authority's earlier claimed clock time.

---

# 10.24 Checkpoint Cadence

**Checkpoint cadence** defines how often or under which conditions Checkpoints are expected.

Cadence may be based upon:

- elapsed time;
- number of Chain Entries;
- batch or transaction volume;
- risk tier;
- Trust Profile;
- Chain closure or epoch transition;
- key or algorithm rotation;
- governance event;
- detected conflict;
- manual or regulatory trigger.

Cadence rules must define grace intervals, missed-boundary behavior, and the effect on Achieved Trust Profile.

Configuration of a cadence is not evidence that the Checkpoints were actually created.

---

# 10.25 Checkpoint Triggers

A Checkpoint may be scheduled or event-triggered.

Representative triggers include:

- periodic timer;
- position threshold;
- high-value Accountable Action;
- Witness Quorum completion;
- Chain segment closure;
- migration;
- key compromise response;
- Policy or Trust Profile change;
- legal hold;
- dispute or incident;
- external publication window.

The trigger event and resulting Checkpoint remain distinct Protocol facts.

Failure to create a required Checkpoint must remain visible.

---

# 10.26 Checkpoint Candidate

A **Checkpoint candidate** is the proposed historical state assembled for Checkpoint validation and issuance.

It may include:

- target Chain Heads or aggregate root;
- proposed scope and boundary;
- Witness Quorum evidence;
- candidate time context;
- policy and profile references;
- validation results;
- unresolved gaps or conflicts;
- intended anchor policy.

A candidate is not a finalized Checkpoint.

```text
Checkpoint Candidate
≠
Signed Checkpoint
≠
Externally Anchored Checkpoint
```

A Witness Observation of a candidate does not prove that the same content was later issued as the Checkpoint unless the binding is verified.

---

# 10.27 Checkpoint Creation Procedure

A representative Checkpoint procedure is:

1. select the applicable Checkpoint policy and scope;
2. obtain candidate Chain and aggregate state;
3. resolve required prior Checkpoints;
4. validate covered Chain continuity and positions;
5. validate required Witness Observations and quorum evidence;
6. detect gaps, forks, conflicts, or unavailable dependencies;
7. enforce cadence and trigger requirements;
8. construct deterministic Checkpoint input;
9. authorize and sign the Checkpoint;
10. issue and publish the Checkpoint;
11. initiate required External Anchoring;
12. preserve evidence and report failures.

The applied validation scope must be identifiable.

Issuance must not conceal unresolved mandatory failures.

---

# 10.28 Canonicalization and Checkpoint Signature

Each Checkpoint version must define or reference deterministic canonicalization and Signature input.

Protected input must bind substitution-sensitive semantics, including:

- object type and versions;
- Checkpoint identifier;
- Checkpoint Authority identity;
- Checkpoint Scope;
- covered Chain and aggregate state;
- Witness Quorum commitment;
- time semantics;
- policy and Trust Profile context;
- critical extensions;
- algorithm identifiers and domain separation.

Unprotected transport, storage, or presentation metadata must not be treated as signed Checkpoint meaning.

Independent implementations must derive equivalent cryptographic input from equivalent conforming Checkpoints.

---

# 10.29 Finalization and Lifecycle

Checkpoint lifecycle states may include:

- candidate assembled;
- validation complete;
- authorized;
- finalized;
- signed;
- issued;
- published;
- anchor submitted;
- externally confirmed;
- superseded;
- preserved;
- verified.

These states must remain distinct.

After finalization and Signature, protected Checkpoint content must not be modified in place.

External anchoring is a later state supported by separate Anchor Evidence.

---

# 10.30 Checkpoint Publication and Distribution

Checkpoints may be:

- returned to Chain operators or Registry services;
- published by the Checkpoint Authority;
- distributed to Witnesses;
- committed into another TrustAgentAI Chain;
- submitted to External Anchors;
- retained by counterparties or verifiers;
- included in Dispute Packs;
- preserved by archival services.

Publication improves availability and comparison only to the extent that the published Checkpoint is authentic, retrievable, and bound to a stable location or namespace.

A mutable web page or database row is not a substitute for the signed Checkpoint object.

---

# 10.31 Checkpoint Verification

Checkpoint Verification may require:

1. resolving the Checkpoint object and applicable versions;
2. validating structure, canonicalization, and digest;
3. validating the Checkpoint Authority Signature;
4. resolving Historical Key State, Key Purpose, and Checkpoint Authority;
5. validating Checkpoint Scope;
6. resolving covered Chains, positions, ranges, or aggregate proofs;
7. validating continuity to prior trusted boundaries;
8. validating bound Witness Observations and quorum evidence;
9. evaluating Checkpoint Time and cadence;
10. detecting conflicting Checkpoints or historical views;
11. resolving required dependencies;
12. evaluating Trust Profile achievement separately.

A verifier may begin from an earlier trusted Checkpoint rather than Chain genesis when the applicable policy permits it.

Successful Checkpoint Signature validation does not complete the remaining checks.

---

# 10.32 Conflicting Checkpoints and Equivocation

Conflicting Checkpoints may exist when:

- the same Checkpoint identifier binds different protected content;
- the same Chain position binds incompatible Chain Heads;
- overlapping ranges contain inconsistent history;
- the Checkpoint Authority signs incompatible aggregate roots;
- different observers receive different exclusive Checkpoints;
- a later Checkpoint is not continuous with an earlier trusted boundary.

```text
Prior Checkpoint
       │
       ├──► Checkpoint A
       └──► Checkpoint B
```

Conflicts must remain visible.

A newer Checkpoint must not silently overwrite an incompatible earlier Checkpoint.

Witness publication, external anchoring, independent retention, and cross-domain comparison may strengthen conflict detection.

---

# 10.33 Correction, Supersession, and Revocation

An issued Checkpoint may later require correction, annotation, supersession, revocation of reliance, or compromise disclosure.

The system must create additional accountable evidence identifying:

- the affected Checkpoint;
- the relationship type;
- reason and responsible Authority;
- effective time semantics;
- replacement or corrected boundary, if any;
- impact on prior anchors and Trust Profiles;
- unresolved historical limitations.

The original signed Checkpoint remains part of history.

Correction does not retroactively make the original boundary different from what was signed.

Anchor Evidence for the original Checkpoint also remains historical evidence.

---

# 10.34 Missed and Failed Checkpoints

A required Checkpoint may be missed or fail because of:

- unavailable Chain state;
- invalid or incomplete Witness Quorum;
- detected fork or conflict;
- Checkpoint Authority outage;
- signing-key failure;
- unsupported version;
- validation error;
- missed cadence window;
- policy or governance hold;
- anchor dependency failure.

The system should distinguish:

- no Checkpoint was required;
- Checkpoint not yet due;
- candidate incomplete;
- creation failed;
- issuance delayed;
- Checkpoint issued but unpublished;
- Checkpoint issued but unanchored;
- Checkpoint exists but is unavailable.

These states affect assurance differently.

---

# 10.35 Graceful Degradation

Governance may permit operation to continue when Checkpointing or External Anchoring is unavailable.

Degradation rules must define:

- which actions may proceed;
- maximum grace period;
- minimum remaining controls;
- retry and recovery obligations;
- later catch-up Checkpoint behavior;
- anchor resubmission behavior;
- notification and escalation;
- effect on Intended and Achieved Trust Profiles;
- evidence required to record the downgrade.

```text
Required Checkpoint / Anchor
          │
          ├── achieved ──► required assurance may be satisfied
          └── missing ───► explicit incomplete or downgraded outcome
```

Operational success must not be reported as full assurance success.

---

# 10.36 External Anchor Role

An External Anchor provides an independently retrievable environment in which a commitment to TrustAgentAI state is recorded.

The External Anchor operator may:

- accept publication requests;
- assign external identifiers;
- order or timestamp submissions;
- expose confirmation or finality state;
- retain externally committed data;
- provide inclusion or existence proofs;
- expose retrieval interfaces;
- publish governance and algorithm rules.

The TrustAgentAI system must evaluate what the external system actually establishes.

An anchor service is not assumed trustworthy for claims outside its defined native behavior.

---

# 10.37 Anchor Target

The **anchor target** is the exact TrustAgentAI commitment submitted for external publication.

It may be:

- a Checkpoint digest;
- a typed commitment to a Checkpoint;
- a Chain Head commitment;
- an aggregate Checkpoint root;
- a migration or closure commitment;
- another TAIP-defined historical commitment.

The target must bind sufficient context to prevent reinterpretation across:

- object types;
- CIDs;
- versions;
- networks or namespaces;
- deployments;
- Trust Profiles;
- cryptographic domains.

A bare digest without governed context may be ambiguous.

---

# 10.38 External System and Namespace

Anchor Evidence must identify the external system and namespace in which publication occurred.

Relevant properties may include:

- system or network identifier;
- environment, instance, or chain identifier;
- namespace or account;
- protocol and version;
- publication endpoint or method;
- external object or transaction type;
- governance authority;
- expected finality semantics;
- retrieval and proof rules.

```text
External Network Identifier
≠
External Account
≠
External Transaction Identifier
≠
TrustAgentAI Checkpoint Identifier
```

Test, staging, forked, and production environments must not be silently interchangeable.

---

# 10.39 Anchoring Method

The anchoring method defines how the TrustAgentAI commitment is represented in the external system.

Methods may include:

- direct publication of a typed commitment;
- inclusion in an external transaction payload;
- submission to a timestamp service;
- inclusion in an external transparency tree;
- notarized or regulated archival deposit;
- publication through a counterparty-controlled register;
- batch aggregation followed by external publication.

The method must define:

- input construction;
- domain separation;
- encoding;
- namespace binding;
- inclusion or retrieval proof;
- confirmation and finality rules;
- failure and retry behavior.

---

# 10.40 Anchor Evidence

**Anchor Evidence (AE)** is a TrustAgentAI Protocol Object or governed evidence package supporting Verification that a defined TrustAgentAI commitment was placed into an External Anchor.

Its logical semantics may include:

- Anchor Evidence type and version;
- Anchor Evidence identifier;
- anchor target commitment;
- Checkpoint or state reference;
- external system and namespace;
- anchoring method;
- external transaction, record, or publication identifier;
- submission, publication, confirmation, and finality state;
- time evidence;
- proof and retrieval material;
- responsible submitter or service identity;
- algorithms and extensions;
- cryptographic protection.

Anchor Evidence is distinct from the external record itself.

---

# 10.41 External Identifier and Locator

An external transaction, record, block, publication, or certificate identifier may help locate anchor state.

The identifier must not be treated as proof by itself.

Verification may require:

- authenticated resolution;
- external-system namespace;
- exact payload or commitment extraction;
- inclusion proof;
- confirmation state;
- historical external-system rules;
- binding to the TrustAgentAI anchor target.

```text
External Identifier
      +
Authenticated External Record
      +
Target-Binding Verification
      =
Potential Anchor Evidence
```

A URL may change or become unavailable while the underlying external identifier remains valid.

---

# 10.42 Publication and Confirmation State

External anchor lifecycle states may include:

- prepared;
- submitted;
- accepted;
- published;
- included;
- confirmed;
- finalized under defined assumptions;
- rejected;
- replaced;
- reorganized;
- expired;
- unavailable;
- conflicting.

These states are not interchangeable.

A submission receipt is not automatically proof of publication.

Publication is not automatically irreversible finality.

Anchor Evidence must state the external state actually supported at the relevant Verification Time.

---

# 10.43 Publication Time

**Publication Time** is the time associated with appearance of the anchor target in the external system under defined semantics.

Its assurance may depend upon:

- an external timestamp assertion;
- ordered block or log position;
- service receipt;
- confirmation boundary;
- independent observation;
- external governance and clock assumptions.

Publication Time may support an upper bound on when the anchored commitment existed.

It does not automatically prove:

- Event Time;
- original Commitment Time;
- Checkpoint Authority clock accuracy;
- permanent finality;
- semantic validity of the Checkpoint.

---

# 10.44 Finality, Reorganization, and Reversal

External systems may have different finality models.

Finality may be:

- immediate by service assertion;
- probabilistic after confirmations;
- governed by a consensus protocol;
- subject to administrative reversal;
- subject to ledger reorganization;
- subject to legal or archival correction;
- bounded by retention or availability policy.

Anchor profiles must define when an external record is considered sufficiently final for a TrustAgentAI claim.

Reorganization, replacement, reversal, or invalidation must remain visible.

A previously valid anchor conclusion may require reevaluation when external finality changes.

---

# 10.45 Anchor Proof and Retrieval

External Anchor Verification may require proof that the target commitment appears in the external state.

Proof forms may include:

- authenticated record retrieval;
- transaction payload verification;
- Merkle or transparency inclusion proof;
- timestamp token validation;
- archival receipt and signature;
- externally signed publication statement;
- block header and confirmation path;
- another governed native proof.

Proof rules must bind:

- external namespace;
- target commitment;
- external record or root;
- algorithm and version;
- proof type;
- confirmation or finality boundary.

Proof verification must be bounded and independently reproducible.

---

# 10.46 Multiple External Anchors

A Trust Profile may require publication into more than one External Anchor.

Multiple anchors may reduce dependence upon:

- one operator;
- one governance system;
- one algorithm family;
- one jurisdiction;
- one infrastructure provider;
- one availability model.

Multiple anchors provide stronger diversity only when relevant Control Domains and failure modes are actually distinct.

Two gateways submitting to the same external system do not necessarily create two independent anchors.

Multi-anchor policy must define threshold, timing, target equality, finality, and conflict behavior.

---

# 10.47 Anchor Control Domains and Independence

External anchoring assurance depends upon the relationship between:

- Checkpoint Authority;
- anchor submitter;
- External Anchor operator;
- underlying consensus or publication operators;
- proof provider;
- archival or retrieval service;
- verifier.

An anchor operated within the same effective Control Domain may still provide operational durability.

It may not satisfy a Trust Profile requiring an external independent boundary.

Public accessibility alone does not prove independent governance.

Control Domain and correlated failure evidence must remain explicit.

---

# 10.48 Anchoring Procedure

A representative anchoring procedure is:

1. select the applicable anchor policy and method;
2. resolve the finalized Checkpoint or target state;
3. construct the typed anchor target commitment;
4. select the correct external system and namespace;
5. submit the target through the governed method;
6. retain submission evidence;
7. monitor publication and confirmation state;
8. retrieve or construct external proof material;
9. create Anchor Evidence;
10. validate target binding and finality;
11. publish and preserve Anchor Evidence;
12. report failure, delay, reorganization, or conflict.

Retry must not create ambiguity about which external records correspond to the same anchor target.

---

# 10.49 External Anchor Verification

A representative verifier should:

1. validate the Checkpoint or target commitment;
2. validate Anchor Evidence type, version, and integrity;
3. identify the external system, namespace, and method;
4. resolve the external identifier through an authenticated mechanism;
5. retrieve or validate native external proof;
6. reproduce target extraction and binding;
7. evaluate Publication Time semantics;
8. evaluate confirmation, finality, and reorganization status;
9. evaluate External Anchor Control Domain and policy eligibility;
10. detect conflicting external records or Anchor Evidence;
11. report missing or unsupported dependencies;
12. bound the resulting conclusion.

Anchor Evidence may be structurally valid while the external record remains pending or unavailable.

---

# 10.50 Pending, Failed, and Unavailable Anchors

Anchor processing may produce:

- submission pending;
- accepted but not published;
- published but not sufficiently confirmed;
- rejected;
- submission expired;
- external record reorganized or reversed;
- proof unavailable;
- external system unavailable;
- namespace unsupported;
- target mismatch;
- conflicting Anchor Evidence;
- success under a lower assurance policy.

These outcomes must remain distinct.

An anchor identifier that cannot be resolved or interpreted does not provide intended anchor assurance merely because it is syntactically present.

---

# 10.51 Replay and Context Binding

Checkpoints and Anchor Evidence must resist inappropriate reuse across incompatible contexts.

Protected input may need to bind:

- Checkpoint and anchor object type;
- CIDs and Chain versions;
- Checkpoint Scope;
- target commitment;
- external network and namespace;
- anchoring method;
- Trust Profile;
- validity or publication window;
- environment and deployment domain;
- critical extensions.

A valid anchor of one Checkpoint must not be presented as an anchor of another merely because both use the same hash algorithm or external identifier format.

---

# 10.52 Privacy and Confidentiality

Checkpoints and anchors can expose durable information about:

- Chain identities and growth;
- transaction or event volume;
- timing and cadence;
- tenant or workflow relationships;
- Witness composition;
- risk tiers and Trust Profiles;
- organizational and infrastructure topology;
- incident, migration, or legal-hold activity.

Privacy controls may include:

- aggregate commitments;
- typed blinded commitments;
- tenant separation;
- delayed or batched publication;
- encrypted supporting material;
- selective disclosure;
- private external namespaces;
- access-controlled proof services.

Hashing low-entropy or guessable values does not guarantee confidentiality.

---

# 10.53 Retention and Preservation

Checkpoint and anchor Preservation must cover both protected objects and interpretation dependencies.

Relevant material includes:

- Checkpoint objects and historical versions;
- covered Chain and aggregate proofs;
- Witness Observations and quorum policies;
- Checkpoint Authority identity and Historical Key State;
- Checkpoint policy and cadence rules;
- Anchor Evidence;
- external identifiers and native proof material;
- external-system rules and finality definitions;
- correction, conflict, reorganization, and migration evidence;
- schemas, algorithms, registries, and Trust Profiles.

External publication does not replace Preservation of underlying TrustAgentAI evidence.

Preservation must account for external services that may disappear or change interfaces.

---

# 10.54 Versioning and Algorithm Agility

Checkpoint and anchor interpretation may depend upon:

- TAIP version;
- Checkpoint object and scope version;
- Chain and aggregate construction;
- canonicalization version;
- Signature and digest algorithms;
- Witness quorum policy;
- anchor method and external protocol version;
- finality policy;
- extension versions.

Historical Verification must use the versions applicable to the original boundary.

Algorithm transition may use dual commitments, transition Checkpoints, multi-anchor overlap, or governed migration evidence.

A new algorithm must not silently reinterpret old Checkpoint or anchor material.

---

# 10.55 Authority, Service, and Namespace Migration

Checkpoint or anchor infrastructure may migrate because of organizational change, compromise, deprecation, algorithm transition, or service retirement.

Migration evidence should bind:

- source and destination identities;
- source final Checkpoint or anchor state;
- destination activation boundary;
- transferred or changed Authority;
- old and new keys, methods, algorithms, and namespaces;
- effective time;
- continuity or explicit discontinuity;
- unresolved conflicts and unavailable dependencies;
- supporting Witness or external evidence.

Changing an endpoint or database is not sufficient evidence of migration.

A deprecated External Anchor may remain required for historical Verification.

---

# 10.56 Validation Layers

Checkpoint and anchor evaluation is layered.

## Availability and Parsing

Can the Checkpoint, Anchor Evidence, external proof, and dependencies be obtained and parsed safely?

## Structural and Semantic Validation

Do type, version, scope, target, range, time, state, and extension values satisfy applicable rules?

## Canonicalization and Cryptographic Validation

Can protected input be reproduced and required digests and Signatures be validated?

## Authority Validation

Was the Checkpoint Authority or anchor submitter authorized for the relevant namespace and purpose?

## Covered-State Validation

Do Chain, range, aggregate, and Witness commitments match the declared Checkpoint Scope?

## Historical Validation

Do historical keys, registries, policies, algorithms, and prior boundaries support interpretation?

## External-Binding Validation

Does native external proof bind the exact TrustAgentAI anchor target to the claimed system and namespace?

## Time and Finality Validation

Do Checkpoint Time, Publication Time, confirmation, and reorganization state support the claimed boundary?

## Conflict Validation

Are incompatible Checkpoints, anchors, external records, gaps, and reversals visible?

## Profile and Completeness Validation

Are cadence, Witness, multi-anchor, Control Domain, Preservation, and coverage requirements satisfied?

---

# 10.57 Verification Outcomes

Checkpoint and anchor Verification should distinguish at least:

- valid;
- invalid;
- indeterminate;
- incomplete;
- unsupported;
- conflicting;
- unavailable;
- pending;
- reorganized or reversed;
- valid with limitations.

Relevant sub-results may include:

- Checkpoint Signature valid;
- Checkpoint Authority unresolved;
- covered Chain state valid;
- Witness Quorum incomplete;
- cadence missed;
- anchor target binding valid;
- external publication confirmed;
- finality insufficient;
- external proof unavailable;
- required independent Control Domain not established;
- underlying evidence unavailable.

A single boolean must not hide these distinctions.

---

# 10.58 Portability and Export

A portable Checkpoint and anchor evidence package should contain or reference enough governed material to reproduce its intended conclusions outside the originating platform.

Depending upon scope, it may include:

- Checkpoint object;
- covered Chain Heads, ranges, or aggregate proofs;
- prior Checkpoints;
- Witness Observations and quorum evidence;
- Checkpoint Authority identity and historical key evidence;
- Checkpoint policy and cadence evidence;
- Anchor Evidence;
- native external records and proofs;
- external-system and finality definitions;
- Trust Profile and Registry versions;
- conflicts, failures, omissions, and limitations.

Package layout does not replace protected relationships or native external proof.

---

# 10.59 Implementation Mapping and Illustrative Sequence

Checkpoint and anchor roles may be implemented through:

- a dedicated Checkpoint service;
- a separately governed module of a Registry or Chain platform;
- a consortium authority;
- a regulated or customer-operated assurance service;
- a transparency-log publisher;
- a timestamp provider;
- a public or permissioned ledger gateway;
- a regulated archive;
- a hybrid multi-anchor service.

Implementation technology does not change the architectural requirements.

```text
Chain Operator        Witnesses        Checkpoint Authority       External Anchor       Verifier
     │                    │                    │                         │                  │
     │ Chain state H      │                    │                         │                  │
     │────────────────────────────────────────►│                         │                  │
     │                    │ WQ evidence         │                         │                  │
     │                    │────────────────────►│                         │                  │
     │                    │                    │ validate and sign CP    │                  │
     │                    │                    │────────────────────────►│                  │
     │                    │                    │      publish commitment │                  │
     │                    │                    │◄────────────────────────│                  │
     │                    │                    │   external proof / state│                  │
     │                    │                    │                                            │
     │                    │        CP + AE + dependencies                                  │
     │                    │───────────────────────────────────────────────────────────────►│
     │                    │                    │                         │     verify layers │
```

The Checkpoint and Anchor Evidence remain separately verifiable.

---

# 10.60 Common Anti-Patterns and Relationship to Other Specifications

## Head Without Scope

Signing a Chain Head without binding CID, version, position, range, or aggregate context.

## Signature Equals Checkpoint Validity

Treating a valid Checkpoint Authority Signature as proof that covered Chain and Witness state are valid.

## Same-Control Independence

Representing a Checkpoint service controlled by the Chain operator as an independent authority without Control Domain evidence.

## Cadence by Configuration

Claiming cadence compliance because a schedule exists rather than because required Checkpoints were issued on time.

## Witness Quorum by Inclusion

Treating Witness material embedded in a Checkpoint as a valid quorum without historical eligibility and independence evaluation.

## Mutable Latest Checkpoint

Replacing an earlier signed Checkpoint with a current value and hiding the historical object.

## Submission Equals Anchoring

Treating anchor request acceptance as external publication or finality.

## Transaction ID Equals Proof

Presenting an external identifier without authenticated record, namespace, payload, and target-binding verification.

## Blockchain Equals Truth

Treating public-ledger inclusion as validation of underlying business assertions.

## Confirmation Equals Permanent Finality

Ignoring reorganization, reversal, administrative correction, or retention semantics of the external system.

## Same-System Multi-Anchor

Counting multiple gateways or accounts in one external Control Domain as independent anchors.

## Current External Rules

Using current network, algorithm, or finality rules to interpret historical Anchor Evidence without versioning.

## Anchor as Preservation

Assuming a compact external commitment keeps underlying evidence and dependencies available.

## Hashing Sensitive State

Publishing guessable or linkable sensitive commitments without privacy analysis.

This chapter defines Checkpoint, Checkpoint Authority, External Anchor, and Anchor Evidence semantics.

Other chapters and TAIP define related concerns, including:

- Protocol Identity and Key Transparency;
- Preservation and cryptographic renewal;
- Trust Profiles and assurance levels;
- Verification Engines and Reports;
- Dispute Pack construction;
- security, privacy, and threat models;
- concrete schemas, algorithms, APIs, registries, and test vectors.

Those specifications may strengthen or specialize Checkpoint and anchoring requirements.

They must preserve the invariants defined here.

---

# Checkpoint and External Anchor Invariants

### INV-CP-001 — Explicit Checkpoint Scope

Every Checkpoint MUST define and cryptographically bind the exact historical state and boundary it commits to.

### INV-CP-002 — Stable Checkpoint Identity

Every finalized interoperable Checkpoint MUST possess a stable governed identifier distinct from its digest, locator, and external identifiers unless an applicable version defines a content-derived relationship.

### INV-CP-003 — Checkpoint Authority Attribution

Every Checkpoint MUST identify the Checkpoint Authority responsible for its protected statement.

### INV-CP-004 — Identity/Key/Authority Separation

Checkpoint Authority identity, signing key, Key Purpose, operational control, and Authority over covered state MUST remain distinguishable.

### INV-CP-005 — Signature/Independence Separation

A valid Checkpoint Signature MUST NOT automatically be interpreted as valid Authority, independence, correct covered state, or Trust Profile achievement.

### INV-CP-006 — Exact Covered-State Binding

Covered CIDs, Chain Heads, positions, ranges, aggregates, versions, and required context MUST be bound sufficiently to prevent substitution.

### INV-CP-007 — Witness/Checkpoint Separation

Inclusion of Witness evidence in a Checkpoint MUST NOT replace historical Witness eligibility, independence, scope, timing, and quorum evaluation.

### INV-CP-008 — Chain/Checkpoint Separation

A Checkpoint MUST remain distinguishable from the Chain state, Chain Entry, or aggregate it commits to.

### INV-CP-009 — Checkpoint/Anchor Separation

A Checkpoint, anchor target, external record, and Anchor Evidence MUST remain separately identifiable and verifiable.

### INV-CP-010 — Lifecycle Separation

Candidate creation, Checkpoint issuance, publication, anchor submission, external inclusion, confirmation, finality, Preservation, and Verification MUST NOT be conflated.

### INV-CP-011 — Time Separation

Commitment Time, Observation Time, Checkpoint Time, Signature Time, Publication Time, confirmation time, and Verification Time MUST remain distinguishable.

### INV-CP-012 — Deterministic Protected Input

Equivalent conforming Checkpoint or Anchor Evidence input under the same governed context MUST produce equivalent cryptographic input and results.

### INV-CP-013 — Domain Separation

Chain Heads, Checkpoint commitments, anchor targets, external native commitments, and Anchor Evidence digests MUST remain cryptographically distinguishable.

### INV-CP-014 — Committed Immutability

Protected content of an issued Checkpoint or finalized Anchor Evidence MUST NOT be silently modified.

### INV-CP-015 — Append-Only Correction

Correction, supersession, revocation, reorganization, reversal, and changed reliance MUST create additional accountable evidence.

### INV-CP-016 — Cadence as Achieved Evidence

Configured cadence or intended policy MUST NOT be represented as achieved cadence without the required historical Checkpoints.

### INV-CP-017 — Conflict Visibility

Incompatible Checkpoints, target commitments, external records, and Anchor Evidence MUST remain visible to Verification.

### INV-CP-018 — No Implied Business Truth

Valid Checkpoint or anchor evidence MUST NOT be represented as proof of underlying business truth, Authority, legality, or correct execution.

### INV-CP-019 — No Implied Completeness

Checkpoint or anchor validity MUST remain distinguishable from evidence Completeness and closed-population coverage.

### INV-CP-020 — Preservation/Anchoring Separation

External anchoring MUST NOT be represented as Preservation of underlying evidence or interpretation dependencies.

### INV-CP-021 — External Namespace Binding

Anchor Evidence MUST bind the external system, environment, namespace, method, and version required to interpret the external record.

### INV-CP-022 — Exact Anchor-Target Binding

External proof MUST bind the exact typed TrustAgentAI anchor target to the claimed external representation.

### INV-CP-023 — Identifier/Proof Separation

An external transaction, record, publication identifier, or locator MUST NOT be represented as sufficient anchor proof by itself.

### INV-CP-024 — Publication/Finality Separation

Submission, publication, inclusion, confirmation, and finality MUST remain distinct external anchor states.

### INV-CP-025 — Reorganization Visibility

External reorganization, reversal, replacement, expiration, or loss of confirmation MUST remain visible where it affects Verification.

### INV-CP-026 — Control-Domain Visibility

Common control and correlated failure among Chain, Checkpoint, anchor, proof, and Preservation roles MUST remain visible.

### INV-CP-027 — Intended/Achieved Separation

An Intended Trust Profile or configured anchor policy MUST NOT be represented as proof that required Checkpoint, Witness, anchor, or finality conditions were achieved.

### INV-CP-028 — Historical Interpretation

Checkpoint and Anchor Evidence MUST be interpreted using applicable historical identities, keys, policies, versions, algorithms, external-system rules, and finality semantics.

### INV-CP-029 — Explicit Uncertainty

Missing, pending, unavailable, unsupported, conflicting, reversed, redacted, and unverifiable dependencies MUST remain explicit.

### INV-CP-030 — Privacy Proportionality

Checkpoint and anchor designs SHOULD minimize unnecessary disclosure and linkability while preserving required accountability semantics.

### INV-CP-031 — Representation Independence

Normative Checkpoint and anchor meaning MUST NOT depend upon one database, transport, vendor, external network, or user interface.

### INV-CP-032 — Bounded Reproducible Verification

Independent conforming implementations SHOULD derive equivalent bounded Checkpoint and anchor conclusions from equivalent evidence under equivalent Verification Contexts.

---

# Architectural Requirements

### REQ-CP-001

TAIP MUST define or reference governed Checkpoint, Checkpoint Scope, anchor target, and Anchor Evidence object types and versions.

### REQ-CP-002

TAIP MUST define Checkpoint identifier namespace, generation responsibility, syntax, comparison behavior, persistence, collision handling, and digest relationship.

### REQ-CP-003

Every Checkpoint MUST identify and cryptographically bind its Checkpoint Scope under governed semantics.

### REQ-CP-004

Every Checkpoint MUST identify the Checkpoint Authority responsible for issuance.

### REQ-CP-005

Checkpoint Signature evaluation MUST resolve Key Identifier, Key Purpose, Historical Key State, algorithm policy, and protected scope.

### REQ-CP-006

Verification MUST evaluate the Checkpoint Authority's historical Authority over the covered Chain, aggregate, Registry, or other namespace separately from Signature validity.

### REQ-CP-007

Checkpoints covering Hash Chains MUST bind applicable CID, Chain version, Chain Head, position or range, epoch or shard context, and algorithm information required by the scope.

### REQ-CP-008

Range Checkpoints MUST define endpoint inclusion, starting and ending state, continuity, gaps, sparse positions, epoch context, and relationship to prior boundaries.

### REQ-CP-009

Aggregate Checkpoints MUST define member identity, ordering, duplicates, omissions, construction, proof, partial failure, cardinality, and privacy behavior.

### REQ-CP-010

Checkpoints claiming Witness Quorum support MUST bind the historical quorum policy, target, contributing evidence, Control Domains, timing, threshold result, exclusions, and conflicts required for independent evaluation.

### REQ-CP-011

Mandatory Checkpoint dependencies MUST be typed, integrity-bound, and either resolvable or explicitly reported as unavailable.

### REQ-CP-012

Each Checkpoint and Anchor Evidence version MUST define or reference deterministic canonicalization, cryptographic input construction, and domain separation.

### REQ-CP-013

Substitution-sensitive type, identifier, Authority, scope, covered-state, time, policy, profile, namespace, method, state, and extension semantics MUST be cryptographically bound.

### REQ-CP-014

Checkpoint Time and Publication Time values MUST identify their meaning, source, precision, and supporting assurance and MUST NOT be silently treated as equivalent.

### REQ-CP-015

Checkpoint cadence policies MUST define frequency or trigger, applicable scope, grace interval, missed-boundary behavior, and effect on Achieved Trust Profile.

### REQ-CP-016

Failure to create, issue, publish, or preserve a required Checkpoint MUST produce an explicit state and MUST NOT be silently converted into success.

### REQ-CP-017

A Checkpoint candidate MUST remain distinguishable from an authorized, signed, and issued Checkpoint.

### REQ-CP-018

Checkpoint creation MUST apply and identify the Chain, range, aggregate, Witness, policy, cadence, conflict, and dependency validation required by the applicable profile.

### REQ-CP-019

Implementations MUST preserve the distinction between Checkpoint candidate, issuance, publication, anchoring, confirmation, finality, Preservation, and Verification states.

### REQ-CP-020

Protected Checkpoint and Anchor Evidence content MUST NOT be modified in place after finalization.

### REQ-CP-021

Correction, supersession, revocation, reorganization, reversal, and changed reliance MUST identify affected objects and create additional accountable evidence.

### REQ-CP-022

Incompatible Checkpoints for an exclusive historical boundary MUST remain visible and MUST NOT be silently resolved through last-write-wins behavior.

### REQ-CP-023

Missed, delayed, failed, unpublished, unanchored, unavailable, and invalid Checkpoint states MUST remain distinguishable where known.

### REQ-CP-024

Graceful degradation rules MUST record missing required controls and MUST NOT represent the Intended Trust Profile as achieved when Checkpoint or anchor conditions are unsatisfied.

### REQ-CP-025

Every anchor target MUST identify or bind the TrustAgentAI commitment type, version, protocol domain, and namespace context required to prevent reinterpretation.

### REQ-CP-026

Anchor Evidence MUST identify the exact external system, environment, namespace, protocol version, anchoring method, and external object type.

### REQ-CP-027

Each anchoring method MUST define input construction, domain separation, encoding, submission, proof, confirmation, finality, retry, and failure semantics.

### REQ-CP-028

Anchor Evidence MUST identify or bind the anchor target, external identifier, publication state, time evidence, proof material, applicable versions, and responsible identity required by its claim.

### REQ-CP-029

An external identifier or locator MUST NOT be treated as successful Anchor Evidence without authenticated resolution and target-binding validation required by the method.

### REQ-CP-030

Anchor lifecycle evaluation MUST distinguish prepared, submitted, accepted, published, included, confirmed, finalized, rejected, replaced, reorganized, expired, unavailable, and conflicting states where applicable.

### REQ-CP-031

Publication Time conclusions MUST be bounded to the external mechanism's actual clock, ordering, confirmation, and proof semantics.

### REQ-CP-032

Anchor profiles MUST define the confirmation or finality condition required for the applicable Trust Profile and Verification Context.

### REQ-CP-033

Reorganization, reversal, replacement, expiration, or changed confirmation state MUST trigger explicit reevaluation of affected anchor conclusions.

### REQ-CP-034

External proof formats MUST bind the external namespace, target commitment, native record or root, algorithm, proof type, and confirmation boundary.

### REQ-CP-035

Multi-anchor policies MUST define eligible anchors, target equality, threshold, Control Domain, timing, finality, conflict, and unavailable-anchor behavior.

### REQ-CP-036

Claims of external independence MUST identify applicable Control Domain criteria and supporting evidence rather than rely on public accessibility or service naming.

### REQ-CP-037

Retry and idempotency behavior MUST preserve the relationship among one anchor target, multiple submissions, resulting external records, duplicates, replacements, and failures.

### REQ-CP-038

Replay-sensitive Checkpoint and Anchor Evidence MUST bind the Chain, scope, target, external namespace, method, environment, profile, and other context required by the applicable threat model.

### REQ-CP-039

Pending, failed, rejected, unsupported, unavailable, reorganized, and conflicting anchor outcomes MUST affect Verification explicitly.

### REQ-CP-040

Unknown critical Checkpoint or Anchor Evidence extensions MUST produce an unsupported or indeterminate outcome for affected claims.

### REQ-CP-041

Checkpoint and anchor designs SHOULD minimize unnecessary disclosure of identifiers, topology, timing, volume, Witness composition, Trust Profile, and underlying content.

### REQ-CP-042

Encrypted, blinded, redacted, or selectively disclosed commitments MUST preserve the binding and opening semantics required by the evaluated claim and expose resulting limitations.

### REQ-CP-043

Implementations MUST bound Checkpoint size, aggregate cardinality, Witness evidence, proof size, parsing effort, dependency depth, external queries, and cryptographic work.

### REQ-CP-044

Preservation planning SHOULD include Checkpoints, covered-state proofs, Witness evidence, Authority and key history, policies, Anchor Evidence, external proof, finality rules, conflicts, and migrations required for the Evidence Lifetime.

### REQ-CP-045

Historical Verification MUST use applicable historical Checkpoint policies, identities, keys, algorithms, Witness rules, external-system versions, and finality semantics rather than current defaults.

### REQ-CP-046

Checkpoint Authority, anchor service, method, namespace, or algorithm migration MUST bind source and destination boundaries, continuity or discontinuity, effective time, Authority, and unresolved limitations.

### REQ-CP-047

Checkpoint and anchor validation SHOULD distinguish availability, structural, semantic, canonicalization, cryptographic, Authority, covered-state, Witness, historical, external-binding, time, finality, conflict, Completeness, and profile results.

### REQ-CP-048

Verification Outcomes MUST distinguish valid, invalid, indeterminate, incomplete, unsupported, conflicting, unavailable, pending, reorganized, and valid-with-limitations states where applicable.

### REQ-CP-049

Portable evidence packages SHOULD identify included, omitted, redacted, pending, unavailable, conflicting, reorganized, externally resolved, and unsupported material required by the intended conclusion.

### REQ-CP-050

Trust Profiles using Checkpoints or External Anchors MUST define required cadence, scope, Authority, Witness relationship, eligible Control Domains, anchor methods, timing, finality, Preservation, and degradation behavior.

### REQ-CP-051

Verification Reports MUST bound conclusions to the evaluated Checkpoint Scope, anchor target, external namespace, historical policies, Verification Time, evidence set, Trust Profile, and unresolved limitations.

### REQ-CP-052

Independent conforming implementations SHOULD derive equivalent Checkpoint, external-binding, time, finality, and profile conclusions from equivalent evidence under equivalent Verification Contexts.

---

# Security Considerations

Checkpoints and External Anchors strengthen historical assurance but introduce concentrated signing, aggregation, external-dependency, and cross-system interpretation risks.

Major threats include:

- forging or misusing a Checkpoint Authority identity or key;
- issuing a Checkpoint without Authority over the covered namespace;
- substituting CID, Chain Head, range, aggregate, or Checkpoint Scope;
- omitting a shard, Chain, Witness, or conflict from an aggregate boundary;
- presenting invalid Witness material as a valid quorum;
- backdating Checkpoint Time;
- suppressing missed Checkpoints or cadence failures;
- issuing incompatible Checkpoints to different observers;
- modifying a finalized Checkpoint;
- substituting another external network, environment, account, or namespace;
- anchoring a bare ambiguous digest;
- fabricating or replaying an external identifier;
- treating submission as publication or publication as finality;
- ignoring external reorganization, reversal, expiration, or governance change;
- manipulating an anchor gateway or proof provider;
- representing correlated anchors as independent;
- exhausting verifiers with large aggregates, remote queries, or proof chains;
- losing historical algorithms, network rules, or finality definitions;
- leaking sensitive activity through public commitments and cadence;
- treating external publication as Preservation of underlying evidence.

Implementations should apply controls including:

- governed Checkpoint Authority identity, Key Purpose, and namespace Authority;
- deterministic scope and covered-state binding;
- historical key, Registry, policy, and Witness evaluation;
- conflict-preserving Checkpoint publication;
- cadence monitoring and explicit failure evidence;
- typed, domain-separated anchor targets;
- authenticated external-system and namespace resolution;
- native proof and finality validation;
- reorganization and reversal monitoring;
- Control Domain analysis for Checkpoint and anchor roles;
- bounded parsing, aggregation, proof, and external query behavior;
- append-only correction and migration;
- privacy-aware aggregation and disclosure;
- durable Preservation of external interpretation dependencies;
- layered, reproducible Verification.

A malicious Checkpoint Authority may sign a self-consistent but false boundary.

A malicious anchor gateway may claim publication that never occurred.

Independent Verification must therefore validate covered state, Authority, native external proof, and historical context rather than trusting labels or receipts.

---

# Privacy Considerations

Checkpoints and anchors are deliberately durable and widely comparable, which can amplify privacy risk.

Even when underlying content is not disclosed, observers may infer:

- activity volume from cadence;
- operational incidents from emergency Checkpoints;
- tenant relationships from aggregate membership;
- risk tiers from anchor method or frequency;
- organizational topology from Authority and Witness data;
- business timing from Publication Time;
- repeated subjects from stable commitments.

Privacy analysis should consider whether:

- several entries can be aggregated safely;
- member inclusion proofs can be selectively disclosed;
- commitments require salt, blinding, or another hiding construction;
- tenants require separate namespaces;
- publication can be delayed without violating assurance;
- Witness and Control Domain details can be disclosed only to authorized verifiers;
- external records are subject to deletion or retention constraints;
- public proof material creates dictionary or correlation attacks.

Data minimization must occur before durable Checkpointing and anchoring because protected historical state cannot later be silently rewritten.

---

# Design Rationale

TrustAgentAI uses Checkpoints because replaying an entire historical Chain from genesis for every decision is expensive and because a compact trusted boundary can make rollback and later inconsistency easier to detect.

Checkpoint Scope prevents a compact commitment from becoming a vague claim about all history.

Checkpoint Authority identity and Authority make responsibility for the boundary explicit.

Binding Witness Quorum evidence allows independent observation to contribute to the boundary without pretending that Checkpoint Signature validates Witness eligibility automatically.

Cadence transforms occasional publication into a measurable assurance control.

Conflict preservation prevents latest-state interfaces from erasing incompatible signed boundaries.

External Anchoring adds a distinct trust boundary by placing the exact Checkpoint commitment into another system.

Anchor Evidence is required because a native external record, transaction identifier, or timestamp token must be related unambiguously to TrustAgentAI semantics.

Explicit confirmation and finality rules prevent provisional publication from becoming false irreversible proof.

Multi-anchor and Control Domain analysis reduce correlated dependence only when the relevant systems are genuinely distinct.

Preservation retains the Checkpoint, native proof, external rules, and underlying dependencies needed to interpret the boundary later.

The stable objective is:

> **A verifier should be able to identify the exact historical boundary, prove who committed to it, reproduce the supporting Witness and Chain checks, and validate where and when the same commitment crossed an external trust boundary.**

---

# Summary

Checkpoints and External Anchors create durable historical boundaries for TrustAgentAI evidence.

A conforming model establishes the following discipline:

1. every Checkpoint has explicit governed scope;
2. Checkpoint identity remains distinct from digest and locator;
3. Checkpoint Authority identity, key, Key Purpose, Authority, and independence remain separate;
4. covered CIDs, Chain Heads, positions, ranges, aggregates, and versions are bound precisely;
5. Witness Quorum evidence remains independently evaluable;
6. Chain state, Checkpoint, anchor target, external record, and Anchor Evidence remain distinct;
7. candidate, issued, published, anchored, confirmed, finalized, preserved, and verified states remain distinct;
8. Checkpoint Time remains separate from Publication and confirmation time;
9. cadence is evaluated from historical evidence rather than configuration;
10. missed and failed Checkpoints remain visible;
11. incompatible Checkpoints and external records are not silently overwritten;
12. correction and supersession create additional accountable history;
13. graceful degradation produces explicit assurance downgrade;
14. anchor targets are typed and domain-separated;
15. external system, environment, namespace, method, and version are explicit;
16. an external identifier is not proof without authenticated resolution and target binding;
17. submission, publication, inclusion, confirmation, and finality remain distinct;
18. reorganization, reversal, and expiration trigger reevaluation;
19. multi-anchor assurance depends upon genuine Control Domain diversity;
20. external publication does not replace underlying Preservation;
21. privacy is protected before durable publication;
22. Verification reports covered state, Authority, Witness support, cadence, external binding, finality, Completeness, and limitations separately.

The foundational rule is:

> **Checkpoint the exact historical boundary, anchor the exact typed commitment, and never confuse publication with truth or finality.**

The broader architectural principle remains:

> **Proof, not logs.**
