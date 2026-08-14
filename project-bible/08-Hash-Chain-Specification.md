# Chapter 8 — Hash Chain Specification

> **A Hash Chain binds evidence to an ordered historical state that cannot be changed silently without changing the resulting Chain state.**

## Purpose

This chapter defines the architectural specification for the TrustAgentAI **Hash Chain**.

The Hash Chain is the principal mechanism through which finalized Protocol Objects enter protected ordered history.

It provides a governed relationship between:

- a committed Protocol Object or object commitment;
- a Chain Entry;
- the preceding Chain state;
- the resulting Chain Head;
- Commitment evidence;
- later Witness Observations, Checkpoints, External Anchors, Preservation, and Verification.

This chapter establishes:

- the Hash Chain semantic boundary;
- Chain Identifier and Chain Head semantics;
- Chain Entry and genesis rules;
- object-to-history binding;
- ordering and concurrency behavior;
- Commitment Time semantics;
- Commitment Receipts;
- inclusion, continuity, fork, and equivocation analysis;
- batching, segmentation, rollover, and migration rules;
- correction and deletion behavior;
- validation and Verification requirements;
- Hash Chain invariants;
- architectural requirements for interoperable implementations.

This chapter applies the common Protocol Object rules defined in [06-Protocol-Objects.md](06-Protocol-Objects.md).

It builds upon the Evidence Record model in [07-Evidence-Record-Specification.md](07-Evidence-Record-Specification.md), the system model in [05-System-Overview.md](05-System-Overview.md), and the principles in [04-Design-Principles.md](04-Design-Principles.md).

It uses the canonical terms defined in [Terminology.md](Terminology.md) and [Acronyms.md](Acronyms.md).

This chapter defines architectural semantics.

It does not define final field names, concrete schemas, media types, wire encodings, canonicalization algorithms, hash functions, Signature suites, transport endpoints, database layouts, consensus protocols, or deployment-specific replication mechanisms.

Those details belong to TAIP, governed registries, schemas, cryptographic profiles, test vectors, and implementation bindings.

---

# 8.1 Hash Chain Definition

A **Hash Chain** is a governed sequence of cryptographically linked historical states.

Each accepted Chain Entry binds defined committed material to the preceding Chain state and produces a new Chain Head.

```text
Genesis State
      │
      ▼
Chain Entry 1 ──► Chain Head 1
      │
      ▼
Chain Entry 2 ──► Chain Head 2
      │
      ▼
Chain Entry n ──► Chain Head n
```

Changing a protected earlier entry, its committed material, its order, or its predecessor relationship changes that entry's cryptographic result and every dependent later state.

The Hash Chain therefore supports detection of certain forms of:

- replacement;
- insertion;
- deletion;
- reordering;
- predecessor substitution;
- history truncation when a trusted later boundary is available.

A Hash Chain is a historical integrity mechanism.

It is not an autonomous source of business truth.

---

# 8.2 Scope

This chapter applies to Hash Chains used to commit:

- Evidence Records;
- other Protocol Objects;
- object digests or typed commitments;
- batches or authenticated aggregate roots;
- correction, revocation, and migration evidence;
- identity and key history;
- Policy or Trust Profile state where governed;
- other accountability-relevant material defined by TAIP.

A deployment may operate one Chain or multiple Chains.

The applicable TAIP version, Trust Profile, namespace rules, and deployment policy determine:

- which objects require Commitment;
- which Chain receives them;
- whether ordering is global, partitioned, or scoped;
- whether Chain operator Signatures are required;
- whether Witnessing, Checkpointing, or External Anchoring is required;
- which proof and Preservation obligations apply.

This chapter does not require a public blockchain, distributed consensus network, or one universal global ledger.

---

# 8.3 Security Objective

The core security objective is to make unauthorized historical modification detectable relative to a valid, independently held, or otherwise trusted Chain state.

The relevant statement is not merely:

> An entry contains the digest of an earlier entry.

The stronger architectural statement is:

> A verifier can determine whether defined committed material occupies a defined position in a governed history that is continuous with a trusted historical boundary.

Achieving that objective requires more than hashing.

It also requires governed semantics for:

- Chain identity;
- entry type and version;
- canonical cryptographic input;
- predecessor state;
- ordering;
- operator identity and Authority where applicable;
- Commitment evidence;
- historical algorithms and keys;
- trusted comparison boundaries;
- conflict handling;
- Preservation.

---

# 8.4 What a Hash Chain Establishes

Subject to successful Verification, a Hash Chain may establish that:

- a defined entry is cryptographically linked to a defined predecessor state;
- a defined object commitment is bound into that entry;
- the entry advances a specific Chain;
- the resulting Chain Head follows from the governed transition;
- a presented range is continuous between defined boundaries;
- later trusted state depends cryptographically upon earlier committed state;
- incompatible successor states exist for the same predecessor, if both are available;
- an object was committed no later than a supported later observation, Checkpoint, or publication boundary.

The exact conclusion depends upon the available evidence and the Verification Context.

```text
Valid Link
      +
Valid Chain Identity
      +
Valid Historical Context
      +
Trusted Boundary
      =
Bounded Historical Conclusion
```

---

# 8.5 What a Hash Chain Does Not Establish

A valid Hash Chain does not by itself prove:

- that every event that should have been recorded was submitted;
- that every submitted object was accepted;
- that the committed assertion is true;
- that the Actor possessed valid Authority;
- that applicable Policy was correct or satisfied;
- that the Chain operator was independent;
- that no alternative history was shown to another observer;
- that all committed evidence remains available;
- that a producer-supplied Event Time is accurate;
- that a required Witness quorum was achieved;
- that a Trust Profile was achieved;
- Legal Validity or Regulatory Compliance.

```text
Chain Validity
≠
Evidence Completeness
≠
Business Truth
≠
Trust Profile Achievement
```

These limitations must remain visible in Verification results.

---

# 8.6 Historical State Model

The Hash Chain represents a sequence of historical state transitions.

For each governed transition, the new state depends upon:

- the Chain Identifier;
- the preceding Chain state;
- the protected Chain Entry content;
- the committed object or aggregate commitment;
- the applicable type and versions;
- domain separation and algorithm context;
- any other substitution-sensitive semantics required by TAIP.

Conceptually:

```text
New Chain State = Governed Transition(
    Chain Identifier,
    Previous Chain State,
    Protected Chain Entry,
    Algorithm and Version Context
)
```

This expression is illustrative.

TAIP defines the exact canonical construction and cryptographic inputs.

The historical state model must be deterministic so that independent conforming implementations derive equivalent results from equivalent inputs.

---

# 8.7 Chain Identifier

The **Chain Identifier (CID)** identifies a governed Hash Chain namespace.

CID rules must define:

- namespace;
- assignment responsibility;
- syntax;
- uniqueness expectations;
- comparison and normalization behavior;
- persistence;
- relationship to the Chain operator;
- relationship to deployment, tenant, domain, or partition context;
- collision and reuse handling;
- lifecycle and migration behavior.

```text
CID
≠
Chain Head
≠
Chain Entry Identifier
≠
Database Table Name
≠
Network Endpoint
```

A CID may remain stable across key rotation or infrastructure migration if governed continuity is preserved.

A CID must not be silently reused for an unrelated history.

---

# 8.8 Chain Namespace and Scope

Every Chain operates within a defined semantic scope.

Scope may be organized by:

- Organization;
- service or control domain;
- tenant;
- asset class;
- workflow family;
- jurisdiction;
- Trust Profile;
- privacy boundary;
- operational partition;
- another governed dimension.

The scope determines which ordering and Completeness claims are meaningful.

For example, a tenant-specific Chain can establish order within that tenant's Chain.

It cannot establish global order across all tenants unless an additional governed aggregation mechanism binds those Chains together.

Partitioning must not be hidden when it narrows an Accountability Claim.

---

# 8.9 Genesis State

Every Chain must begin from a governed **genesis state**.

The genesis definition must identify or bind:

- the CID;
- the Chain version;
- the genesis construction;
- the applicable algorithm suite;
- the initial operator or governance context where required;
- creation or activation evidence;
- any predecessor Chain or migration relationship;
- critical extensions.

The genesis state is not an ordinary missing predecessor.

It is an explicit starting boundary.

```text
Missing Predecessor
≠
Governed Genesis
```

A verifier must reject or report indeterminate a Chain whose alleged genesis cannot be distinguished from a truncated or unresolved history under the applicable rules.

---

# 8.10 Chain Entry

A **Chain Entry (CE)** is a Protocol Object that binds committed material to defined preceding Chain state.

Its logical semantics include:

- Protocol Object type and version;
- CID;
- Chain Entry identity where defined;
- predecessor Chain state;
- committed object or aggregate commitment;
- ordering information;
- Commitment Time with defined semantics;
- Chain operator or issuer identity where applicable;
- algorithm and canonicalization context;
- extensions;
- cryptographic protection.

A Chain Entry is distinct from:

- the Evidence Record it commits;
- the committed object's digest;
- the CID;
- the Chain Head;
- a Commitment Receipt;
- a Witness Observation;
- a Checkpoint;
- a database row.

---

# 8.11 Chain Entry Identifier

TAIP may define a stable identifier for each Chain Entry.

If defined, its rules must establish:

- namespace and generation responsibility;
- uniqueness and comparison behavior;
- relationship to CID;
- relationship to sequence or position;
- relationship to the Chain Entry digest;
- collision handling;
- persistence after migration or reserialization.

An entry identifier must not be assumed to be the entry digest unless the applicable version defines it as content-derived.

```text
Chain Entry Identifier
≠
Chain Entry Digest
≠
Chain Position
```

Different protected entries associated with the same identifier must be surfaced as a conflict.

---

# 8.12 Predecessor Reference

Every non-genesis Chain Entry must bind to exactly the predecessor state required by the applicable Chain construction.

The predecessor reference must be strong enough to prevent substitution of:

- another Chain;
- another Chain epoch;
- another entry at the same apparent position;
- an unprotected database pointer;
- an entry interpreted under incompatible rules.

The protected relationship must include the CID and any version or domain context needed to prevent cross-Chain reuse.

A predecessor locator may assist retrieval.

It must not replace the integrity-bound predecessor state.

---

# 8.13 Chain Head

The **Chain Head (CH)** is the cryptographic state representing a defined latest committed boundary of a Chain at a particular point in history.

A Chain Head may be:

- returned in a Commitment Receipt;
- observed by a Witness;
- included in a Checkpoint;
- placed into an External Anchor;
- stored by an independent verifier;
- used as the expected predecessor for a subsequent append;
- preserved for future comparison.

```text
Chain Head
≠
Claimed Current Database State
≠
Checkpoint
≠
Witness Observation
```

A Chain may have many historical Chain Heads.

The phrase **current Chain Head** is observer-relative unless supported by an authoritative or independently corroborated view at a defined time.

---

# 8.14 Resulting State

Acceptance of a Chain Entry produces a resulting Chain state according to the applicable version.

The resulting state must bind all semantics whose substitution could alter the historical claim, including at minimum:

- the Chain identity;
- the predecessor state;
- the protected entry commitment;
- applicable construction and algorithm versions.

The resulting Chain Head may equal a governed digest of the complete Chain Entry or may be derived through another TAIP-defined construction.

The construction must avoid self-reference ambiguity.

If the resulting Chain Head is carried inside a receipt or presentation object, it must be clear whether that value is:

- an input to cryptographic protection;
- a derived value;
- an operator assertion;
- independently verified output.

---

# 8.15 Committed Material

A Chain Entry may commit directly or indirectly to:

- one complete Protocol Object;
- a canonical object digest;
- a typed commitment to external content;
- a batch manifest;
- an authenticated aggregate root;
- a governed state-transition commitment;
- a migration or continuity object.

The commitment must identify its semantic type.

A bare digest is insufficient when the verifier cannot determine what was hashed, under which canonicalization, version, domain, or object type.

```text
Digest Bytes
      +
Type and Domain Context
      +
Canonicalization and Algorithm Context
      =
Interpretable Commitment
```

---

# 8.16 Object-to-History Binding

Object Integrity and Historical Integrity are distinct.

An Evidence Record digest may establish the protected identity of the record.

A Chain Entry binds that protected identity to a position in a governed history.

```text
Evidence Record
      │ canonical object digest
      ▼
Object Commitment
      │ included in protected Chain Entry
      ▼
Chain Entry
      │ linked to predecessor state
      ▼
Chain Head
```

Verification must establish both links when an Accountability Claim depends upon historical Commitment.

A valid Evidence Record without valid Chain binding is not thereby committed.

A valid Chain Entry whose committed object cannot be resolved may establish a bounded historical commitment while leaving semantic evaluation incomplete.

---

# 8.17 Canonical Cryptographic Input

Each Hash Chain version must define or reference deterministic canonical cryptographic input construction.

The definition must address:

- Chain Entry property coverage;
- CID binding;
- predecessor-state binding;
- committed-material binding;
- type and version binding;
- algorithm identifiers;
- absent, null, default, and unknown values;
- extension handling;
- character, number, and collection normalization;
- domain separation;
- limits on size and nesting.

The canonical Chain input is not necessarily the same as:

- a transport body;
- a database serialization;
- a user-interface presentation;
- an encrypted archive package;
- a debug representation.

Independent implementations must derive equivalent chain-state inputs from equivalent conforming entries.

---

# 8.18 Domain Separation

Hash Chain operations must use domain separation sufficient to prevent one cryptographic value from being reinterpreted as another protocol artifact.

Distinct domains may be required for:

- genesis construction;
- Chain Entry commitment;
- Chain Head derivation;
- object commitments;
- batch roots;
- inclusion proof nodes;
- Checkpoints;
- migration bridges;
- Signatures.

The same digest algorithm does not make two domains interchangeable.

```text
Object Digest
≠
Chain Entry Digest
≠
Chain Head
≠
Checkpoint Commitment
```

Domain identifiers affecting Verification must be governed and cryptographically bound.

---

# 8.19 Chain Operator

The **Chain operator** maintains the governed process that accepts eligible commitments and advances Chain state.

Its responsibilities may include:

- validating submitted material;
- selecting the applicable Chain;
- enforcing append preconditions;
- assigning governed ordering information;
- constructing Chain Entries;
- advancing Chain state atomically;
- producing Commitment Receipts;
- publishing or exposing Chain Heads;
- preserving Chain material;
- reporting conflicts and failures;
- supporting Witnessing, Checkpointing, export, and Verification.

The Chain operator may be the same Organization as an Evidence Registry or Commitment Service.

Role combination must remain visible because operational convenience does not create independence.

---

# 8.20 Operator Identity and Authority

Where a Chain Entry, Commitment Receipt, or Chain Head statement is attributed to an operator, the responsible Protocol Identity must be identifiable.

Verification may require:

- operator identity evidence;
- Key Identifier and Key Purpose;
- Historical Key State;
- Authority to operate the relevant CID;
- applicable governance or Registry state;
- Signature validation;
- compromise or revocation status.

Hash linkage alone does not authenticate the operator.

A valid operator Signature does not prove that the operator correctly applied admission Policy or recorded every required submission.

Identity, key control, operational Authority, and Chain correctness remain separate evaluation dimensions.

---

# 8.21 Submission, Acceptance, and Commitment

The following states must remain distinct:

## Submission

Material was sent to a Registry, Commitment Service, or Chain operator.

## Acceptance

The receiver accepted the material for defined processing.

## Commitment

The material was bound into protected Chain history under the applicable rules.

```text
Submitted
≠
Accepted
≠
Committed
```

A successful HTTP response, queue acknowledgment, database write, or locally assigned sequence number is not automatically Commitment evidence.

An interoperable Commitment claim requires a valid Chain Entry, Commitment Receipt, or other TAIP-defined proof sufficient for the claim.

---

# 8.22 Admission Validation

Before Commitment, the Chain operator must apply the validation required by the Chain version, object type, Policy, and Trust Profile.

Admission validation may include:

- parsing and structural validation;
- type and version support;
- object identifier and digest checks;
- Signature checks;
- reference validation;
- Policy or Authority prerequisites;
- duplicate and conflict detection;
- size and resource bounds;
- expected-predecessor checks;
- Chain scope and partition eligibility.

Admission success does not imply that every Accountability Claim about the object has been verified.

The applied admission scope and any deferred checks should be identifiable.

Objects that fail mandatory admission rules must not be represented as committed.

---

# 8.23 Atomic Append

Appending a Chain Entry must be atomic with respect to the governed predecessor state.

Conceptually, the operator performs:

```text
1. Read expected Chain Head
2. Validate proposed commitment
3. Construct entry bound to expected Chain Head
4. Compare current Chain Head with expected Chain Head
5. Commit entry and new Chain Head as one governed transition
```

If the current Chain Head changed before the append completed, the operator must not silently commit the entry against an unintended predecessor.

It may retry by constructing a new governed entry or may reject the attempt.

The retry behavior must preserve accountability-relevant attempt and ordering semantics.

---

# 8.24 Ordering Semantics

Every Chain must define the ordering relation established by its entries.

Possible semantics include:

- strict total order within one CID;
- total order within a partition;
- ordered batches with defined internal order;
- partial order across multiple Chains;
- aggregate order established only at Checkpoint boundaries.

A linear Chain establishes order of Commitment within its scope.

It does not automatically establish:

- real-world event order;
- causal order;
- order across unrelated Chains;
- wall-clock order when timestamps conflict;
- completeness of the events represented.

Ordering claims must state their scope.

---

# 8.25 Position and Sequence Information

A Chain Entry may carry a governed position, sequence number, epoch-relative index, or range coordinate.

Such information supports:

- gap detection;
- efficient retrieval;
- range Verification;
- operational diagnostics;
- Checkpoint coverage;
- duplicate detection.

Sequence information must be cryptographically bound where substitution could change interpretation.

A sequence number is not a timestamp.

A missing sequence value may indicate:

- omitted evidence;
- a reserved or failed position;
- an unavailable entry;
- a version-defined sparse sequence;
- a presentation error.

The applicable rules must distinguish these cases.

---

# 8.26 Concurrency

Concurrent submissions may target the same current Chain Head.

The Chain construction must resolve concurrency without producing an undisclosed fork.

Permitted approaches may include:

- serialization by one governed append service;
- compare-and-append with retry;
- deterministic ordering within an accepted batch;
- consensus across authorized operators;
- partitioning into separate CIDs followed by governed aggregation.

TAIP does not require one concurrency mechanism.

It requires the resulting ordering, conflicts, retries, and failures to remain explicit and verifiable.

Two successful commitments claiming the same exclusive position under incompatible states constitute a conflict.

---

# 8.27 Commitment Time

**Commitment Time** is the time associated with entry into protected historical state under defined semantics.

Its assurance depends upon:

- who asserted it;
- which clock supplied it;
- precision and synchronization assumptions;
- whether it is cryptographically bound;
- whether it is corroborated by Witnessing, Checkpointing, or external publication;
- the applicable Trust Profile.

```text
Event Time
≠
Submission Time
≠
Acceptance Time
≠
Commitment Time
≠
Observation Time
```

A Chain establishes ordering more directly than wall-clock accuracy.

An operator-supplied Commitment Time must not be represented as independently trusted time without supporting evidence.

---

# 8.28 Commitment Receipt

A **Commitment Receipt (CR)** is a Protocol Object or governed cryptographic artifact providing evidence that defined material was committed.

A Commitment Receipt may identify or bind to:

- the committed object or typed commitment;
- the CID;
- the Chain Entry;
- the predecessor state;
- the resulting Chain Head;
- ordering or position information;
- Commitment Time semantics;
- the Commitment Service or Chain operator;
- applicable versions and algorithms;
- proof material;
- operator Signature or other authentication.

A receipt must state what it proves.

It must not imply Witnessing, Checkpointing, Anchoring, Preservation, or Completeness unless those properties are separately supported.

---

# 8.29 Receipt Verification

Verification of a Commitment Receipt may require:

1. resolving the committed object or commitment;
2. validating the receipt type and version;
3. verifying CID and entry identity;
4. validating cryptographic binding to the predecessor and resulting state;
5. validating operator identity, Key Purpose, and Historical Key State;
6. confirming the applicable Chain construction;
7. evaluating Commitment Time semantics;
8. checking referenced Chain material or proof material;
9. identifying any missing required dependencies;
10. bounding the resulting conclusion.

A receipt presented without the required Chain material may still be authentic as an operator statement while remaining insufficient to prove continuity.

A verifier must distinguish these outcomes.

---

# 8.30 Inclusion Proofs

A Chain or authenticated batch construction may support a compact **inclusion proof** showing that defined committed material contributes to a particular Chain Head or aggregate root.

An inclusion proof must bind:

- the committed material;
- the applicable CID or aggregate namespace;
- the entry or batch position where relevant;
- the target Chain Head or root;
- proof type and version;
- algorithm context;
- ordering semantics required by the claim.

Proof verification must reject ambiguous or cross-domain reuse.

An inclusion proof establishes inclusion in the committed structure covered by the proof.

It does not establish semantic validity of the included object.

---

# 8.31 Completeness and Exclusion Claims

A Hash Chain naturally proves relationships among material that appears in the presented history.

It does not automatically prove that omitted material never existed or that every required event was committed.

An exclusion or Completeness claim may require additional evidence such as:

- a governed authenticated-set construction;
- a defined closed input population;
- signed submission and rejection records;
- sequence and gap-accounting rules;
- Witness Observations from independent control domains;
- Checkpoints covering defined ranges;
- reconciliation across producers and execution systems;
- Trust Profile-specific controls.

```text
Proof of Inclusion
≠
Proof of Non-Submission
≠
Proof of Complete History
```

A verifier must not infer absence merely because an object is missing from one presented Chain segment.

---

# 8.32 Chain Segments and Range Proofs

A verifier may evaluate a **Chain segment** bounded by two historical states rather than replaying the Chain from genesis.

A segment presentation may include:

- CID and Chain version;
- starting boundary;
- ending boundary;
- ordered Chain Entries or compact proof material;
- covered positions or range semantics;
- required algorithm and Registry state;
- relevant Checkpoints, Witness Observations, or anchors;
- unavailable or intentionally omitted material.

The starting boundary must be trusted or verified through earlier evidence.

The ending boundary must be derived from the segment or bound by valid later evidence.

A valid segment proves continuity only for its declared scope and construction.

It does not prove the validity of history preceding the starting boundary.

---

# 8.33 Missing and Unavailable Chain Material

Chain material may be missing because it was:

- never produced;
- rejected before Commitment;
- deleted under retention rules;
- lost through operational failure;
- withheld;
- redacted;
- encrypted without available keys;
- omitted from an export;
- referenced through an unavailable service;
- corrupted.

These states must not be collapsed into one generic not-found result where the distinction affects Verification.

A preserved later Chain Head can show that some earlier committed state existed without making the underlying entry or object available.

```text
Cryptographic Continuity Available
      +
Underlying Evidence Unavailable
      =
Limited, Not Successful, Semantic Verification
```

Verification must report which claims remain evaluable and which are indeterminate.

---

# 8.34 Forks

A **fork** exists when incompatible successor states claim continuity from the same predecessor under semantics that require one exclusive successor.

```text
                 ┌──► State n+1-A
State n ─────────┤
                 └──► State n+1-B
```

A fork may result from:

- operator error;
- non-atomic append behavior;
- failed replication or recovery;
- unauthorized state restoration;
- malicious history construction;
- a deliberately permitted branching model.

If branching is permitted, the branch identity and merge or selection semantics must be governed.

If the Chain claims strict linearity, incompatible successors are a conflict and must not be hidden through last-write-wins behavior.

---

# 8.35 Equivocation and Split Views

**Equivocation** occurs when an operator presents incompatible histories or Chain Head claims to different observers without disclosing the conflict.

A Hash Chain viewed by only one relying party cannot by itself prove that no split view exists.

Equivocation resistance may be strengthened through:

- Witness publication;
- Checkpoint comparison;
- External Anchoring;
- cross-Organization gossip or reconciliation;
- independent retention of Chain Heads;
- multi-party operation;
- transparent conflict reporting.

The relevant assurance depends upon control-domain separation, observation timing, coverage, and availability.

Multiple replicas controlled by one operator may improve resilience without providing independent split-view detection.

---

# 8.36 Independent Retention of Chain State

An observer may retain a Chain Head, Commitment Receipt, Witness Observation, Checkpoint, or other protected boundary for later comparison.

Independent retention can make subsequent rollback or inconsistent-history presentation detectable relative to that retained boundary.

The retained material should identify:

- the CID;
- the exact Chain state;
- the observation or receipt semantics;
- applicable versions and algorithms;
- acquisition time and source;
- observer identity where relevant;
- integrity and Preservation information.

A cached value without provenance may be useful operationally but provide weaker accountability evidence.

Retention of a Chain Head does not preserve the underlying evidence by itself.

---

# 8.37 Witnesses, Checkpoints, and External Anchors

Hash Chains compose with later assurance layers.

| Layer | Contribution | Does not establish by itself |
|---|---|---|
| Hash Chain | Ordered continuity within a CID | No split view or completeness |
| Witness Observation | Observation by an eligible Witness | Truth of underlying event |
| Checkpoint | Stable commitment to a defined historical boundary | Availability of all covered evidence |
| External Anchor | Commitment across another trust boundary | Meaning of committed content |

A Witness may observe a Chain Entry, Chain Head, Commitment Receipt, or Checkpoint candidate.

A Checkpoint may bind one or more Chain Heads or entry ranges.

An External Anchor may publish a Checkpoint or another governed Chain commitment.

These artifacts must remain separately verifiable rather than being represented as attributes that appear automatically when a Chain Entry is created.

---

# 8.38 Batching and Aggregate Commitments

A Chain Entry may commit a batch of objects through a governed manifest or authenticated aggregate.

Batch semantics must define:

- batch identity;
- member identity and commitment rules;
- member ordering, if meaningful;
- duplicate handling;
- aggregate construction;
- inclusion proof format;
- partial rejection behavior;
- maximum size and cardinality;
- privacy characteristics;
- Preservation obligations.

A batch root is not sufficient unless the verifier can determine which objects it covers and validate membership under the applicable construction.

If internal ordering matters to an Accountability Claim, an unordered set commitment is insufficient.

---

# 8.39 Batch Atomicity and Partial Failure

The applicable batch type must state whether Commitment is:

- atomic for the complete batch;
- independent for each member;
- atomic for defined subgroups;
- represented through a manifest containing explicit member outcomes.

The system must not return one undifferentiated success result when some members were rejected, conflicted, or omitted.

```text
Batch Accepted
      │
      ├── Member A committed
      ├── Member B rejected
      ├── Member C duplicate
      └── Member D indeterminate
```

Member-level Commitment claims require member-level evidence or a valid inclusion path into an atomically committed manifest.

---

# 8.40 Segments and Epochs

A long-lived Chain may be divided into governed segments or epochs for scalability, retention, algorithm transition, or operational administration.

An epoch boundary must preserve continuity through a protected relationship between:

- the prior epoch's final state;
- the new epoch identity or number;
- the new epoch's genesis state;
- applicable version and algorithm changes;
- operator or governance changes;
- boundary time semantics;
- supporting migration or Checkpoint evidence.

An epoch is not a new unrelated Chain merely because storage was rotated.

Conversely, a new unrelated history must not inherit an old CID or continuity claim without a governed bridge.

---

# 8.41 Rollover and Chain Closure

A Chain may be closed or rolled over because of:

- planned operational rotation;
- key or algorithm transition;
- tenant or organizational migration;
- compromise response;
- policy change;
- end of service;
- archival transition.

Closure evidence should identify:

- the final accepted Chain Head;
- the final covered position or range;
- closure reason and status;
- operator identity;
- closure time semantics;
- outstanding gaps or unresolved conflicts;
- successor Chain, if any;
- applicable Checkpoint or anchor.

A closed Chain remains part of historical Verification.

Rollover must not erase the predecessor history or imply that uncommitted pending submissions were committed.

---

# 8.42 Multiple Chains, Sharding, and Aggregation

Deployments may use multiple Chains for throughput, isolation, privacy, or governance.

Sharding rules must define:

- how an object is assigned to a CID;
- whether assignment is deterministic or Policy-driven;
- whether an object may be committed to more than one Chain;
- the ordering relation within and across shards;
- how shard movement is represented;
- how aggregate Checkpoints bind shard heads;
- how omitted or unavailable shards affect Verification.

```text
Chain A Head ─┐
Chain B Head ─┼──► Aggregate Checkpoint
Chain C Head ─┘
```

An aggregate boundary can support cross-Chain historical comparison.

It does not retroactively create a total order among shard entries unless the construction explicitly defines one.

---

# 8.43 Duplicate, Replay, and Idempotency

Distributed delivery may present the same commitment request more than once.

The following cases must remain distinguishable:

- retransmission of the same canonical object and commitment request;
- duplicate request after successful Commitment;
- reuse of an idempotency key for different protected content;
- reuse of an object identifier with conflicting content;
- intentional recommitment of the same object to another Chain;
- replay outside the permitted context or validity interval;
- a distinct retry or attempt that matters to causality.

Idempotent handling must not silently create a second exclusive Chain position for the same accepted request.

Nor may deduplication erase a distinct attempt whose timing, context, or outcome is accountability-relevant.

---

# 8.44 Correction, Supersession, and Revocation

Committed history is not rewritten when a committed assertion is later found to be wrong, obsolete, revoked, or disputed.

The system appends additional accountable evidence.

```text
Original Evidence Record
        │ committed in Entry n
        ▼
Correction or Revocation Record
        │ committed in Entry n+k
        ▼
Historical interpretation includes both
```

The later object must identify the earlier object and the governed relationship.

The original Chain Entry and its position remain valid historical facts even if the semantic effect of the original object changes.

Verification must distinguish:

- integrity of the original Commitment;
- current semantic status;
- effective time of correction or revocation;
- whether the later relationship itself was valid and committed.

---

# 8.45 Retention, Deletion, and Tombstone Evidence

Legal, privacy, security, or operational rules may require deletion of underlying content.

Deletion does not permit silent rewrite of committed Chain state.

Depending upon applicable rules, the system may preserve:

- the original cryptographic commitment;
- a deletion or disposition record;
- reason and Authority for deletion;
- affected object and Chain references;
- deletion time semantics;
- evidence of destroyed encryption keys;
- remaining Verification limitations.

A retained commitment may still reveal information through correlation or guessing.

Deletion design must therefore consider both data availability and commitment privacy.

```text
Content Deleted
≠
Commitment Never Existed
≠
History Rewritten
```

---

# 8.46 Privacy and Confidentiality

Hash Chains can create durable correlation across identities, workflows, tenants, assets, and time.

Privacy design should minimize unnecessary exposure of:

- direct identifiers;
- low-entropy values;
- business amounts and asset identifiers;
- workflow correlation identifiers;
- tenant topology;
- operational volume and timing patterns;
- Policy or Authority details;
- sensitive entry classifications.

Hashing sensitive data does not necessarily anonymize it.

Low-entropy or guessable values may be recoverable through dictionary attacks.

Chain partitioning, typed blinded commitments, encryption, selective disclosure, access control, and retention policy may reduce exposure when governed carefully.

Privacy controls must not make mandatory Verification dependencies silently unavailable.

---

# 8.47 Encryption and Blinded Commitments

Committed material may be encrypted or represented through a blinded commitment.

The applicable construction must define:

- what is encrypted or blinded;
- which commitment remains publicly verifiable;
- how disclosure or opening is performed;
- algorithm and parameter identification;
- key custody and recovery requirements;
- whether commitment equality creates linkability;
- how redaction and selective disclosure are represented;
- what happens when keys or opening material are unavailable.

Encryption does not replace integrity binding.

Loss of decryption or opening material may leave Chain continuity verifiable while making object-level claims indeterminate.

A presentation of decrypted content must be cryptographically related to the committed representation.

---

# 8.48 Versioning and Algorithm Agility

Hash Chain interpretation may depend upon:

- TAIP version;
- Chain version;
- Chain Entry version;
- canonicalization version;
- hash or commitment algorithm;
- Signature suite;
- batch or proof construction;
- extension versions.

Version and algorithm identifiers must be bound wherever substitution could change the result or conclusion.

An implementation must not silently verify historical entries using current default algorithms.

Algorithm transition may use:

- a governed epoch boundary;
- dual commitments during a transition interval;
- a signed migration record;
- a Checkpoint binding old and new constructions;
- another TAIP-defined bridge.

Transition evidence must preserve the last valid old state and the first valid new state.

---

# 8.49 Operator Key Rotation and Compromise

Operator signing keys may rotate without changing CID when Chain governance permits continuity.

Historical Verification must use the key state applicable when the relevant entry, receipt, closure, or migration statement was signed.

The Chain must not rely on the current operator key to reinterpret all earlier operator statements.

Compromise response may require:

- key revocation evidence;
- last-known-good Chain Head;
- independent Witness or Checkpoint comparison;
- fork and equivocation analysis;
- emergency Chain closure;
- successor Chain creation;
- migration evidence;
- explicit uncertainty for the affected interval.

Key revocation does not automatically invalidate every historical Signature made before compromise.

The applicable Historical Key State and compromise semantics determine the result.

---

# 8.50 Extensions

Hash Chain and Chain Entry extensions must follow the common extension model defined in Chapter 6.

Extensions may add:

- domain-specific ordering data;
- alternate proof constructions;
- additional operator attestations;
- privacy-preserving commitment methods;
- partition or epoch metadata;
- external consensus evidence;
- specialized migration information.

Critical extensions affecting Chain interpretation must be cryptographically bound and understood by the verifier.

An unknown critical extension must produce an unsupported or indeterminate outcome for affected claims.

An unknown non-critical extension may be preserved without affecting defined base semantics only when the applicable rules allow it.

---

# 8.51 Validation Layers

Hash Chain evaluation is layered.

## Availability and Parsing

Can the Chain Entries, proofs, and required dependencies be obtained and parsed safely?

## Structural Validation

Do the objects conform to the applicable types, versions, and schemas?

## Semantic Validation

Are CID, entry type, position, predecessor, commitment, time, and extension combinations permitted?

## Canonicalization and Digest Validation

Can the protected input and cryptographic values be reproduced deterministically?

## Link Validation

Does each entry bind to the required predecessor and produce the expected resulting state?

## Object Binding Validation

Does each committed object or aggregate match the commitment carried by the entry?

## Operator Validation

Are required operator identity, Signature, Key Purpose, Historical Key State, and Authority claims valid?

## Range and Boundary Validation

Is the presented history continuous between valid starting and ending boundaries?

## Conflict Validation

Are forks, duplicate positions, incompatible heads, gaps, and split-view evidence visible and evaluated?

## Profile and Completeness Validation

Are required Witness, Checkpoint, anchor, Preservation, and coverage controls satisfied?

Successful validation at one layer does not imply success at later layers.

---

# 8.52 Verification Procedure

An independent verifier evaluating a historical Commitment should perform the checks required by the claim and Trust Profile.

A representative procedure is:

1. identify the Accountability Claim and Verification Context;
2. identify the CID, Chain version, and relevant boundaries;
3. resolve required Chain Entries, objects, receipts, and dependencies;
4. validate types, versions, canonicalization, and algorithms;
5. validate the committed object or aggregate commitment;
6. validate predecessor linkage and resulting Chain states;
7. validate ordering, position, and range semantics;
8. validate operator identity, Signatures, keys, and Authority where required;
9. evaluate Commitment Time within its assurance limits;
10. detect gaps, conflicts, forks, rollback, and incompatible heads;
11. validate Witnesses, Checkpoints, anchors, and Preservation evidence required by the Trust Profile;
12. evaluate Completeness separately from Chain validity;
13. produce bounded outcomes and explicit limitations.

The procedure may begin from a trusted Checkpoint rather than genesis when the applicable profile permits it.

---

# 8.53 Verification Outcomes

Hash Chain Verification should distinguish at least:

- valid;
- invalid;
- indeterminate;
- incomplete;
- unsupported;
- conflicting;
- unavailable;
- valid with limitations.

Relevant sub-results may include:

- object commitment valid;
- Chain link valid;
- range continuity valid;
- operator Signature valid;
- operator Authority unresolved;
- Commitment Time operator-asserted only;
- required Checkpoint missing;
- fork detected;
- completeness not established;
- underlying object unavailable.

A single boolean is insufficient when it would hide the difference between cryptographic validity and missing assurance evidence.

---

# 8.54 Portability and Export

A portable Chain export should contain or reference enough governed material to reproduce the intended Verification conclusions outside the originating platform.

Depending upon scope, it may include:

- CID and Chain metadata;
- genesis or trusted starting boundary;
- ordered Chain Entries;
- committed Protocol Objects or typed commitments;
- Commitment Receipts;
- inclusion or range proofs;
- historical schemas and registries;
- algorithm and canonicalization definitions;
- operator identity and Historical Key State;
- Witness Observations and Checkpoints;
- External Anchor Evidence;
- missing-material and limitation statements.

Export order and file layout are not automatically normative Chain order.

The protected Chain relationships remain authoritative.

---

# 8.55 Preservation and Recovery

Hash Chain Preservation must address both cryptographic state and interpretation dependencies.

Relevant material includes:

- Chain Entries and historical Chain Heads;
- committed objects or governed commitments;
- receipts and proof material;
- schemas and canonicalization rules;
- algorithm identifiers and historical validation rules;
- operator identity and key history;
- Witness, Checkpoint, and anchor evidence;
- migration and closure records;
- known conflicts and gaps.

Backup replication alone does not prove Preservation or independence.

Recovery must not replace the last externally supported Chain Head with an older state without disclosure.

After recovery, the operator should reconcile recovered state against independently retained boundaries and surface rollback, gaps, or forks.

---

# 8.56 Migration

Migration may move Chain operation, storage, algorithms, governance, or namespace across systems.

A governed migration must preserve:

- source CID and final supported state;
- destination CID or continued CID semantics;
- migration reason and Authority;
- source and destination operator identities;
- historical version and algorithm context;
- continuity or explicit discontinuity;
- unresolved conflicts or unavailable ranges;
- effective boundary;
- supporting Signatures, Checkpoints, or anchors.

```text
Source Chain Final Head
          │
          ▼
     Migration Record
          │
          ▼
Destination Genesis / Continued Epoch
```

Copying database rows is not sufficient evidence of Chain migration.

If continuity cannot be established, the destination must begin as a new history and the limitation must be explicit.

---

# 8.57 Implementation Mapping

The logical Hash Chain model may be implemented using:

- an append-only ledger service;
- a relational database with atomic compare-and-append controls;
- an immutable object store plus protected index;
- a transparency-log construction;
- a replicated state machine;
- a permissioned or public ledger binding;
- a hybrid service with periodic external anchoring.

Implementation technology does not change the architectural requirements.

A database row marked immutable is not sufficient unless the governed cryptographic relationship can be verified.

A public ledger transaction is not sufficient unless it can be related unambiguously to the TrustAgentAI commitment.

A distributed deployment is not automatically independent.

---

# 8.58 Illustrative Logical Construction

The following construction is illustrative and non-normative.

```text
ObjectCommitment_i = Hash(
    DOMAIN_OBJECT,
    ObjectType,
    ObjectVersion,
    CanonicalObject_i
)

EntryCommitment_i = Hash(
    DOMAIN_ENTRY,
    ChainIdentifier,
    ChainVersion,
    Position_i,
    PreviousChainHead,
    ObjectCommitment_i,
    CommitmentContext_i
)

ChainHead_i = Hash(
    DOMAIN_HEAD,
    ChainIdentifier,
    ChainVersion,
    EntryCommitment_i
)
```

An implementation must use the construction defined by the applicable TAIP and cryptographic profile rather than this illustration.

The example demonstrates four required ideas:

1. object and Chain domains are separated;
2. CID and version are bound;
3. the predecessor state is bound;
4. the resulting state depends upon the protected entry.

---

# 8.59 Illustrative Commitment Sequence

```text
Evidence Producer          Commitment Service            Independent Verifier
        │                           │                              │
        │ finalized Evidence Record │                              │
        │──────────────────────────►│                              │
        │                           │ validate and accept          │
        │                           │ bind to current Chain Head   │
        │                           │ atomically append entry      │
        │                           │                              │
        │ Commitment Receipt        │                              │
        │◄──────────────────────────│                              │
        │                           │                              │
        │ receipt + evidence + Chain material                     │
        │─────────────────────────────────────────────────────────►│
        │                           │                              │
        │                           │                 verify object binding
        │                           │                 verify Chain continuity
        │                           │                 report bounded outcome
```

The receipt supports a Commitment claim only to the extent defined by its protected content and available dependencies.

Later Witnessing, Checkpointing, anchoring, and Preservation may strengthen the historical conclusion.

---

# 8.60 Common Anti-Patterns and Relationship to Other Specifications

## Hashing Without Governance

Storing a previous digest without defining Chain identity, versions, canonicalization, domains, or validation rules.

## Submission Equals Commitment

Treating a transport acknowledgment or queued request as protected historical Commitment.

## Timestamp Equals Order

Using producer or operator timestamps as the sole proof of Chain order.

## Database Sequence Equals Chain Proof

Treating an auto-increment column as cryptographic historical integrity.

## Mutable Committed Row

Updating protected entry or object content in place after Commitment.

## Silent Fork Selection

Keeping one incompatible successor and deleting the other without conflict evidence.

## Head Without Provenance

Presenting a Chain Head without CID, version, acquisition source, or supporting evidence.

## Digest as Completeness

Claiming that valid links prove every required event was recorded.

## Replica as Witness

Treating multiple instances under one control domain as independent observation.

## Unordered Batch as Ordered History

Using a set commitment where member order is required by the claim.

## Current-Key Verification

Using the operator's current key state for every historical receipt or entry.

## Rollover by Reset

Starting a new database or digest sequence without protected closure and continuity evidence.

## Hashing Sensitive Plaintext

Assuming a digest of low-entropy sensitive data prevents disclosure or correlation.

## Proof Without Availability

Reporting full semantic success when only a commitment remains and the underlying object is unavailable.

This chapter defines Hash Chain, Chain Entry, Chain Head, and Commitment Receipt semantics.

Other chapters and TAIP define related concerns, including:

- Witness Observations and independence;
- Checkpoints and External Anchors;
- identity, operator governance, and Key Transparency;
- evidence lifecycle state transitions;
- Preservation Evidence;
- Verification Engines and Reports;
- Dispute Pack construction;
- Trust Profiles and conformance;
- concrete schemas, algorithms, APIs, registries, and test vectors.

Those specifications may strengthen Chain-related assurance.

They must preserve the Hash Chain invariants defined here.

---

# Hash Chain Invariants

### INV-CHAIN-001 — Governed Chain Identity

Every interoperable Hash Chain MUST possess a governed CID whose scope and comparison semantics are explicit.

### INV-CHAIN-002 — Explicit Genesis

Every Chain MUST begin from a governed genesis or continuity boundary distinguishable from a missing predecessor.

### INV-CHAIN-003 — Deterministic Transition

Equivalent conforming Chain Entry inputs under the same governed context MUST produce equivalent cryptographic Chain state.

### INV-CHAIN-004 — Predecessor Binding

Every non-genesis Chain Entry MUST cryptographically bind to the predecessor state required by the applicable Chain construction.

### INV-CHAIN-005 — Object-to-History Binding

A Commitment claim MUST preserve a verifiable relationship between the committed material, its Chain Entry, and the resulting Chain state.

### INV-CHAIN-006 — Domain Separation

Object digests, Chain Entry commitments, Chain Heads, batch roots, Checkpoints, and other cryptographic domains MUST remain distinguishable.

### INV-CHAIN-007 — Ordering Scope

The ordering relation established by a Chain MUST be explicit and MUST NOT be represented as broader than its CID, partition, epoch, or aggregate scope.

### INV-CHAIN-008 — Lifecycle Separation

Submission, Acceptance, Commitment, Witnessing, Checkpointing, Anchoring, Preservation, and Verification MUST NOT be conflated.

### INV-CHAIN-009 — Atomic Append

A successful append MUST advance Chain state atomically from the predecessor state against which the accepted Chain Entry was constructed.

### INV-CHAIN-010 — Committed Immutability

Committed Chain Entries, committed material, and protected predecessor relationships MUST NOT be silently rewritten.

### INV-CHAIN-011 — Append-Only Correction

Correction, supersession, revocation, reversal, dispute, and deletion effects on committed evidence MUST be represented through additional accountable state.

### INV-CHAIN-012 — Conflict Visibility

Forks, incompatible successors, duplicate exclusive positions, identifier conflicts, and contradictory Chain Heads MUST remain visible to Verification.

### INV-CHAIN-013 — No Implied Completeness

Chain continuity or inclusion MUST NOT be represented as proof that every required event was submitted or committed.

### INV-CHAIN-014 — No Implied Truth

Valid Chain state MUST NOT be represented as proof that committed assertions are true, authorized, lawful, or independently corroborated.

### INV-CHAIN-015 — Time Separation

Event Time, Submission Time, Acceptance Time, Commitment Time, Observation Time, Checkpoint Time, and Publication Time MUST remain distinguishable.

### INV-CHAIN-016 — Historical Interpretation

Chain Verification MUST use the versions, algorithms, schemas, operator keys, and governance state applicable to the evaluated historical boundary.

### INV-CHAIN-017 — Operator/Integrity Separation

Hash linkage, operator authentication, operator Authority, and correct admission behavior MUST remain separate evaluation dimensions.

### INV-CHAIN-018 — Replication/Independence Separation

Replication within one control domain MUST NOT be represented as independent Witnessing or independent split-view protection.

### INV-CHAIN-019 — Validity/Availability Separation

Cryptographic Chain validity MUST remain distinguishable from availability of committed objects and interpretation dependencies.

### INV-CHAIN-020 — Intended/Achieved Separation

Chain configuration or an Intended Trust Profile MUST NOT be represented as proof that required Witness, Checkpoint, anchor, or Preservation controls were achieved.

### INV-CHAIN-021 — Explicit Partitioning

Sharding, segmentation, epochs, and aggregate boundaries MUST NOT conceal a narrower ordering or coverage scope.

### INV-CHAIN-022 — Governed Transition

Rollover, algorithm migration, operator migration, and successor-Chain creation MUST preserve explicit continuity or explicit discontinuity.

### INV-CHAIN-023 — Proof Scope

Inclusion, range, continuity, and exclusion proofs MUST be interpreted only according to their governed construction and declared target state.

### INV-CHAIN-024 — Explicit Uncertainty

Missing, unavailable, redacted, encrypted, malformed, unsupported, conflicting, and unverifiable Chain states MUST remain distinguishable where they affect conclusions.

### INV-CHAIN-025 — Representation Independence

Normative Hash Chain meaning MUST NOT depend upon one database, storage engine, transport, vendor, or user interface.

### INV-CHAIN-026 — Privacy-Preserving Commitment

Hash Chain designs SHOULD minimize unnecessary disclosure and linkability while preserving mandatory accountability semantics.

### INV-CHAIN-027 — Bounded Verification

Successful Chain Verification MUST NOT be represented as evidence Completeness, business truth, Legal Validity, or Regulatory Compliance beyond the evaluated claims.

### INV-CHAIN-028 — Reproducible Conclusion

Independent conforming implementations SHOULD derive equivalent Chain-level conclusions from equivalent evidence under equivalent Verification Contexts.

---

# Architectural Requirements

### REQ-CHAIN-001

TAIP MUST define or reference governed Hash Chain, Chain Entry, Chain Head, and Commitment Receipt semantics.

### REQ-CHAIN-002

TAIP MUST define CID syntax, namespace, assignment responsibility, comparison behavior, scope, persistence, collision handling, and reuse rules.

### REQ-CHAIN-003

Each Chain version MUST define a governed genesis construction and the evidence required to recognize a valid genesis or predecessor-Chain boundary.

### REQ-CHAIN-004

Each Chain Entry version MUST define its semantic purpose, required protected properties, permitted committed-material types, extension behavior, and validation rules.

### REQ-CHAIN-005

Every non-genesis Chain Entry MUST bind the applicable CID, predecessor state, committed material, type, and version context into deterministic cryptographic input.

### REQ-CHAIN-006

TAIP MUST define or reference canonicalization, domain separation, algorithm identification, and cryptographic input construction sufficient for independent reproduction of Chain state.

### REQ-CHAIN-007

Implementations MUST NOT infer Chain Entry digest, Chain Head, identifier, locator, position, or timestamp semantics from value appearance unless the applicable version defines the relationship.

### REQ-CHAIN-008

The protected Chain construction MUST prevent undetected cross-Chain, cross-version, cross-epoch, and cross-domain substitution under the applicable threat model.

### REQ-CHAIN-009

Chain operators MUST enforce atomic append behavior or a governed alternative that prevents undisclosed incompatible success from one exclusive predecessor state.

### REQ-CHAIN-010

Concurrency handling MUST preserve the resulting order, retries, rejections, conflicts, and any permitted branch semantics required for later Verification.

### REQ-CHAIN-011

Sequence, position, range, partition, and epoch values affecting historical interpretation MUST be cryptographically bound.

### REQ-CHAIN-012

Implementations MUST report the ordering scope established by a Chain and MUST NOT silently infer order across unrelated CIDs or partitions.

### REQ-CHAIN-013

Commitment Time values MUST identify their semantics, source, precision, and supporting assurance and MUST NOT be treated as equivalent to Event Time or independent observation.

### REQ-CHAIN-014

Admission behavior MUST identify the validation scope applied before Commitment and MUST NOT represent an object that failed mandatory admission rules as committed.

### REQ-CHAIN-015

An interoperable Commitment claim MUST be supported by a valid Chain Entry, Commitment Receipt, or other TAIP-defined evidence rather than transport or storage success alone.

### REQ-CHAIN-016

Commitment Receipts MUST identify or bind the committed material, CID, Chain state, applicable versions, commitment semantics, and responsible issuer required by their claim.

### REQ-CHAIN-017

Receipt Verification MUST report operator-statement validity separately from independently verified Chain continuity when the available evidence supports only one of those conclusions.

### REQ-CHAIN-018

Inclusion and range proof formats MUST bind their proof type, algorithm context, target Chain state, committed material, and ordering semantics required by the claim.

### REQ-CHAIN-019

Implementations MUST NOT infer non-submission, exclusion, or evidence Completeness from absence in a presented Chain segment unless a governed proof and coverage model supports that conclusion.

### REQ-CHAIN-020

Missing, unavailable, withheld, redacted, encrypted, corrupted, rejected, and never-produced Chain material MUST affect Verification explicitly where the distinction is known and relevant.

### REQ-CHAIN-021

Strictly linear Chains MUST detect and surface incompatible successor states, duplicate exclusive positions, and conflicting Chain Heads.

### REQ-CHAIN-022

Permitted branching Chains MUST define branch identity, ordering, selection, merge, closure, and Verification semantics.

### REQ-CHAIN-023

Trust Profiles claiming equivocation resistance MUST define the independent observation, comparison, publication, or consensus evidence required to support that claim.

### REQ-CHAIN-024

Batch commitments MUST define membership, ordering, duplicate handling, aggregate construction, proof behavior, atomicity, partial failure, and member-level outcome semantics.

### REQ-CHAIN-025

Batch processing MUST NOT conceal record-level rejection, conflict, omission, duplication, or indeterminate status behind an aggregate success result.

### REQ-CHAIN-026

Segmentation and epoch transitions MUST bind the preceding final state to the successor genesis or explicitly state discontinuity.

### REQ-CHAIN-027

Chain closure and rollover evidence SHOULD identify the final supported state, reason, operator, boundary time, outstanding gaps, and successor relationship.

### REQ-CHAIN-028

Sharded deployments MUST define object-to-Chain assignment, per-shard order, cross-shard claims, movement, aggregate coverage, and missing-shard behavior.

### REQ-CHAIN-029

Duplicate and idempotent processing MUST distinguish retransmission of equivalent protected material from identifier conflict, replay, recommitment, and a distinct accountability-relevant attempt.

### REQ-CHAIN-030

Committed Chain state MUST NOT be altered in place to implement correction, supersession, revocation, reversal, dispute, retention, or deletion.

### REQ-CHAIN-031

Deletion or disposition of committed content MUST preserve the accountable historical effect and resulting Verification limitations required by applicable rules.

### REQ-CHAIN-032

Hash Chain designs SHOULD mitigate dictionary attack, correlation, traffic analysis, and excessive linkability risks created by durable commitments.

### REQ-CHAIN-033

Encrypted or blinded commitments MUST define disclosure, opening, key-loss, algorithm, linkability, and Verification-failure behavior.

### REQ-CHAIN-034

Historical Chain Verification MUST use applicable historical algorithm, canonicalization, schema, Registry, operator key, and governance state rather than silently substituting current defaults.

### REQ-CHAIN-035

Unknown critical Chain extensions MUST produce an unsupported or indeterminate outcome for affected claims.

### REQ-CHAIN-036

Implementations MUST bound Chain Entry size, batch cardinality, proof size, parsing effort, range traversal, dependency resolution, and cryptographic resource consumption.

### REQ-CHAIN-037

Portable Chain exports SHOULD include or reference the boundaries, entries, committed objects, proof material, historical dependencies, conflicts, gaps, and limitations required to reproduce their intended conclusions.

### REQ-CHAIN-038

Preservation and recovery procedures MUST protect against undisclosed rollback and SHOULD reconcile restored state with independently retained Chain Heads, Witness Observations, Checkpoints, or anchors.

### REQ-CHAIN-039

Chain migration MUST identify source and destination state, continuity or discontinuity, operator and governance context, historical algorithms, effective boundary, and unresolved limitations.

### REQ-CHAIN-040

Hash Chain validation SHOULD distinguish availability, structural, semantic, canonicalization, cryptographic, link, object-binding, operator, range, conflict, Completeness, and Trust Profile results.

### REQ-CHAIN-041

Verification Reports MUST bound Chain conclusions to the evaluated CID, range, versions, evidence set, Verification Time, Trust Profile, and unresolved limitations.

### REQ-CHAIN-042

Independent conforming implementations SHOULD derive equivalent Chain Heads, proof results, and bounded Chain conclusions from equivalent conforming inputs and Verification Contexts.

---

# Security Considerations

The Hash Chain protects historical integrity but is itself exposed to protocol, cryptographic, operational, and governance attacks.

Major threats include:

- substituting another CID, epoch, version, or cryptographic domain;
- modifying committed object content while retaining a locator or identifier;
- constructing an entry against an unintended predecessor;
- racing concurrent appends to create incompatible successful histories;
- rolling back the current view to an older valid Chain Head;
- presenting split views to different observers;
- deleting one side of a fork;
- omitting rejected, failed, or competing submissions;
- treating a sequence number or timestamp as cryptographic proof;
- forging or misusing an operator signing key;
- validating historical statements using current key state;
- exploiting algorithm downgrade or ambiguous canonicalization;
- presenting an inclusion proof under the wrong root or namespace;
- hiding batch member failure;
- resetting state during rollover or disaster recovery;
- migrating without binding source and destination histories;
- exhausting verifiers with unbounded ranges, batches, proofs, or references;
- using low-entropy commitments that disclose sensitive values;
- withholding underlying objects while overstating semantic Verification;
- claiming independence from replicas within one control domain.

Implementations should apply controls including:

- strict CID, type, version, and domain binding;
- deterministic canonicalization and governed algorithms;
- atomic compare-and-append or equivalent protected ordering;
- conflict-preserving fork detection;
- operator identity, Key Purpose, and Historical Key State validation;
- independent retention and comparison of Chain Heads;
- Witnessing, Checkpointing, and anchoring appropriate to the Trust Profile;
- bounded parsing, range evaluation, and proof validation;
- explicit batch and partial-failure outcomes;
- append-only correction and deletion evidence;
- rollback-aware backup and recovery;
- governed rollover, algorithm transition, and migration;
- privacy-preserving commitments and access controls;
- durable Preservation of Chain material and interpretation dependencies;
- layered, reproducible Verification.

A malicious operator that controls the only available view may create a self-consistent alternative history.

Hash linkage alone cannot reveal that alternative if no verifier possesses or can obtain an incompatible trusted boundary.

This is why Witnesses, Checkpoints, External Anchors, cross-domain comparison, and independent Preservation matter.

---

# Privacy Considerations

Append-only history can conflict with privacy objectives because identifiers, timing, volume, and durable commitments may remain linkable after content is hidden or deleted.

Privacy analysis should consider:

- whether each committed value is necessary for a defined Accountability Claim;
- whether a typed commitment can replace direct disclosure;
- whether the committed value has sufficient entropy;
- whether tenant or workflow identifiers permit unwanted correlation;
- whether a global Chain exposes operational relationships better kept partitioned;
- whether batch membership reveals sensitive association;
- whether public Witness or anchor publication exposes volume or timing;
- whether encryption keys and blinded-opening material can be governed long term;
- whether deletion duties require disposition evidence;
- whether exported Chain segments expose unrelated entries.

Data minimization should begin before Commitment because committed protected state cannot later be silently rewritten.

Selective disclosure should reveal only the material required for the verifier's claim while retaining a valid relationship to the committed state.

Privacy limitations caused by missing or withheld evidence must be reported honestly rather than converted into a successful complete result.

---

# Design Rationale

TrustAgentAI uses Hash Chains because Object Integrity alone does not establish historical integrity.

A valid Evidence Record Signature can show that defined content was protected by a key.

It does not show when the record entered a governed history, what state preceded it, whether the record was later replaced, or which historical boundary depends upon it.

The Chain Entry supplies the Object-to-History binding.

The predecessor relationship makes alteration of earlier protected state propagate into later Chain Heads.

The CID prevents an entry from becoming meaningful only through a database location or service endpoint.

Explicit ordering prevents Event Time and wall-clock claims from substituting for Commitment order.

Atomic append prevents ordinary concurrency from becoming an undisclosed fork.

Commitment Receipts make the transition portable without pretending that a receipt automatically proves continuity, independence, or Completeness.

Witnesses, Checkpoints, and External Anchors strengthen the Chain by distributing or crossing trust boundaries, while Preservation keeps the evidence and interpretation dependencies available.

Append-only correction preserves the difference between what was committed earlier and what became known later.

Governed rollover and migration let the system evolve without resetting accountability history.

The stable objective is:

> **A verifier should be able to determine which material entered which governed history, after which protected state, under which historical rules, and with which unresolved limitations.**

---

# Summary

The Hash Chain is the TrustAgentAI mechanism for binding Protocol Objects to protected ordered history.

A conforming Hash Chain establishes a disciplined historical boundary:

1. every Chain has a governed CID and explicit scope;
2. every Chain begins from a governed genesis or continuity boundary;
3. every non-genesis entry binds to the required predecessor state;
4. committed material is typed, versioned, and cryptographically bound;
5. object digests, entry commitments, Chain Heads, and Checkpoints remain distinct;
6. Chain state transitions are deterministic and domain-separated;
7. the ordering relation is explicit and bounded to its scope;
8. append behavior is atomic under the applicable construction;
9. Submission, Acceptance, and Commitment remain distinct;
10. Commitment Receipts make bounded Commitment claims portable;
11. inclusion does not imply semantic validity or Completeness;
12. missing material and proof limitations remain explicit;
13. forks, conflicting heads, and split-view evidence are not silently discarded;
14. Witnesses, Checkpoints, and anchors add distinct assurance properties;
15. batching preserves member identity, proof, order, and partial outcomes;
16. epochs, rollover, sharding, and migration preserve explicit boundaries;
17. committed history is corrected through additional accountable state;
18. deletion does not become silent historical rewrite;
19. historical algorithms, keys, schemas, and governance state remain evaluable;
20. Verification separates Chain validity, availability, Completeness, independence, and Trust Profile achievement;
21. privacy is addressed before durable Commitment;
22. conclusions remain bounded to the evaluated evidence and context.

The foundational Hash Chain rule is:

> **Bind every Commitment to its Chain, predecessor, protected material, and historical rules—and make every discontinuity or conflict visible.**

The broader architectural principle remains:

> **Proof, not logs.**
