# Chapter 9 — Witness Observation Specification

> **A Witness Observation is a signed, scope-bounded statement that an identifiable Witness observed defined protocol state under defined historical conditions.**

## Purpose

This chapter defines the architectural specification for the TrustAgentAI **Witness** role and **Witness Observation (WO)**.

Witnessing reduces exclusive dependence upon the Evidence Producer, Evidence Registry, Commitment Service, or Hash Chain operator by allowing separately accountable parties to observe and attest to defined protocol state.

This chapter establishes:

- Witness role and responsibility;
- Observation Scope (OS);
- Witness Observation semantics;
- Witness identity, keys, eligibility, and Authority;
- substantive independence and Control Domain analysis;
- observation targets, methods, and time semantics;
- canonicalization and cryptographic protection;
- Witness Quorum (WQ) construction and evaluation;
- conflict, equivocation, non-response, and degradation behavior;
- publication, portability, Preservation, and Verification;
- Witness invariants;
- architectural requirements for interoperable implementations.

This chapter applies the common Protocol Object rules defined in [06-Protocol-Objects.md](06-Protocol-Objects.md).

It builds upon the Evidence Record model in [07-Evidence-Record-Specification.md](07-Evidence-Record-Specification.md) and the Hash Chain model in [08-Hash-Chain-Specification.md](08-Hash-Chain-Specification.md).

It also applies the system boundaries in [05-System-Overview.md](05-System-Overview.md) and the principles in [04-Design-Principles.md](04-Design-Principles.md).

It uses the canonical terms defined in [Terminology.md](Terminology.md) and [Acronyms.md](Acronyms.md).

This chapter defines architectural semantics.

It does not define final field names, concrete schemas, media types, wire encodings, canonicalization algorithms, Signature suites, discovery endpoints, transport protocols, Witness compensation, or jurisdiction-specific legal status.

Those details belong to TAIP, Trust Profiles, governed registries, schemas, cryptographic profiles, APIs, and test vectors.

---

# 9.1 Witness Definition

A **Witness** is a logical protocol role that observes defined protocol state and issues a cryptographically protected statement about that observation.

A Witness may be operated by:

- an independent Organization;
- a customer or counterparty;
- an auditor or assurance provider;
- a regulated infrastructure provider;
- a consortium member;
- a separately governed internal control function;
- another eligible entity defined by a Trust Profile.

The existence of a Witness role does not by itself prove independence.

Independence is an evaluated property of the Witness, its operator, its Control Domain, and its relationship to other assurance roles.

---

# 9.2 Witness Observation Definition

A **Witness Observation (WO)** is a signed Protocol Object representing what a Witness claims to have observed about defined protocol state.

A Witness Observation binds:

- the Witness Protocol Identity;
- the Observation Scope;
- the observed target or state commitment;
- Observation Time semantics;
- applicable versions and algorithms;
- any declared validation performed;
- relevant eligibility or Trust Profile context;
- cryptographic protection.

```text
Defined Protocol State
          │
          ▼
       Witness
          │ observe under explicit scope
          ▼
  Witness Observation
          │ signed statement
          ▼
 Independent Verification
```

A Witness Observation is evidence of a bounded observation statement.

It is not automatic endorsement of every underlying claim.

---

# 9.3 Scope

A Witness Observation may concern:

- an Evidence Record commitment;
- a Chain Entry;
- a Chain Head;
- a Commitment Receipt;
- a Chain range or continuity boundary;
- a batch or aggregate commitment;
- a Checkpoint candidate;
- a migration or closure boundary;
- key or Registry state;
- another governed Protocol Object or protocol state.

A Witness need not receive or inspect all underlying sensitive content.

It may observe a digest, typed commitment, authenticated state, proof, or complete object depending upon the Observation Scope and Trust Profile.

This chapter does not require every Witness to perform the same observation.

It requires each observation's meaning and limitations to be explicit.

---

# 9.4 Security Objective

The core security objective of Witnessing is to reduce exclusive reliance on one producing or operating party for later historical claims.

Witnessing can provide independently attributable evidence that defined state was available or presented to an eligible observer within a defined historical context.

The relevant architectural statement is:

> A verifier can determine which Witness observed which exact protocol state, under which scope and historical eligibility rules, at which supported time, and whether the required independent quorum was achieved.

This objective requires more than collecting multiple Signatures.

It requires governed semantics for:

- observation target;
- Observation Scope;
- Witness identity and Key Purpose;
- historical eligibility;
- Control Domain and independence;
- observation timing;
- conflict handling;
- quorum composition;
- Preservation and Verification.

---

# 9.5 What a Witness Observation Establishes

Subject to successful Verification, a Witness Observation may establish that:

- an identifiable Witness issued the observation statement;
- the Witness statement binds to a defined target or state commitment;
- the Witness declared a specific Observation Scope;
- the observation occurred no later than a supported later publication or Checkpoint boundary;
- the Witness performed explicitly declared validation steps;
- the Witness reported a matching, conflicting, unavailable, or other governed result;
- the observation was issued while defined historical identity, key, and eligibility conditions applied;
- the observation contributes to a Witness Quorum under an applicable Trust Profile.

The exact conclusion depends upon the observation type, available dependencies, and Verification Context.

---

# 9.6 What a Witness Observation Does Not Establish

A valid Witness Observation does not by itself prove:

- that the committed business assertion is true;
- that the Actor possessed valid Authority;
- that applicable Policy was lawful or correctly applied;
- that every required event was committed;
- that the Witness inspected underlying plaintext;
- that the Witness verified the entire Hash Chain;
- that the Witness was independent;
- that other Witnesses observed the same state;
- that a Witness Quorum was achieved;
- that no alternative state was shown elsewhere;
- that the underlying evidence remains available;
- Legal Validity or Regulatory Compliance.

```text
Valid Witness Signature
≠
Eligible Witness
≠
Independent Witness
≠
Witness Quorum
≠
Business Truth
```

---

# 9.7 Witness Role and Other Roles

The Witness role is distinct from:

- Evidence Producer;
- Evidence Registry;
- Commitment Service;
- Hash Chain operator;
- Checkpoint Authority;
- External Anchor service;
- Key Transparency service;
- Preservation Service;
- Verification Engine.

One Organization or component may perform multiple roles where the applicable Trust Profile permits it.

Role combination must remain visible.

A service does not become independent merely because it exposes a separate endpoint, process, instance, or key.

---

# 9.8 Witness Operator

The **Witness operator** controls the system that performs observations and issues Witness Observations.

Its responsibilities may include:

- discovering observation targets;
- authenticating observation sources;
- obtaining target state;
- applying declared validation;
- recording acquisition and Observation Time context;
- constructing and signing Witness Observations;
- publishing or returning observations;
- preserving observations and dependencies;
- reporting conflicts and failures;
- supporting historical Verification.

The Witness Protocol Identity may identify the operator, a governed service, or another accountable entity according to TAIP.

The responsible Control Domain must remain attributable.

---

# 9.9 Witness Protocol Identity

Every interoperable Witness Observation must identify the Witness according to governed Protocol Identity semantics.

Witness identity rules must support resolution of:

- stable identity reference;
- responsible Organization or operator;
- applicable service or role identity;
- historical status;
- identity namespace and version;
- relationships to signing keys;
- relationships to Witness Registry entries;
- Control Domain and independence claims where required.

A display name, hostname, certificate subject, account name, or network address is not automatically a stable Witness identity.

Identity must remain distinguishable from the key used to sign one observation.

---

# 9.10 Witness Key and Key Purpose

A Witness Observation Signature must identify or resolve the signing key and applicable Key Purpose.

Historical Verification may require:

- Key Identifier;
- algorithm and Signature suite;
- activation and retirement times;
- suspension or revocation state;
- compromise evidence;
- binding between key and Witness identity;
- authorization for the Witness Observation purpose;
- Historical Key State.

```text
Valid Cryptographic Signature
      +
Valid Historical Key State
      +
Correct Key Purpose
      ≠
Witness Independence
```

Key control proves use of a signing capability under defined assumptions.

It does not alone prove eligibility, independence, or correct observation.

---

# 9.11 Witness Eligibility

**Witness eligibility** determines whether a Witness may satisfy a defined assurance requirement.

Eligibility rules may consider:

- registered identity;
- approved Witness class;
- permitted observation types;
- operational or legal qualification;
- key and algorithm status;
- geographic or jurisdictional constraints;
- financial or organizational conflicts;
- Control Domain;
- service availability history;
- required accreditation;
- effective interval;
- suspension or revocation state.

Eligibility is historical.

A Witness eligible today may not have been eligible when an earlier observation was issued.

Trust Profile evaluation must use the rules and Registry state applicable to the relevant observation boundary.

---

# 9.12 Witness Registry

A governed **Witness Registry** may record the information needed to discover and evaluate Witnesses.

Registry state may include:

- Witness Protocol Identity;
- operator and Organization;
- supported Observation Scopes;
- service endpoints or discovery metadata;
- keys or key-resolution references;
- Key Purposes;
- eligibility class;
- Control Domain declarations;
- independence-relevant relationships;
- effective intervals;
- suspension, revocation, and retirement;
- supported versions and algorithms;
- applicable policies or Trust Profiles.

The Registry is an interpretation dependency.

Its current state must not silently replace historical eligibility state.

A Registry assertion of independence is evidence to evaluate, not automatic proof of independence.

---

# 9.13 Observation Scope

The **Observation Scope (OS)** defines exactly what the Witness claims to have observed and, where applicable, what validation it claims to have performed.

Observation Scope may specify:

- target object or state type;
- target identifier and cryptographic commitment;
- Chain, range, batch, or namespace context;
- representation observed;
- source or acquisition method;
- validation checks performed;
- time semantics;
- completeness or availability boundaries;
- disclosure level;
- critical limitations.

Observation Scope is security-critical.

It must be governed, versioned, and cryptographically bound.

Free-form prose may supplement scope but must not replace required machine-interpretable semantics.

---

# 9.14 Observation Scope Classes

TAIP or governed registries may define Observation Scope classes.

Representative classes include:

## Receipt Observation

The Witness observed a defined Commitment Receipt.

## Chain Entry Observation

The Witness observed a defined Chain Entry and protected relationship.

## Chain Head Observation

The Witness observed a defined Chain Head for a CID.

## Range Continuity Observation

The Witness validated a declared Chain segment between defined boundaries.

## Object Commitment Observation

The Witness observed that a defined object commitment was included in historical state.

## Checkpoint Candidate Observation

The Witness observed state proposed for later Checkpointing.

Different scope classes must not be treated as equivalent merely because they reference the same digest.

---

# 9.15 Observation Target

Every Witness Observation must bind unambiguously to its observation target.

The target binding may include:

- Protocol Object type;
- identifier;
- canonical digest or typed commitment;
- CID;
- Chain position or range;
- Chain Head;
- batch root and membership information;
- target version;
- domain and algorithm context;
- source identity where relevant.

A locator may assist retrieval.

It must not replace an integrity-bound target commitment.

```text
Observed Target Identifier
≠
Observed Target Commitment
≠
Retrieval Locator
```

---

# 9.16 Observation of an Evidence Record Commitment

A Witness may observe that an Evidence Record commitment appears in defined historical state without receiving the complete Evidence Record.

Such an observation may establish:

- the exact typed commitment observed;
- its inclusion in a Chain Entry or aggregate;
- the relevant Chain state;
- the Witness's declared validation scope;
- supported Observation Time.

It does not establish:

- semantic validity of the Evidence Record;
- correctness of the producer's assertion;
- availability of the complete record;
- Authority, Policy, or causal completeness;
- absence of another conflicting record.

The Witness Observation must disclose whether the underlying object was available and inspected.

---

# 9.17 Observation of Chain State

A Witness may observe a Chain Entry, Chain Head, range, or closure boundary.

The Observation Scope must state whether the Witness:

- received a claimed Chain Head only;
- validated a specific Chain Entry link;
- validated continuity from a previously retained head;
- validated a complete range;
- compared the state with another source;
- checked for an incompatible previously observed state;
- retained the observed material.

```text
Received Chain Head
≠
Validated Chain Link
≠
Validated Chain Range
≠
No Equivocation Detected Across Defined Sources
```

The strongest conclusion must not exceed the declared and verified scope.

---

# 9.18 Observation of a Commitment Receipt

A Witness may observe a Commitment Receipt issued by a Registry, Commitment Service, or Chain operator.

The observation must distinguish whether the Witness:

- observed receipt bytes;
- validated the receipt Signature;
- resolved the committed object;
- validated Chain inclusion;
- validated continuity to a retained Chain Head;
- verified operator identity or Authority;
- merely recorded receipt availability.

A valid Witness Observation of a receipt is not automatically independent Verification of the underlying Commitment.

The receipt and the Witness Observation remain separately verifiable Protocol Objects.

---

# 9.19 Direct and Derived Observation

Observation provenance must distinguish:

## Direct Observation

The Witness obtained target state from the governed source or protocol mechanism identified by the Observation Scope.

## Relayed Observation

The Witness obtained target state through an intermediary.

## Derived Observation

The Witness derived a conclusion from other evidence, proofs, or observations.

## Reconstructed Observation

The observation was created later from retained operational or archival material.

Derived, relayed, and reconstructed observations may be valid within their declared semantics.

They must not be represented as direct contemporaneous observation.

---

# 9.20 Observation Method and Source

The Observation Scope may bind the method and source used to obtain state.

Relevant properties may include:

- source Protocol Identity;
- authenticated endpoint or channel;
- push, pull, publication, or gossip method;
- requested and received target identifiers;
- transport protection;
- proof material;
- retry and timeout behavior;
- source diversity;
- acquisition failure status.

Transport authentication can support provenance.

It does not replace target-level cryptographic binding.

An observation from the Chain operator and an observation from an independent publication channel may provide different assurance even when the target bytes match.

---

# 9.21 Validation Performed by the Witness

A Witness Observation may declare validation performed before issuance.

Declared checks may include:

- parsing and structural validation;
- canonical digest reproduction;
- Signature validation;
- Chain linkage validation;
- inclusion proof validation;
- range continuity validation;
- operator identity or key validation;
- comparison with previously retained state;
- conflict detection across sources;
- schema or version support.

The declaration must state the validation profile or exact governed checks.

The phrase **verified by Witness** is insufficient without defined scope.

A verifier may reperform declared checks independently when the required evidence is available.

---

# 9.22 Observation Result

Witness Observation result semantics must be governed.

Representative results include:

- observed and matched;
- observed with declared limitations;
- observed conflicting state;
- target unavailable;
- proof invalid;
- unsupported version;
- indeterminate;
- observation attempt failed.

A negative or failure result may itself be accountability-relevant evidence.

The result must remain distinguishable from:

- non-response;
- absence of an observation request;
- observation not yet due;
- Witness ineligibility;
- later revocation of eligibility.

---

# 9.23 Observation Time

**Observation Time** is the time at which the Witness claims to have observed the defined target or completed the governed observation procedure.

Its assurance depends upon:

- the Witness clock;
- precision and synchronization;
- whether time is cryptographically bound;
- acquisition and issuance delay;
- external time evidence;
- later publication, Checkpoint, or anchor boundaries;
- Trust Profile rules.

```text
Event Time
≠
Commitment Time
≠
Observation Time
≠
Witness Observation Signature Time
≠
Publication Time
```

A Witness-supplied Observation Time is an attributable assertion unless additional evidence strengthens it.

---

# 9.24 Observation Window and Freshness

A Trust Profile may require observation within a defined window relative to:

- Commitment Time;
- Chain position;
- Checkpoint schedule;
- action completion;
- a prior Witness Observation;
- a publication event;
- another governed boundary.

Freshness rules must define:

- window start and end semantics;
- clock assumptions;
- permitted delay;
- treatment of missing or imprecise times;
- retry behavior;
- late observation outcome;
- grace and degradation behavior.

A valid late observation may still provide evidence.

It may fail a Trust Profile timing requirement.

Object validity and profile achievement must remain separate.

---

# 9.25 Observation Identifier

TAIP may define a stable identifier for each Witness Observation.

If defined, identifier rules must establish:

- namespace and generation responsibility;
- uniqueness and comparison behavior;
- relationship to Witness identity;
- relationship to the observation target;
- relationship to the canonical observation digest;
- collision handling;
- persistence across export and reserialization.

```text
Witness Observation Identifier
≠
Witness Observation Digest
≠
Target Identifier
```

Different protected observations associated with the same identifier must be surfaced as a conflict.

---

# 9.26 Logical Information Model

The Witness Observation logical model contains governed properties applicable to its scope and version.

| Logical property group | Purpose |
|---|---|
| Type and version | Determines object and observation semantics |
| Observation identifier | Provides stable governed reference where defined |
| Witness identity | Attributes the observation statement |
| Witness key and purpose | Supports historical Signature evaluation |
| Observation Scope | Bounds what was observed and checked |
| Observation target | Binds the exact state or commitment |
| Source and method | Describes acquisition provenance where required |
| Observation result | States the governed outcome |
| Time properties | Distinguishes observation, Signature, and publication time |
| Eligibility context | References applicable Witness Registry state |
| Independence context | Supplies evidence for Control Domain analysis |
| Trust Profile context | Identifies intended assurance requirements where applicable |
| Extensions | Adds governed optional or critical semantics |
| Cryptographic protection | Protects integrity and attribution |

The model is logical rather than a final wire schema.

---

# 9.27 Common Object Envelope

A Witness Observation is a Protocol Object and participates in the Common Object Envelope defined by Chapter 6.

The envelope must make it possible to determine:

- that the object is a Witness Observation;
- the applicable Witness Observation and TAIP versions;
- the Witness identity;
- the typed observation body;
- the Observation Scope;
- applicable references and extensions;
- the cryptographic protection model.

Transport metadata, queue state, database columns, and API paths must not silently supply security-critical observation meaning unless TAIP explicitly binds them into the governed object.

---

# 9.28 Canonical Representation

Every finalized Witness Observation participating in digest, Signature, quorum, Checkpoint, or Preservation operations must have a deterministic canonical representation.

The applicable definition must address:

- logical property coverage;
- Witness identity and Key Identifier;
- Observation Scope;
- target commitment;
- result and time semantics;
- eligibility and profile references;
- absent, null, default, and unknown values;
- extension handling;
- domain separation;
- collection ordering;
- size and nesting limits.

Independent conforming implementations must derive equivalent cryptographic input from equivalent conforming Witness Observations.

---

# 9.29 Witness Observation Signature

A Witness Observation Signature protects the canonical observation statement and attributes use of a Witness signing capability.

Signature input must bind all substitution-sensitive semantics, including:

- object type and version;
- Witness identity;
- Key Identifier and Key Purpose;
- Observation Scope;
- target commitment;
- observation result;
- Observation Time semantics;
- critical extensions;
- relevant context identifiers.

Unprotected transport or presentation metadata must not be interpreted as signed observation meaning.

A valid Signature does not establish that the Witness observed honestly or independently.

---

# 9.30 Finalization and Issuance

Finalization establishes the Witness Observation content intended for canonicalization, digesting, signing, issuance, publication, or Commitment.

Before finalization, a local draft may be mutable.

After finalization and Signature, protected content must not be changed in place.

```text
Observation Attempt
      │ acquire and validate
      ▼
Observation Draft
      │ finalize and sign
      ▼
Issued Witness Observation
```

Issuance means the Witness created the signed Protocol Object.

It does not automatically mean the observation was delivered, published, committed, included in a quorum, checkpointed, or preserved.

---

# 9.31 Delivery, Acceptance, and Commitment

Witness Observation lifecycle states must remain distinct.

## Issued

The Witness finalized and signed the observation.

## Delivered

The observation was transmitted to an intended receiver or publication service.

## Accepted

The receiver accepted the observation for defined processing.

## Committed

The observation entered protected historical state.

## Counted

The observation was evaluated as contributing to a defined Witness Quorum.

```text
Issued
≠
Delivered
≠
Accepted
≠
Committed
≠
Counted in Quorum
```

A successful delivery response does not prove quorum contribution.

---

# 9.32 Replay and Context Binding

A valid Witness Observation must not be reusable to support a different target, Chain, epoch, Trust Profile, quorum window, or protocol context unless the applicable semantics explicitly permit that use.

Replay resistance may require binding:

- target commitment;
- CID and Chain state;
- observation request or challenge identifier;
- nonce;
- intended audience or relying context;
- Observation Scope version;
- validity or freshness interval;
- Trust Profile and quorum instance;
- protocol domain.

The same observation may be cited in multiple Verification Reports when each report evaluates the original observation within its valid scope.

That is reuse of evidence, not replay as a new observation.

---

# 9.33 Duplicate and Idempotent Processing

Distributed systems may receive the same Witness Observation more than once.

The following cases must remain distinguishable:

- retransmission of the same canonical observation;
- duplicate publication of the same observation;
- reuse of an observation identifier with different protected content;
- two distinct observations by the same Witness about the same target;
- a corrected or superseding observation;
- a conflicting observation;
- replay into an unrelated quorum instance.

Idempotent storage may collapse equivalent deliveries operationally.

It must not collapse distinct accountability-relevant statements or hide identifier conflicts.

One Witness must not count multiple times toward a quorum merely because the same observation was delivered through multiple channels.

---

# 9.34 Correction, Supersession, and Revocation

A Witness may discover that an issued observation was erroneous, incomplete, based upon compromised state, or no longer usable for a defined purpose.

The Witness or another authorized party may issue additional accountable evidence that:

- corrects a statement;
- annotates a limitation;
- supersedes an observation;
- revokes reliance for a defined purpose;
- reports key compromise;
- disputes a target or earlier result.

The original signed observation remains part of history.

```text
Original Witness Observation
          │
          ├── corrected-by ──► Correction Observation
          ├── superseded-by ─► Superseding Observation
          └── reliance-revoked-by ─► Revocation Evidence
```

Later status must not be backdated silently.

Quorum evaluation must apply the correction or revocation semantics relevant to the evaluated historical boundary.

---

# 9.35 Publication and Dissemination

Witness Observations may be:

- returned directly to a requester;
- submitted to an Evidence Registry;
- committed to a Hash Chain;
- published through a Witness service;
- exchanged among Witnesses;
- included in a Checkpoint package;
- preserved in a Dispute Pack;
- distributed through another governed mechanism.

Publication can strengthen availability and cross-observer comparison.

It does not by itself make a Witness independent or an observation valid.

Publication evidence should preserve source, target, time, version, and integrity semantics.

A mutable web page or unprotected API response is not a durable substitute for the signed Witness Observation.

---

# 9.36 Independence Definition

**Witness independence** is the degree to which a Witness's observation and signing behavior are not controlled by, dependent upon, or predictably correlated with the party whose state it is intended to corroborate or with other Witnesses counted as independent.

Independence is contextual.

A Witness may be independent for one Accountability Claim and not for another.

For example:

- a counterparty may be independent of the Evidence Producer but financially interested in the outcome;
- an internal audit service may be administratively separate but share infrastructure and executive control;
- two vendors may be separately incorporated but depend upon one cloud account or signing service;
- a customer-operated Witness may be independent of the platform but unavailable during an incident.

Independence must be evaluated against explicit criteria rather than inferred from labels.

---

# 9.37 Control Domain

A **Control Domain** groups components, Organizations, keys, operators, or infrastructure subject to sufficiently common control or correlated failure for the applicable assurance analysis.

Control Domain analysis may consider:

- ownership and corporate control;
- administrative authority;
- personnel and privileged access;
- signing-key custody;
- deployment account or subscription;
- infrastructure provider and region;
- software supply chain;
- network path and identity provider;
- funding or contractual dependence;
- source data and observation channel;
- governance and incident response.

The applicable Trust Profile defines which dimensions matter and how Control Domains are distinguished.

Control Domain identifiers and supporting evidence must be historically interpretable.

---

# 9.38 Dimensions of Independence

Independence may include several non-equivalent dimensions.

| Dimension | Question |
|---|---|
| Organizational | Are the operators under separate ownership or management? |
| Administrative | Can one administrator alter both Witnesses? |
| Cryptographic | Are keys and signing services separately controlled? |
| Infrastructure | Do the Witnesses share hosting, region, network, or identity dependencies? |
| Software | Do they share one build, update channel, or vulnerable implementation? |
| Data-source | Do they obtain state from genuinely distinct sources? |
| Economic | Do incentives or contracts create common influence? |
| Geographic | Are they exposed to the same physical or jurisdictional disruption? |
| Governance | Can one authority change both eligibility or operating rules? |

No single dimension universally proves independence.

Trust Profiles may require separation across selected dimensions and may tolerate correlation across others.

---

# 9.39 Correlated Failure and Common-Mode Risk

Witness count alone does not measure assurance when failures are correlated.

Common-mode risks include:

- shared signing infrastructure;
- shared cloud account or region;
- identical software defect;
- compromised dependency or update channel;
- shared identity provider;
- one operator controlling configuration;
- one observation source presenting the same false state;
- common legal or economic pressure;
- shared clock or timestamp service;
- coordinated network isolation.

```text
Three Witness Processes
      under one administrator
      using one signing service
      observing one operator endpoint
      =
Resilience may increase; independence may not
```

Quorum evaluation must account for the Control Domain model required by the Trust Profile.

---

# 9.40 Role Combination

A Witness may share an Organization or component with another TrustAgentAI role where permitted.

Examples include:

- Registry and Witness;
- Chain operator and Witness;
- Checkpoint Authority and Witness;
- Preservation Service and Witness;
- verifier and Witness.

Role combination affects assurance.

A Chain operator observing its own Chain Head can produce useful self-attestation and operational evidence.

It does not supply independent corroboration of that Chain operator's view.

Trust Profile evaluation must classify the observation according to actual control relationships rather than the role name printed in the object.

---

# 9.41 Conflicts of Interest

Witness eligibility and weighting may depend upon conflicts of interest.

Relevant conflicts may include:

- financial exposure to the underlying action;
- contractual incentives tied to one outcome;
- ownership by an observed party;
- liability transfer arrangements;
- governance participation;
- operational dependence;
- competitive or adversarial interest;
- prior involvement in producing or approving the observed state.

A conflict does not always make an observation invalid.

It may make the Witness ineligible for an independence requirement or require separate disclosure and weighting.

Conflict rules must be governed, historical, and claim-specific.

---

# 9.42 Witness Quorum

A **Witness Quorum (WQ)** is a set of eligible Witness Observations satisfying the threshold, independence, scope, timing, and other conditions defined by an applicable Trust Profile or quorum policy.

```text
Valid Witness Observations
          │
          ├── eligible Witnesses
          ├── compatible Observation Scope
          ├── matching target state
          ├── required time window
          ├── required Control Domains
          └── threshold satisfied
          │
          ▼
    Witness Quorum Achieved
```

Quorum is a derived conclusion.

It is not created merely by placing several Signatures in one file.

---

# 9.43 Quorum Policy

Every quorum claim must identify or resolve the applicable governed quorum policy.

The policy may define:

- policy identifier and version;
- eligible Witness classes;
- eligible Witness population;
- required Observation Scope;
- target matching rules;
- threshold model;
- weighting rules;
- required number of Control Domains;
- independence dimensions;
- observation window;
- permitted late or partial observations;
- conflict and dissent handling;
- suspension and revocation behavior;
- grace and degradation behavior;
- evidence and Preservation requirements.

The current quorum policy must not silently replace the historical version applicable to an earlier quorum claim.

---

# 9.44 Threshold and Weight Models

Quorum thresholds may be expressed through:

- a minimum count;
- a fraction of an eligible population;
- weighted voting power;
- threshold per Witness class;
- threshold per Control Domain;
- a composite rule;
- another governed condition.

The model must define the denominator and eligibility boundary.

```text
2 of 3 Configured Witnesses
≠
2 of 3 Historically Eligible Witnesses
≠
2 Independent Control Domains
```

Weights must not be inferred from duplicate observations, service capacity, or organizational size unless the policy defines that relationship.

Threshold arithmetic must be deterministic and independently reproducible.

---

# 9.45 Control-Domain Thresholds

A Trust Profile may require a minimum number of independent Control Domains in addition to a Witness count.

For example:

```text
Required:
  at least 3 valid observations
  from at least 2 eligible Control Domains
  including at least 1 external Organization
```

Five Witnesses within one Control Domain may satisfy a resilience target while failing the independence condition.

Control-Domain membership used for quorum must be supported by historical Registry, governance, or other evidence.

Unknown or conflicting Control-Domain relationships must not be silently treated as independent.

---

# 9.46 Scope and Target Equivalence

Observations count toward the same quorum only when their targets and scopes are compatible under the quorum policy.

Compatibility may require equality of:

- target object type;
- target commitment;
- CID and Chain Head;
- position or range;
- batch root;
- Observation Scope class and version;
- validation profile;
- protocol domain;
- critical extensions.

Observing different Chain Heads at different legitimate positions is not necessarily conflict.

Counting them toward one same-state quorum requires rules that establish a common covered boundary.

An aggregator must not normalize away a security-relevant mismatch.

---

# 9.47 Quorum Timing

Quorum policy must define the time relationship among contributing observations.

Timing rules may require:

- all observations within one window;
- maximum spread between earliest and latest observation;
- observation before a Checkpoint deadline;
- observation after a defined Commitment;
- observation of the same or monotonically compatible Chain state;
- freshness at Verification Time.

Observations valid individually may fail to form a quorum if they are too far apart or refer to incompatible historical boundaries.

Uncertain or unsupported time values must produce an explicit limitation rather than assumed timing compliance.

---

# 9.48 Historical Eligibility and Dynamic Membership

Witness populations may change through onboarding, suspension, revocation, retirement, replacement, or governance updates.

Quorum evaluation must establish:

- which Witnesses were eligible;
- which quorum policy applied;
- which Control Domain assignments applied;
- whether a key and Key Purpose were valid;
- whether suspension or revocation affected the observation;
- whether membership changes occurred before or after the quorum boundary.

A later removal from the Registry does not automatically invalidate a historically valid observation.

A later-added Witness must not be counted retroactively unless the governing rules explicitly support that effect.

---

# 9.49 Quorum Aggregation Evidence

A quorum aggregator may assemble Witness Observations and produce a compact quorum result or proof package.

The aggregator's output must identify or bind:

- target state;
- quorum policy and version;
- contributing observations;
- excluded or rejected observations;
- Witness identities and Control Domains;
- eligibility and timing evidence;
- threshold calculation;
- conflicts and limitations;
- aggregation method and version;
- aggregator identity where applicable.

The aggregator does not create Witness independence.

Independent Verification should be able to reproduce the quorum conclusion from the contributing evidence or a governed cryptographic proof.

---

# 9.50 Conflicting Observations and Dissent

Witnesses may report incompatible target state, validation outcomes, or historical views.

Conflicts may include:

- different Chain Heads for the same exclusive boundary;
- different object commitments for the same identifier;
- valid versus invalid proof results;
- incompatible Observation Scope claims;
- contradictory publication or timing evidence;
- different views of Witness eligibility or Registry state.

Conflicting observations must remain visible.

A majority or threshold result must not erase dissenting evidence.

The quorum policy must define whether conflict:

- prevents quorum;
- lowers the achieved assurance;
- triggers investigation;
- requires a Checkpoint hold;
- produces a separate conflicting outcome.

---

# 9.51 Non-Response, Absence, and Negative Evidence

Witness non-response does not automatically mean that the target state was absent, invalid, or conflicting.

The following conditions must remain distinguishable:

- no observation request was made;
- request delivery failed;
- Witness timed out;
- Witness was unavailable;
- Witness refused the request;
- target was unavailable;
- target was observed and failed validation;
- Witness issued a negative result;
- observation exists but cannot be retrieved.

Negative evidence requires governed semantics and attributable support.

Silence must not be converted into a signed negative observation.

---

# 9.52 Availability and Graceful Degradation

Witness services may be delayed or unavailable.

Governance may permit an operational action or Commitment to continue with reduced assurance.

Degradation rules must define:

- which actions may proceed;
- minimum remaining controls;
- retry and recovery behavior;
- maximum grace interval;
- required downgrade evidence;
- notification and escalation;
- later observation or reconciliation duties;
- effect on Intended and Achieved Trust Profiles.

```text
Intended Profile Requires WQ
          │
          ├── WQ achieved ──► intended Witness assurance achieved
          └── WQ missing ───► explicit downgrade or incomplete outcome
```

Operational success must not be represented as Trust Profile success when the required Witness Quorum was not achieved.

---

# 9.53 Equivocation Detection and Witness Gossip

Witnesses may strengthen split-view detection by exchanging or publishing observed state.

A governed gossip or comparison mechanism may bind:

- CID and Chain state;
- Witness identities;
- Observation Times;
- source and acquisition path;
- prior retained state;
- conflict evidence;
- applicable versions;
- comparison result.

Gossip is not automatically trustworthy merely because it is peer-to-peer.

It requires authenticated observations, replay protection, bounded resource use, and conflict-preserving behavior.

Two Witnesses receiving the same false view from one source may agree without proving absence of equivocation elsewhere.

---

# 9.54 Relationship to Checkpoints and External Anchors

Witness Observations, Checkpoints, and External Anchors contribute different assurance properties.

```text
Chain Head
    │
    ├── observed by Witnesses ──► Witness Observations / Quorum
    │
    ├── committed as boundary ──► Checkpoint
    │
    └── published across trust boundary ──► External Anchor
```

A Checkpoint may require or include evidence of a Witness Quorum.

It must not silently transform ineligible or correlated Witnesses into an eligible quorum.

An External Anchor may support timing or cross-domain publication without replacing Witness eligibility or Observation Scope analysis.

Checkpoint and External Anchor object semantics remain defined by separate later specifications.

---

# 9.55 Privacy and Confidentiality

Witnessing can expose sensitive information about Organizations, identities, transactions, timing, workflow volume, and relationships.

Privacy design should consider:

- whether the Witness needs complete content or only a typed commitment;
- whether observation targets are low entropy or guessable;
- whether Witness selection reveals sensitive risk classification;
- whether correlation identifiers create linkability;
- whether public observations expose business activity;
- whether quorum packages reveal unnecessary organizational topology;
- whether source and network metadata require protection;
- whether redaction preserves target and scope integrity;
- whether retention is proportionate to the Accountability Claim.

Encryption or selective disclosure may limit exposure.

The Witness must still be able to bind its statement to the exact governed target.

---

# 9.56 Retention and Preservation

Witness Observation Preservation must cover the observation and dependencies required for future interpretation.

Relevant material may include:

- canonical Witness Observation;
- Signature and algorithm context;
- Witness identity and Historical Key State;
- historical Witness Registry state;
- eligibility and Control Domain evidence;
- Observation Scope definitions;
- target objects, commitments, or proofs;
- quorum policy and calculation evidence;
- correction, revocation, and conflict evidence;
- publication, Checkpoint, or anchor references.

Storing observations in the observed party's database alone may preserve availability without providing independent custody.

Preservation claims must identify the relevant Control Domain and evidence lifetime.

---

# 9.57 Portability and Export

A portable Witness evidence package should allow an independent verifier to reproduce the intended observation and quorum conclusions outside the originating platform.

Depending upon scope, it may include:

- Witness Observations;
- target commitments and proof material;
- Observation Scope definitions;
- Witness identity and historical key evidence;
- historical Witness Registry entries;
- quorum policy and version;
- Control Domain evidence;
- included, excluded, late, and conflicting observations;
- correction and revocation history;
- missing or unavailable dependency statements.

File order and package layout do not determine quorum membership.

Protected object relationships and governed policy remain authoritative.

---

# 9.58 Validation and Verification

Witness evaluation is layered.

## Availability and Parsing

Can the observation and mandatory dependencies be obtained and parsed safely?

## Structural and Semantic Validation

Do type, version, scope, target, result, time, and extension values satisfy applicable rules?

## Canonicalization and Signature Validation

Can the protected input be reproduced and the Signature validated under Historical Key State and Key Purpose?

## Target-Binding Validation

Does the observation bind to the exact object, commitment, Chain state, range, or proof claimed?

## Observation-Scope Validation

Do the available materials support the exact checks and result declared by the Witness?

## Eligibility Validation

Was the Witness eligible under the historical Registry and policy state?

## Independence Validation

Do Control Domain and relationship evidence satisfy the applicable independence criteria?

## Timing Validation

Does the observation satisfy required window, freshness, and time-assurance rules?

## Quorum Validation

Do compatible eligible observations satisfy threshold, Control Domain, timing, and conflict rules?

## Profile and Completeness Validation

Are all Witness and related controls required by the Trust Profile satisfied?

A representative verifier should:

1. identify the Accountability Claim and Verification Context;
2. resolve the target state and Observation Scope;
3. validate each Witness Observation object and Signature;
4. validate target binding and declared checks;
5. resolve historical Witness identity, key, eligibility, and Control Domain state;
6. evaluate timing and freshness;
7. group only scope-compatible observations;
8. surface duplicates, conflicts, dissent, and non-response correctly;
9. apply the historical quorum policy deterministically;
10. report observation validity, independence, quorum, Completeness, and limitations separately.

---

# 9.59 Implementation Mapping and Illustrative Sequence

The logical Witness model may be implemented through:

- independent Witness services;
- customer-operated observers;
- consortium members;
- regulated timestamp or assurance providers with Witness capability;
- separately governed internal control systems;
- hardware-protected observation agents;
- distributed transparency monitors;
- hybrid push, pull, and publication networks.

Implementation technology does not change the architectural requirements.

```text
Chain Operator             Witness A              Witness B             Verifier
     │                         │                      │                    │
     │ publish Chain Head H    │                      │                    │
     │────────────────────────►│                      │                    │
     │ publish Chain Head H    │                      │                    │
     │───────────────────────────────────────────────►│                    │
     │                         │ validate scope       │ validate scope     │
     │                         │ sign WO-A            │ sign WO-B          │
     │                         │                      │                    │
     │                         │ WO-A                 │                    │
     │                         │──────────────────────────────────────────►│
     │                         │                      │ WO-B               │
     │                         │                      │───────────────────►│
     │                         │                      │       validate identities
     │                         │                      │       evaluate independence
     │                         │                      │       apply quorum policy
```

Agreement by Witness A and Witness B contributes independent assurance only if their eligibility and Control Domain relationships satisfy the applicable policy.

---

# 9.60 Common Anti-Patterns and Relationship to Other Specifications

## Signature Count Equals Quorum

Counting Signatures without evaluating eligibility, scope, timing, and independence.

## Replica Equals Independent Witness

Treating several instances under one Control Domain as independent assurance.

## Witness Equals Auditor of Everything

Interpreting observation of a Chain Head as validation of every underlying business assertion.

## Scope-Free Attestation

Using vague statements such as witnessed or verified without governed Observation Scope.

## Current Registry Verification

Using current Witness eligibility or key state to interpret all historical observations.

## Silence Equals Negative Observation

Treating timeout or non-response as proof that target state was absent or invalid.

## Duplicate Counting

Counting repeated delivery or multiple observations from one Witness as multiple independent Witnesses.

## Conflict Suppression

Discarding dissenting or incompatible observations after a threshold is reached.

## Control-Domain Labeling

Accepting self-declared independence without evaluating supporting relationships and common-mode risk.

## Timestamp Inflation

Treating Witness-supplied Observation Time as independently trusted time without supporting evidence.

## Aggregator Trust Substitution

Treating an aggregator's success response as proof of a valid quorum without contributing evidence or governed proof.

## Retroactive Membership

Counting a later-added Witness toward an earlier quorum without explicit historical rules.

## Checkpoint Substitution

Treating the presence of a Checkpoint as proof that Witness eligibility and quorum requirements were satisfied.

## Over-Disclosure

Sending complete sensitive Evidence Records to Witnesses when typed commitments would satisfy the defined claim.

This chapter defines Witness, Witness Observation, Observation Scope, independence, and Witness Quorum semantics.

Other chapters and TAIP define related concerns, including:

- Checkpoints and External Anchors;
- Protocol Identity and Key Transparency;
- Trust Profiles and assurance levels;
- Preservation Evidence;
- Verification Engines and Reports;
- Dispute Pack construction;
- security and privacy threat models;
- concrete schemas, algorithms, APIs, registries, and test vectors.

Those specifications may strengthen or specialize Witness requirements.

They must preserve the Witness invariants defined here.

---

# Witness Invariants

### INV-WIT-001 — Bounded Observation

Every Witness Observation MUST state a governed Observation Scope that bounds what the Witness claims to have observed and validated.

### INV-WIT-002 — Exact Target Binding

Every Witness Observation MUST bind unambiguously to the exact Protocol Object, commitment, Chain state, range, aggregate, or other governed target it concerns.

### INV-WIT-003 — Witness Attribution

Every Witness Observation MUST identify the Witness responsible for the observation statement according to governed Protocol Identity semantics.

### INV-WIT-004 — Identity/Key Separation

Witness identity, signing key, Key Purpose, operator, Organization, and Control Domain MUST remain distinguishable.

### INV-WIT-005 — Signature/Independence Separation

A valid Witness Observation Signature MUST NOT automatically be interpreted as Witness eligibility, independence, or quorum contribution.

### INV-WIT-006 — Historical Eligibility

Witness eligibility MUST be evaluated using the Registry, policy, key, and governance state applicable to the observation's historical boundary.

### INV-WIT-007 — Control-Domain Visibility

Common control and correlated failure relationships affecting Witness assurance MUST remain visible.

### INV-WIT-008 — Replication/Independence Separation

Multiple Witness instances, keys, processes, or endpoints within one Control Domain MUST NOT automatically be represented as independent Witnesses.

### INV-WIT-009 — Role-Combination Visibility

Combination of Witness, producer, Registry, Chain, Checkpoint, Preservation, or Verification roles MUST remain visible during assurance evaluation.

### INV-WIT-010 — Explicit Observation Result

Matching, conflicting, invalid, unavailable, unsupported, failed, and indeterminate observation results MUST remain distinguishable where they affect Verification.

### INV-WIT-011 — Direct/Derived Separation

Direct, relayed, derived, and reconstructed observations MUST NOT be silently conflated.

### INV-WIT-012 — Time Separation

Event Time, Commitment Time, Observation Time, Witness Signature Time, Publication Time, and Checkpoint Time MUST remain distinguishable.

### INV-WIT-013 — Lifecycle Separation

Issuance, delivery, Acceptance, Commitment, quorum contribution, Checkpointing, Preservation, and Verification MUST NOT be conflated.

### INV-WIT-014 — Observation Immutability

Protected content of an issued Witness Observation MUST NOT be silently modified.

### INV-WIT-015 — Append-Only Correction

Correction, supersession, revocation, dispute, and limitation of an issued Witness Observation MUST create additional accountable evidence.

### INV-WIT-016 — Context-Bound Use

A Witness Observation MUST NOT be reused as a new observation for an incompatible target, scope, time window, quorum instance, Trust Profile, or protocol domain.

### INV-WIT-017 — Duplicate Visibility

Duplicate delivery, repeated observation, identifier conflict, replay, correction, and conflicting observation MUST remain distinguishable.

### INV-WIT-018 — Quorum as Derived Conclusion

Witness Quorum achievement MUST be derived from valid evidence and governed rules rather than asserted by count or label alone.

### INV-WIT-019 — Compatible Quorum Scope

Only observations compatible under the applicable target and Observation Scope rules MAY contribute to the same quorum conclusion.

### INV-WIT-020 — Deterministic Threshold

Equivalent eligible observations evaluated under the same historical quorum policy MUST produce equivalent threshold results.

### INV-WIT-021 — Substantive Independence

Witness independence MUST be evaluated using the Control Domain and independence criteria required by the applicable Trust Profile.

### INV-WIT-022 — Conflict Visibility

Dissenting, incompatible, and equivocation-relevant Witness Observations MUST NOT be silently discarded after a threshold is reached.

### INV-WIT-023 — Silence/Negative Separation

Witness non-response or absence of an observation MUST NOT be represented as an attributable negative observation.

### INV-WIT-024 — Intended/Achieved Separation

An Intended Trust Profile or configured Witness set MUST NOT be represented as proof that required observation and quorum conditions were achieved.

### INV-WIT-025 — Validity/Completeness Separation

Witness Observation validity and Witness Quorum validity MUST remain distinguishable from evidence Completeness and business truth.

### INV-WIT-026 — Historical Interpretation

Witness Verification MUST use applicable historical scopes, registries, quorum policies, keys, algorithms, and Control Domain evidence.

### INV-WIT-027 — Explicit Uncertainty

Unknown, missing, redacted, unavailable, malformed, unsupported, conflicting, and unverifiable Witness dependencies MUST remain explicit.

### INV-WIT-028 — Privacy Proportionality

Witnessing SHOULD minimize unnecessary sensitive disclosure and linkability while preserving the evidence required by defined claims.

### INV-WIT-029 — Representation Independence

Normative Witness meaning MUST NOT depend upon one service, transport, database, vendor, or user interface.

### INV-WIT-030 — Bounded Reproducible Verification

Independent conforming implementations SHOULD derive equivalent bounded Witness and quorum conclusions from equivalent evidence under equivalent Verification Contexts.

---

# Architectural Requirements

### REQ-WIT-001

TAIP MUST define or reference governed Witness Observation object types, versions, lifecycle semantics, and validation behavior.

### REQ-WIT-002

TAIP MUST define or reference governed Observation Scope classes, versions, target types, declared validation semantics, and result values.

### REQ-WIT-003

Every interoperable Witness Observation MUST identify the Witness Protocol Identity responsible for the statement.

### REQ-WIT-004

Witness Observation Signature evaluation MUST resolve the applicable Key Identifier, Key Purpose, Historical Key State, algorithm policy, and protected scope.

### REQ-WIT-005

Witness Observations MUST bind the exact observation target, type, identifier or namespace, cryptographic commitment, and version context required by the Observation Scope.

### REQ-WIT-006

A target locator MUST NOT substitute for an integrity-bound target commitment where substitution could alter an Accountability Claim.

### REQ-WIT-007

Each Witness Observation version MUST define or reference deterministic canonicalization, domain separation, and cryptographic input construction.

### REQ-WIT-008

Substitution-sensitive Witness identity, scope, target, result, time, profile, and critical extension semantics MUST be cryptographically bound.

### REQ-WIT-009

Observation Time values MUST identify their meaning, source, precision, and supporting assurance and MUST NOT be represented as independently trusted time without sufficient evidence.

### REQ-WIT-010

Observation freshness and timing requirements MUST define their reference boundary, window, clock assumptions, grace behavior, and late-observation outcome.

### REQ-WIT-011

Witness Observations MUST distinguish direct, relayed, derived, and reconstructed observation provenance where the difference affects interpretation.

### REQ-WIT-012

When a Witness declares validation performed, the observation MUST identify the governed checks or validation profile and the evidence scope to which they applied.

### REQ-WIT-013

Witness Observation result semantics MUST distinguish successful match, conflict, invalid proof, unavailable target, unsupported input, failed attempt, and indeterminate state where applicable.

### REQ-WIT-014

Implementations MUST preserve the distinction between observation issuance, delivery, Acceptance, Commitment, quorum contribution, Checkpointing, Preservation, and Verification.

### REQ-WIT-015

Replay-sensitive Witness Observations MUST bind the target and context identifiers, challenge, validity interval, quorum instance, audience, or other values required by the applicable threat model.

### REQ-WIT-016

Duplicate handling MUST distinguish equivalent redelivery from identifier conflict, distinct repeated observation, correction, conflicting observation, and cross-context replay.

### REQ-WIT-017

Protected content of an issued Witness Observation MUST NOT be modified in place.

### REQ-WIT-018

Correction, supersession, revocation, dispute, or changed reliance status for an issued observation MUST identify the affected observation and create additional accountable evidence.

### REQ-WIT-019

Witness Registries MUST preserve or permit resolution of historical identity, eligibility, scope, key, Control Domain, suspension, revocation, and version state required for Verification.

### REQ-WIT-020

Trust Profiles using Witness assurance MUST define the eligibility criteria applicable to each required Witness class and Observation Scope.

### REQ-WIT-021

Claims of Witness independence MUST identify the applicable independence dimensions and the evidence supporting satisfied Control Domain separation.

### REQ-WIT-022

Unknown, missing, or conflicting independence relationships MUST NOT be silently treated as satisfied independence.

### REQ-WIT-023

Role combination between Witnesses and producers, Registries, Chain operators, Checkpoint Authorities, Preservation Services, or verifiers MUST be represented where it affects assurance.

### REQ-WIT-024

Applicable conflicts of interest and their effect on eligibility, weighting, or independence MUST be governed and historically evaluable.

### REQ-WIT-025

Every interoperable Witness Quorum claim MUST identify or resolve the applicable quorum policy and version.

### REQ-WIT-026

Quorum policy MUST define the eligible population or classes, target and scope compatibility, threshold, weighting, Control Domain, timing, conflict, and degradation rules required for evaluation.

### REQ-WIT-027

Count- or fraction-based quorum policies MUST define the denominator and treatment of suspended, revoked, unavailable, late, and newly added Witnesses.

### REQ-WIT-028

Weight-based quorum policies MUST define the source, historical applicability, normalization, and deterministic arithmetic of Witness weights.

### REQ-WIT-029

Control-Domain-based quorum policies MUST define required domain count, qualifying dimensions, historical membership evidence, and treatment of unknown or overlapping domains.

### REQ-WIT-030

Only observations satisfying the applicable target, scope, version, and validation compatibility rules MAY contribute to the same quorum result.

### REQ-WIT-031

Quorum timing evaluation MUST use the applicable historical time semantics and MUST surface missing, imprecise, late, or incompatible observation times.

### REQ-WIT-032

Dynamic Witness membership MUST be evaluated at the historical boundary specified by the quorum policy and MUST NOT be applied retroactively without explicit governed semantics.

### REQ-WIT-033

Quorum aggregators MUST bind contributing, excluded, rejected, late, duplicate, and conflicting observations together with the applied policy and threshold calculation.

### REQ-WIT-034

An aggregator assertion MUST NOT replace independent quorum evaluation unless a governed proof construction supports the same conclusion.

### REQ-WIT-035

Conflicting or dissenting Witness Observations MUST remain available to Verification and MUST be evaluated according to the applicable quorum policy.

### REQ-WIT-036

Non-response, timeout, unavailability, refusal, negative observation, and unavailable observation evidence MUST remain distinguishable where known.

### REQ-WIT-037

Graceful degradation rules MUST produce explicit evidence of missing Witness controls and MUST NOT represent the Intended Trust Profile as achieved when quorum requirements are unsatisfied.

### REQ-WIT-038

Witness gossip or comparison mechanisms MUST authenticate observations, bind compared state, preserve conflicts, prevent inappropriate replay, and bound resource consumption.

### REQ-WIT-039

Checkpoint or External Anchor evaluation MUST NOT infer Witness Quorum achievement unless the required Witness evidence and historical policy evaluation are available or validly committed.

### REQ-WIT-040

Unknown critical Witness Observation extensions MUST produce an unsupported or indeterminate outcome for affected claims.

### REQ-WIT-041

Witnessing implementations SHOULD minimize unnecessary disclosure of underlying content, identity, correlation, topology, timing, and quorum-composition information.

### REQ-WIT-042

Encrypted, redacted, or selectively disclosed Witness evidence MUST preserve the target and scope integrity required by the evaluated claim and MUST expose resulting Verification limitations.

### REQ-WIT-043

Implementations MUST bound Witness Observation size, collection cardinality, quorum membership, dependency depth, parsing effort, Signature operations, and comparison work.

### REQ-WIT-044

Preservation planning SHOULD include Witness Observations, target proofs, historical identities, keys, Registries, Observation Scope definitions, quorum policies, Control Domain evidence, conflicts, and corrections required for the Evidence Lifetime.

### REQ-WIT-045

Portable Witness evidence packages SHOULD identify included, omitted, redacted, unavailable, late, excluded, duplicate, and conflicting material required by the intended conclusion.

### REQ-WIT-046

Witness validation SHOULD distinguish availability, structural, semantic, canonicalization, Signature, target-binding, scope, eligibility, independence, timing, quorum, Completeness, and Trust Profile results.

### REQ-WIT-047

Historical Witness Verification MUST use applicable historical keys, algorithms, registries, scopes, quorum policies, Control Domain relationships, and Trust Profiles rather than current defaults.

### REQ-WIT-048

Verification Reports MUST bound Witness conclusions to the evaluated target, Observation Scope, Witness set, policy version, time window, evidence set, Trust Profile, and unresolved limitations.

### REQ-WIT-049

Independent conforming implementations SHOULD derive equivalent observation-validity, eligibility, independence, and quorum conclusions from equivalent evidence under equivalent Verification Contexts.

---

# Security Considerations

Witnessing creates an additional assurance layer but also introduces identity, key, availability, collusion, and aggregation attack surfaces.

Major threats include:

- forging a Witness identity or Observation;
- compromising or misusing a Witness signing key;
- using a valid key for an unauthorized Witness purpose;
- rewriting historical Witness Registry or eligibility state;
- creating Sybil Witnesses under one Control Domain;
- misrepresenting replicas as independent Witnesses;
- collusion among Witness operators;
- shared-source deception in which independent Witnesses receive the same false view;
- scope substitution or target digest substitution;
- replaying an observation into another Chain, time window, or quorum;
- manipulating Observation Time or freshness evidence;
- suppressing conflicting or negative observations;
- counting duplicate deliveries or multiple keys from one Witness;
- quorum aggregator omission or threshold manipulation;
- denial of service against selected Witnesses to alter quorum composition;
- retroactive membership or weighting changes;
- gossip amplification or unbounded comparison work;
- key compromise followed by false correction or revocation;
- privacy leakage through targets, timing, selection, or quorum topology;
- permanent loss of historical policies, Registry state, or Control Domain evidence.

Implementations should apply controls including:

- governed Witness identity and Key Purpose;
- Historical Key State and Registry preservation;
- deterministic canonicalization and exact target binding;
- versioned Observation Scope and result semantics;
- replay and freshness controls;
- substantive Control Domain analysis;
- Sybil-resistant eligibility and quorum rules;
- conflict-preserving storage and aggregation;
- authenticated, bounded gossip;
- member-level quorum evidence;
- append-only correction and revocation;
- explicit degradation and non-response handling;
- independent publication and Preservation where required;
- privacy-aware commitments and selective disclosure;
- layered, reproducible Verification.

Witness diversity reduces some failures only when the relevant control and observation paths are actually diverse.

Several honest Witnesses can still report the same false state if their only source is a malicious operator.

Several separately hosted Witnesses can still fail together if their software, keys, identity provider, or governance is shared.

Assurance claims must therefore describe the independence dimensions actually evaluated.

---

# Privacy Considerations

Witnessing may distribute accountability data beyond the Organization that produced or committed it.

That distribution can improve independent assurance while increasing disclosure and correlation risk.

Privacy analysis should consider:

- whether the Witness needs plaintext or only a commitment;
- whether the target commitment is resistant to guessing;
- whether a Witness can correlate the same Actor across workflows;
- whether Witness selection reveals transaction value or risk tier;
- whether timestamps reveal business activity;
- whether public observations expose Chain growth;
- whether Control Domain evidence reveals sensitive supplier or infrastructure relationships;
- whether quorum evidence can be selectively disclosed;
- whether retention and deletion rules remain enforceable across Witness operators;
- whether incident investigation requires broader disclosure than routine Verification.

The minimum necessary observation should be preferred.

Privacy protection must not depend solely upon an undocumented data format or private endpoint.

Encryption, access control, typed commitments, blinded constructions, selective disclosure, retention limits, and explicit Verification outcomes should operate together.

---

# Design Rationale

TrustAgentAI uses Witness Observations because a Hash Chain operated and presented by one party can remain internally consistent while showing different histories to different observers.

A Witness creates a separately attributable statement about defined protocol state.

The Observation Scope prevents that statement from expanding into claims the Witness never evaluated.

Exact target binding prevents a valid observation from being redirected to another object, Chain, range, or quorum.

Historical identity, key, and eligibility evaluation prevents current Registry state from rewriting who was authorized to Witness earlier.

Control Domain analysis prevents process count, replica count, or Signature count from becoming false evidence of independence.

Quorum rules compose multiple observations only after validating scope compatibility, target equality, eligibility, timing, independence, and conflicts.

Conflict preservation prevents a successful threshold from hiding evidence of equivocation or dissent.

Explicit non-response and graceful degradation preserve the difference between operational continuation and achieved assurance.

Checkpointing may later commit to Witness quorum evidence, but it does not replace Witness evaluation.

Portable and preserved observations allow future verifiers to reproduce the conclusion outside the original Witness network.

The stable objective is:

> **A verifier should be able to determine who observed which exact state, what the observer actually checked, whether the observer was eligible and independent, and whether the required quorum existed at the relevant time.**

---

# Summary

The Witness layer provides separately attributable observation of TrustAgentAI protocol state.

A conforming Witness model establishes a disciplined independent-observation boundary:

1. the Witness role remains distinct from producer, Registry, Chain, Checkpoint, Preservation, and Verification roles;
2. every Witness Observation has bounded Observation Scope;
3. every observation binds the exact target and result;
4. Witness identity, key, Key Purpose, operator, Organization, and Control Domain remain distinguishable;
5. Signature validity does not imply eligibility or independence;
6. eligibility and Registry state are evaluated historically;
7. direct, relayed, derived, and reconstructed observations remain distinct;
8. Observation Time remains separate from Commitment and Publication Time;
9. issuance, delivery, Commitment, quorum contribution, and Checkpointing remain distinct;
10. replay, duplicates, corrections, and conflicts remain visible;
11. independence is substantive and claim-specific;
12. multiple replicas within one Control Domain do not become independent Witnesses;
13. Witness Quorum is derived from governed target, scope, timing, threshold, and Control Domain rules;
14. historical membership and weighting are not rewritten by current state;
15. quorum aggregation preserves contributing, excluded, late, duplicate, and conflicting evidence;
16. non-response is not negative observation;
17. operational degradation produces explicit assurance downgrade;
18. gossip and publication may strengthen split-view detection without creating truth automatically;
19. Checkpoints and External Anchors remain separate assurance layers;
20. Witness evidence and interpretation dependencies remain portable and preservable;
21. privacy is protected through minimization and governed disclosure;
22. Verification reports bounded observation, independence, quorum, Completeness, and limitations separately.

The foundational Witness rule is:

> **Count observations only after proving what was observed, who observed it, when, under which scope, and from which genuinely distinct Control Domains.**

The broader architectural principle remains:

> **Proof, not logs.**
