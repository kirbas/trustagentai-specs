# Chapter 14 — Verification

> **Verification is a reproducible, claim-bounded evaluation of evidence under an explicit context—not a Boolean assertion that everything is valid, complete, true, or legally sufficient.**

## Purpose

This chapter defines the architectural specification for TrustAgentAI **Verification**, **Verification Engines (VEs)**, **Verification Contexts**, **Verification Dependencies (VDs)**, **Verification Outcomes**, and **Verification Reports (VRs)**.

Verification composes all preceding accountability layers into bounded protocol conclusions. It evaluates Protocol Objects, representations, identifiers, Signatures, Historical Key State, Authority, Policy, Hash Chains, Witnesses, Checkpoints, External Anchors, Preservation Evidence, Dispute Packs, Completeness, and Trust Profile achievement.

This chapter establishes:

- independent and claim-bounded Verification semantics;
- Verification Engine roles and trust boundaries;
- Verification Context construction and immutability;
- Verification Dependency Graph resolution;
- layered validation from safe parsing through protocol conclusion;
- historical identity, key, Authority, Policy, and time interpretation;
- Chain, Witness, Checkpoint, Anchor, Preservation, and Dispute Pack evaluation;
- Validity, Completeness, and Trust Profile separation;
- explicit outcomes for invalid, incomplete, indeterminate, unsupported, unavailable, and conflicting evidence;
- deterministic, reproducible, offline, and privacy-aware Verification;
- durable machine-readable and human-readable Verification Reports;
- Verification invariants and architectural requirements.

This chapter applies the common Protocol Object rules defined in [06-Protocol-Objects.md](06-Protocol-Objects.md).

It builds upon the Evidence Record model in [07-Evidence-Record-Specification.md](07-Evidence-Record-Specification.md), the Hash Chain model in [08-Hash-Chain-Specification.md](08-Hash-Chain-Specification.md), the Witness model in [09-Witness-Observation-Specification.md](09-Witness-Observation-Specification.md), the Checkpoint model in [10-Checkpoints-and-External-Anchors.md](10-Checkpoints-and-External-Anchors.md), the Historical Key State model in [11-Key-Transparency.md](11-Key-Transparency.md), the Preservation model in [12-Preservation.md](12-Preservation.md), and the Dispute Pack model in [13-Dispute-Packs.md](13-Dispute-Packs.md).

It also applies the system model in [05-System-Overview.md](05-System-Overview.md), the principles in [04-Design-Principles.md](04-Design-Principles.md), and the canonical terms in [Terminology.md](Terminology.md) and [Acronyms.md](Acronyms.md).

This chapter defines architectural semantics.

It does not require one Verification Engine, programming language, user interface, database, resolver, cryptographic library, scoring system, or deployment model. It does not define final field names, concrete schemas, wire encodings, every algorithm suite, court procedure, evidentiary admissibility, accounting treatment, or jurisdiction-specific legal conclusions.

Those details belong to TAIP, Trust Profiles, registries, cryptographic profiles, APIs, test vectors, conformance suites, and applicable governance.

---

# 14.1 Verification Definition

**Verification** is the governed evaluation of TrustAgentAI evidence and dependencies under an explicit Verification Context to determine which bounded protocol conclusions are supported.

Verification may evaluate:

- object structure and semantics;
- canonical representation and identifiers;
- cryptographic protections;
- historical identity, key, Authority, and Policy state;
- historical continuity and lifecycle state;
- independent observation and historical commitments;
- Preservation and dependency availability;
- evidence Completeness;
- Intended and Achieved Trust Profiles;
- individual Accountability Claims.

```text
Evidence
   +
Verification Dependencies
   +
Verification Context
   +
Governed Rules
   =
Bounded Verification Outcomes
```

---

# 14.2 Independent Verification

**Independent Verification** means that an authorized verifier can reproduce protocol conclusions without relying solely upon undocumented producer logic, mutable dashboards, privileged production access, or the producer's assertion that its own evidence is valid.

Independence may be strengthened by:

- open specifications;
- portable Protocol Objects and Dispute Packs;
- deterministic canonicalization;
- versioned registries and Trust Profiles;
- preserved Verification Dependencies;
- independently implemented Verification Engines;
- published test vectors;
- explicit resolver and trust inputs.

Independent does not mean trust-free. The verifier still evaluates specified trust roots, Authorities, registries, algorithms, and external evidence under the applicable context.

---

# 14.3 Scope

This chapter applies to Verification of:

- individual Protocol Objects;
- object collections and causal graphs;
- Signatures and Historical Key State;
- Authority and Policy evidence;
- Chain Entries, ranges, forks, and lifecycle events;
- Witness Observations and quorum;
- Checkpoints and External Anchors;
- Preservation Evidence and Migration Records;
- Dispute Packs and their Manifests;
- Verification Dependencies;
- Completeness and Trust Profile achievement;
- prior Verification Reports;
- protocol-level Accountability Claims.

Verification may be performed during operation, admission, monitoring, audit, dispute, migration, preservation testing, or long-term archival review.

---

# 14.4 Security and Assurance Objectives

Verification has six primary objectives:

1. apply the correct historical rules and dependencies to the correct evidence;
2. separate successful lower-layer checks from unsupported higher-layer conclusions;
3. expose missing, invalid, unavailable, unsupported, conflicting, or incomplete state;
4. produce deterministic conclusions under equivalent contexts;
5. prevent intended assurance from being reported as achieved without evidence;
6. preserve enough explanation for independent reproduction and challenge.

The architectural objective is:

> Another conforming verifier can determine what was checked, under which context, with which dependencies, which results followed, and why stronger conclusions were or were not supported.

---

# 14.5 What Verification Establishes

Subject to the available evidence and applicable context, Verification may establish that:

- an object conforms to supported structure and semantics;
- canonical identifiers and cryptographic protections verify;
- a Signature matches exact key material under supported algorithms;
- Historical Key State, Key Purpose, Authority, and Policy support a defined interpretation;
- evidence entered a verifiable Chain state;
- eligible Witnesses observed defined state and satisfied a quorum rule;
- Checkpoints or External Anchors verify within their proof scope;
- Preservation Evidence supports defined retention or availability claims;
- a Dispute Pack is valid and complete or incomplete for defined claims;
- an Intended Trust Profile was fully, partially, or not achieved;
- an Accountability Claim is supported, rejected, or unresolved at the protocol layer.

---

# 14.6 What Verification Does Not Establish

Successful protocol Verification does not automatically establish:

- truth of every external fact;
- accuracy of an unverified source assertion;
- lawful business Authority beyond evaluated evidence;
- fraud absence;
- economic correctness;
- accounting approval;
- contractual interpretation;
- evidentiary admissibility;
- Legal Validity;
- Regulatory Compliance;
- the only possible human conclusion.

```text
Successful Protocol Verification
≠
Business Truth
≠
Legal Validity
≠
Regulatory Compliance
```

Verification Reports must state their boundaries so protocol evidence is not overstated as a conclusion outside protocol scope.

---

# 14.7 Conceptual Roles

A Verification workflow may involve:

- a **Requesting Party** defining claims or review purpose;
- a **Verifier** selecting or approving the Verification Context;
- a **Verification Engine** executing deterministic checks;
- a **Resolver** retrieving dependencies;
- a **Trust Profile Authority** governing assurance requirements;
- an **Algorithm or Registry Authority** governing interpretation material;
- an **Evidence Custodian or Preservation Service** supplying evidence;
- a **Dispute Pack Assembler** packaging portable inputs;
- a **Reviewer, Auditor, Investigator, or Decision Maker** interpreting reports;
- an **Independent Monitor** comparing outcomes or detecting divergence.

One entity may perform several roles, but role combination and conflicts of interest must remain visible where assurance depends upon independence.

---

# 14.8 Verification Engine

A **Verification Engine (VE)** evaluates TrustAgentAI evidence according to explicit protocol, cryptographic, historical, Policy, and Trust Profile requirements.

Its responsibilities may include:

- safe parsing and version dispatch;
- canonicalization and identifier validation;
- cryptographic verification;
- dependency resolution;
- Historical Key State derivation;
- Authority and Policy evaluation;
- Chain, Witness, Checkpoint, and Anchor verification;
- Preservation and Dispute Pack evaluation;
- Completeness calculation;
- Trust Profile evaluation;
- outcome composition;
- Verification Report generation.

A Verification Engine must not define protocol meaning through undocumented implementation behavior. Products may differ in performance and presentation while preserving equivalent conclusions.

---

# 14.9 Verification Context

The **Verification Context** is the complete, explicit context under which evidence is evaluated.

It may include:

- context identifier and version;
- focal Accountability Claims;
- TAIP and Protocol Object versions;
- schemas and canonicalization rules;
- Intended Trust Profile;
- Policy and registry versions;
- Verification Time;
- historical event or state boundary;
- Historical Key State and resolver rules;
- cryptographic algorithm policy;
- trusted roots and Authorities;
- available, permitted, or prohibited dependency sources;
- privacy and disclosure constraints;
- resource and offline constraints;
- extension and compatibility behavior.

The full term should normally be used because `VC` may also mean Verifiable Credential.

---

# 14.10 Claims and Verification Scope

Verification must identify the exact claims and evidence scope being evaluated.

A claim specification should identify:

- claim identifier and type;
- subject and Accountable Action;
- relevant object, event, or state;
- historical boundary;
- expected Authority and Policy;
- required Trust Profile;
- evidence population or Dispute Pack;
- decision or review purpose where protocol-relevant;
- excluded questions.

```text
Valid Object
≠
Supported Accountability Claim
```

One evidence set may support one claim while remaining insufficient for another. Outcomes must remain claim-specific.

---

# 14.11 Verification Input Set

The Verification input set may contain:

- focal Protocol Objects;
- related causal and execution evidence;
- historical-integrity evidence;
- identity, key, Authority, and Policy evidence;
- Trust Profiles and registries;
- schemas and algorithms;
- Preservation Evidence;
- a Dispute Pack Manifest;
- prior Verification Reports;
- resolver snapshots and cached dependencies;
- explicit missing, redacted, or unavailable-state markers.

Inputs must retain identity, integrity, provenance, version, and disclosure state.

An undeclared runtime default, mutable local configuration, or hidden network response that affects the conclusion is part of the effective context and must not remain invisible.

---

# 14.12 Verification Dependency Graph

The **Verification Dependency Graph (VDG)** is the typed graph connecting evidence to the material required for its interpretation and Verification.

Dependencies may include:

- schemas and canonicalization rules;
- algorithm definitions and public keys;
- Historical Key State;
- Authority and Policy evidence;
- registries and Trust Profiles;
- Chain Entries and Witness Observations;
- Checkpoints and Anchor Evidence;
- Preservation Evidence;
- causal records;
- extension definitions;
- migration and renewal evidence.

The graph may contain cycles or shared nodes. Evaluation must terminate safely, reject circular proof that supplies no independent foundation, and preserve typed dependency semantics.

---

# 14.13 Dependency Resolution

Dependency resolution must distinguish:

- dependency identity;
- expected version;
- expected integrity;
- locator or resolver;
- resolver trust role;
- retrieved representation;
- retrieval time;
- embedded, cached, external, unavailable, redacted, unsupported, or conflicting state.

A resolver response is not accepted merely because a URL or database query succeeds.

The VE must validate identity, integrity, type, and applicability under the Verification Context.

Missing mandatory dependencies must not be replaced with current defaults or guessed values. Optional dependencies may affect warnings or alternate claims according to the Trust Profile.

---

# 14.14 Historical and Current State

Historical Verification must use the state applicable to the relevant boundary rather than silently substitute current configuration.

This applies to:

- Protocol Identity and keys;
- Key Purpose;
- Authority and delegation;
- Policy;
- registries and eligibility;
- Witness quorum rules;
- Checkpoint Authorities;
- Trust Profiles;
- algorithms and deprecation state;
- preservation and migration context.

```text
Historical State
≠
Current State
```

Current state may inform present risk or a separate claim, but it must not rewrite historical interpretation.

---

# 14.15 Time Semantics

Verification may encounter:

- Event Time;
- Record Time;
- Signature Time;
- Submission Time;
- Acceptance Time;
- Commitment Time;
- Observation Time;
- Checkpoint Time;
- Publication Time;
- migration or preservation time;
- Verification Time.

```text
Claimed Event Time
≠
Cryptographically Supported Commitment Time
≠
Independent Observation Time
≠
Verification Time
```

The VE must evaluate each time according to its type, source, precision, ordering evidence, clock assumptions, and Trust Profile. A producer timestamp is an assertion unless supported by stronger evidence.

---

# 14.16 Layered Verification Pipeline

Verification proceeds through ordered but composable layers:

```text
Availability and Safe Parsing
          ▼
Structural and Semantic Validation
          ▼
Identifier and Canonicalization Validation
          ▼
Cryptographic Validation
          ▼
Reference and Dependency Validation
          ▼
Historical and Lifecycle Validation
          ▼
Completeness and Trust Profile Evaluation
          ▼
Claim-Bounded Protocol Conclusion
```

A later layer may require several earlier results. Failure at one layer does not erase valid results from another, though it may prevent stronger conclusions.

---

# 14.17 Availability and Safe Parsing

Before semantic evaluation, the VE must determine whether required representations can be obtained and processed safely.

Checks may include:

- media type and binding support;
- byte and encoding validity;
- object size and depth limits;
- decompression and archive limits;
- duplicate-member behavior;
- path and special-file safety;
- parser ambiguity;
- prohibited active content;
- encryption and decryption availability;
- timeout and resource constraints.

`Unavailable`, `malformed`, `unsupported`, and `unsafe to process` are distinct states.

Unsafe input must fail closed without being converted into a semantic invalidity claim that was never evaluated.

---

# 14.18 Structural Validation

Structural validation determines whether an object conforms to the applicable type, envelope, schema, and mandatory-field rules.

It may evaluate:

- object type and namespace;
- supported version;
- required and prohibited properties;
- value representation and cardinality;
- extension placement;
- duplicate members;
- typed envelope and payload boundaries;
- schema references;
- package or batch structure.

Structural success means the object can be interpreted under the supported grammar.

It does not establish semantic correctness, cryptographic validity, Authority, historical Commitment, Completeness, or truth.

---

# 14.19 Semantic Validation

Semantic validation evaluates whether represented values, combinations, transitions, and relationships are permitted for the object type and context.

Examples include:

- valid enumerations and identifiers;
- coherent state transitions;
- permitted role and object combinations;
- time-order constraints;
- required reference types;
- bounded numeric and monetary semantics;
- extension criticality;
- mutually exclusive or conditional fields;
- correction, supersession, and revocation behavior;
- claim-specific meaning.

A structurally well-formed object may be semantically invalid.

Unknown mandatory semantics must produce `unsupported`, not guessed interpretation.

---

# 14.20 Identifier and Canonicalization Validation

This layer evaluates:

- identifier namespace, syntax, normalization, and collision behavior;
- relationship between stable identifiers and digests;
- canonicalization version;
- canonical field coverage;
- character, number, time, and binary encoding;
- absent, null, and default semantics;
- extension handling;
- domain separation;
- embedded and referenced content boundaries;
- expected and computed digest values.

The VE must derive the cryptographic input defined by the applicable historical rules.

Pretty-printed, transport, database, encrypted, or packaged representations must not be hashed as canonical input unless the binding explicitly defines them that way.

---

# 14.21 Cryptographic Validation

Cryptographic validation evaluates the mathematical and profile-specific validity of:

- digests and commitments;
- Signatures;
- message authentication codes;
- inclusion and consistency proofs;
- aggregate or threshold cryptography;
- encryption authenticity where applicable;
- renewal and migration protections;
- Checkpoint and Anchor commitments.

It must bind exact algorithms, parameters, keys, inputs, encodings, and domain contexts.

```text
Cryptographically Valid
≠
Historically Authorized
≠
Complete
```

An algorithm may verify mathematically while failing the applicable Trust Profile or deprecation policy.

---

# 14.22 Signature Verification

Signature Verification combines:

- exact canonical signed input;
- Signature bytes and encoding;
- algorithm and parameter validation;
- exact public-key material;
- signer and KID binding;
- protected object type and purpose;
- multi-Signature or threshold semantics;
- applicable cryptographic profile.

A valid Signature establishes only the bounded cryptographic result supported by the scheme and context.

It does not automatically prove who controlled the key, whether the key was authorized, whether the signer possessed business Authority, or whether the signed assertion is true.

The VE should report cryptographic failure separately from key-state, Key Purpose, Authority, and Policy failure.

---

# 14.23 Identity, Key Purpose, and Historical Key State

Historical Signature interpretation requires evaluation of:

- Protocol Identity;
- Key Identifier and exact key material;
- Key Purpose;
- activation, suspension, revocation, expiration, and supersession state;
- compromise and recovery evidence;
- effective historical boundary;
- Key Transparency Records and proofs;
- algorithm context;
- conflicts and uncertainty.

```text
Current Key State
≠
Historical Key State
```

A later revocation does not automatically invalidate every earlier Signature. The applicable historical state and compromise evidence determine the bounded conclusion.

---

# 14.24 Authority and Policy Validation

Authority and Policy validation determines whether the accountable subject or signer was permitted to perform the action represented by the evidence.

It may evaluate:

- Authority source and type;
- delegation chain;
- scope, limits, and conditions;
- effective interval;
- revocation or supersession;
- required human or organizational approval;
- Policy identifier and version;
- input facts and exceptions;
- quorum or separation-of-duty rules;
- evidence that Policy was evaluated.

```text
Valid Signature
≠
Valid Authority
≠
Policy Compliance
```

Circular delegation or Authority depending solely upon the action it purports to authorize must be rejected.

---

# 14.25 Reference and Dependency Validation

Every accountability-critical reference must be evaluated according to its type and integrity strength.

The VE should determine:

- whether the target is required;
- whether it is available;
- whether type and version match;
- whether identifier and integrity binding verify;
- whether the target is applicable at the historical boundary;
- whether resolver trust is acceptable;
- whether competing targets exist;
- whether redaction or access restrictions prevent evaluation;
- whether cycles or recursion limits were reached.

A valid referring object does not make an invalid or missing dependency valid.

Dependency failures must propagate only to the checks and claims that actually rely upon them.

---

# 14.26 Hash Chain and Historical Continuity

Hash Chain Verification may evaluate:

- Chain Identifier and version;
- Chain Entry structure and protected input;
- sequence position and predecessor commitment;
- entry and payload binding;
- genesis, fork, merge, closure, and migration semantics;
- gap, truncation, rollback, or duplicate state;
- Chain Head and range proofs;
- operator identity and Historical Key State;
- relevant Checkpoints and external commitments.

A valid Chain segment does not prove global Completeness or absence of another fork unless supported by the applicable evidence.

Verification must identify the exact Chain range and historical conclusion established.

---

# 14.27 Witness and Quorum Validation

Witness Verification may evaluate:

- Witness Observation type and scope;
- observed target and digest;
- Witness identity and Historical Key State;
- eligibility at Observation Time;
- independence criteria;
- conflict and disclosure state;
- quorum rule and version;
- distinct Control Domains;
- correlated-failure limits;
- aggregation and proof semantics.

Signature count is not quorum.

The VE must not infer that a Witness inspected underlying business truth unless its Observation Scope explicitly supports that conclusion.

Valid observations that do not satisfy the required eligible quorum remain valid individual evidence but do not achieve the quorum control.

---

# 14.28 Checkpoint and External Anchor Validation

Checkpoint Verification may evaluate:

- exact target scope;
- commitment construction;
- Checkpoint Authority and Historical Key State;
- predecessor and cadence rules;
- Witness or quorum dependencies;
- conflict, correction, and succession;
- availability of committed state.

External Anchor Verification may additionally evaluate:

- external namespace and record identity;
- exact anchored commitment;
- publication, confirmation, and finality state;
- trust-boundary independence;
- rollback or reorganization;
- Anchor Evidence and retrieval proof.

A valid external record does not validate the underlying TrustAgentAI content.

---

# 14.29 Preservation Validation

Preservation Verification may evaluate:

- Preservation Target and Manifest;
- Preservation Service and Authority;
- retention and Legal Hold state;
- WORM or object-lock semantics;
- integrity and fixity checks;
- custody and access evidence;
- encryption and decryptability;
- replication, independence, and availability;
- restoration and recovery tests;
- migration, renewal, disposition, and erasure;
- Verification Dependency availability.

```text
Stored
≠
Preserved
```

The VE must distinguish intended retention configuration from Preservation Evidence that supports achieved controls.

---

# 14.30 Dispute Pack Validation

Dispute Pack Verification may evaluate:

- container safety;
- DPM structure and cryptographic protection;
- Pack identity and version;
- focal claims and expected Verification Context;
- entry inventory and integrity;
- canonical versus derived or redacted representations;
- embedded and external dependencies;
- omissions, unavailable material, and conflicts;
- package Completeness;
- encryption, compartments, delivery, and custody.

A valid Pack does not make included evidence valid.

The Pack's recommended Verification Context may be accepted, modified, or rejected by the verifier, but any difference must remain explicit in the report.

---

# 14.31 Lifecycle-State Validation

Protocol lifecycle states must be validated separately rather than collapsed into one `valid` flag.

Relevant states may include:

- draft and finalized;
- signed;
- submitted and accepted;
- committed;
- witnessed;
- checkpointed;
- externally anchored;
- preserved;
- corrected or superseded;
- revoked;
- migrated;
- verified.

```text
Signed
≠
Accepted
≠
Committed
≠
Witnessed
≠
Preserved
```

The VE must require the evidence appropriate to each claimed state and must not infer a later state from an earlier operational event.

---

# 14.32 Causal and Execution Validation

An Accountability Claim may depend upon the relationship among intent, Authority, Policy evaluation, decision, execution request, external action, result, correction, and later observation.

The VE may evaluate:

- typed causal references;
- input and output integrity;
- action and result identifiers;
- sequence and time relationships;
- approval and exception evidence;
- execution-system attestations;
- counterparty confirmations;
- retries, reversals, and failures;
- missing or conflicting branches;
- claim-specific causation limits.

A cryptographically valid intent does not prove execution. An execution receipt does not prove that the action was authorized or that every external effect occurred as claimed.

---

# 14.33 Evidence Completeness

**Evidence Completeness** is the degree to which the available evidence and dependencies satisfy the requirements for defined claims, population, historical boundary, Verification Context, and Trust Profile.

```text
Object Validity
≠
Evidence Completeness
≠
Trust Profile Achievement
```

Completeness evaluation may consider:

- mandatory object types;
- causal and dependency closure;
- Chain range or sequence coverage;
- known omissions and redactions;
- unavailable or unresolved material;
- conflicting evidence;
- trusted population and selection rules;
- required external state;
- profile-specific controls.

Completeness is never universal without a bounded definition.

---

# 14.34 Intended and Achieved Trust Profiles

The **Intended Trust Profile** is the assurance profile selected or required for the action or evidence.

The **Achieved Trust Profile** is the profile actually supported by verified controls and available evidence.

```text
Intended Trust Profile
≠
Achieved Trust Profile
```

The VE must evaluate each mandatory control, dependency, independence criterion, threshold, and exception.

A lower profile may be achieved when a higher intended profile fails, but the downgrade, unsatisfied controls, and effect on each claim must remain explicit. A deployment configuration or service label is not proof of achievement.

---

# 14.35 Result Composition and Dependency Propagation

Verification results form a dependency graph.

For example:

```text
Signature Result
      ├── Canonicalization Result
      ├── Algorithm Result
      ├── Key Material Result
      └── Historical Key State Result
```

A parent conclusion must not succeed when a mandatory child result is invalid, unsupported, unavailable, indeterminate, or conflicting under the applicable rule.

Failures should propagate only to dependent checks. An unavailable optional presentation view need not invalidate canonical object integrity. A missing mandatory schema may block all semantic conclusions based upon that object.

The composition rule must be deterministic and explainable.

---

# 14.36 Verification Outcome Taxonomy

TAIP should define a structured outcome taxonomy supporting at least:

- `valid`;
- `invalid`;
- `incomplete`;
- `indeterminate`;
- `unsupported`;
- `unavailable`;
- `conflicting`;
- `valid-with-warnings` where permitted;
- `not-evaluated`;
- profile-specific nonconformance states.

Outcomes may apply to individual checks, dependencies, objects, claims, and overall profile achievement.

One overall value must not erase the underlying result graph.

---

# 14.37 Valid Outcome

`Valid` means that the evaluated target satisfied all mandatory rules for the exact check or claim under the stated Verification Context.

Validity must identify:

- target;
- check or claim;
- applicable rules and versions;
- required dependencies;
- historical boundary;
- Trust Profile where relevant;
- supported conclusion;
- limitations that do not negate validity.

`Valid` at one layer does not imply Validity at every higher layer.

For example, a valid Signature may accompany an invalid Authority result, and a valid object may exist within an incomplete evidence set.

---

# 14.38 Invalid Outcome

`Invalid` means that available, understood evidence contradicts or fails a mandatory rule for the evaluated target.

Examples include:

- schema violation;
- identifier or digest mismatch;
- bad Signature;
- prohibited state transition;
- wrong Key Purpose;
- Authority absent where evidence proves it was required and not present;
- broken Chain link;
- ineligible Witness counted toward quorum;
- Checkpoint target mismatch;
- conflicting value where exclusivity is required.

Invalid must not be used merely because a dependency is unavailable or an algorithm is unsupported. Those conditions have distinct outcomes.

---

# 14.39 Incomplete Outcome

`Incomplete` means known evidence or dependencies required for the defined claim, context, or profile are absent, redacted, unavailable, invalid for inclusion, or otherwise insufficient.

Examples include:

- missing Authority evidence;
- missing Historical Key State;
- absent required Chain range;
- insufficient eligible Witness quorum;
- unavailable mandatory Checkpoint;
- omitted schema in a Dispute Pack;
- missing Preservation Evidence;
- redacted required field.

Incomplete does not mean every available object is invalid.

The VE should identify the missing requirement and which stronger conclusions it prevents.

---

# 14.40 Indeterminate Outcome

`Indeterminate` means the VE understands the applicable question but available evidence does not support a reliable Valid or Invalid conclusion.

Examples include:

- uncertain compromise boundary;
- unresolved conflict between credible histories;
- ambiguous event ordering;
- evidence of unknown completeness within an open population;
- unavailable external state with no governed default;
- insufficient time evidence;
- disputed identity binding without decisive proof.

Indeterminate preserves uncertainty. It must not be converted into Valid through optimistic assumptions or into Invalid merely because certainty is unavailable.

---

# 14.41 Unsupported Outcome

`Unsupported` means the VE cannot correctly interpret or execute a required semantic, object type, version, extension, algorithm, proof, or Trust Profile rule.

Examples include:

- unknown mandatory extension;
- newer incompatible object version;
- unavailable cryptographic implementation;
- unrecognized canonicalization method;
- unsupported resolver or proof semantics;
- unknown Trust Profile control.

```text
Understood
≠
Verified
```

Ignoring unsupported mandatory meaning and continuing as Valid is prohibited.

---

# 14.42 Unavailable Outcome

`Unavailable` means identified evidence, dependency, key, service, or representation cannot be obtained or accessed for the evaluated operation.

Reasons may include:

- network or provider failure;
- access denial;
- deleted or corrupted evidence;
- missing decryption key;
- offline restriction;
- unresolved locator;
- legal or Policy restriction;
- Preservation failure;
- timeout or resource limit.

Unavailable is not equivalent to invalid, nonexistent, omitted, or unsupported.

The VE must record the affected dependency and propagate the effect to dependent checks.

---

# 14.43 Conflicting Outcome

`Conflicting` means available evidence supports incompatible states, histories, identities, values, or conclusions that cannot be resolved by an applicable deterministic rule.

Examples include:

- two protected objects sharing one identifier with different content;
- competing Chain forks;
- inconsistent KT Checkpoints;
- contradictory Authority records;
- conflicting Witness Observations;
- incompatible external anchor states;
- multiple exclusive lifecycle successors;
- different registry snapshots claiming the same boundary.

The VE must preserve and identify the competing evidence. It must not select a branch through filename, input order, resolver preference, or undocumented heuristic.

---

# 14.44 Warnings and Nonconformance

A warning identifies a material condition that does not fail the evaluated mandatory rule but may affect interpretation, future validity, risk, or recommended action.

Examples include:

- algorithm approaching deprecation;
- optional dependency unavailable;
- stale but permitted resolver snapshot;
- reduced independence above the minimum threshold;
- untested recovery outside the claim scope;
- privacy-sensitive overcollection;
- noncanonical presentation metadata;
- upcoming retention expiry.

Warnings must not be used to downgrade a mandatory failure into success.

Conformance and security recommendations may be reported separately from protocol Validity when the distinction is explicit.

---

# 14.45 Per-Claim and Aggregate Outcomes

One Verification run may evaluate multiple claims and objects.

The report should preserve:

- per-check results;
- per-dependency results;
- per-object results;
- per-claim results;
- Completeness results;
- control-level Trust Profile results;
- aggregate run status.

Aggregate rules must be defined by TAIP or the Trust Profile.

An aggregate `invalid` must not hide that some claims were valid. An aggregate `valid` must not hide an incomplete mandatory subclaim. Consumers should use the most specific outcome relevant to their decision.

---

# 14.46 Scores, Confidence, and Decision Thresholds

An implementation may present scores, confidence measures, or decision aids, but they must not replace canonical outcome semantics.

Any score affecting a protocol conclusion must identify:

- inputs and weights;
- versioned calculation method;
- missing-data behavior;
- relationship to mandatory controls;
- threshold and Policy;
- uncertainty representation;
- claim and Trust Profile scope;
- human-readable explanation.

```text
Opaque Score
≠
Verification Outcome
```

A high score must not override a mandatory invalid, unsupported, or conflicting result. Heuristic business-risk scoring belongs outside protocol Verification unless TAIP explicitly governs it.

---

# 14.47 Determinism and Reproducibility

Equivalent evidence evaluated under equivalent Verification Contexts should produce equivalent protocol conclusions across conforming engines.

Determinism requires explicit:

- object and schema versions;
- canonicalization and algorithm rules;
- registry and Trust Profile snapshots;
- historical boundary and Verification Time;
- resolver inputs and conflict behavior;
- dependency availability state;
- numeric, time, ordering, and comparison semantics;
- extension and unsupported behavior;
- outcome composition rules;
- resource-limit semantics where they affect results.

Human-readable wording may differ. The canonical machine-readable conclusions and supporting result graph should remain equivalent.

---

# 14.48 Resolver Snapshots and External State

Mutable external state can make Verification irreproducible unless the effective resolver state is recorded.

A resolver snapshot or retrieval record may identify:

- resolver identity and version;
- query and namespace;
- requested historical boundary;
- returned object and integrity;
- retrieval time;
- cache and freshness state;
- authoritative or mirror role;
- conflicts with other sources;
- failure and fallback behavior.

Current DNS, current certificate status, current Registry content, or current Policy must not silently replace the historical state selected by the context.

The VE should retain enough resolver evidence to explain material conclusions.

---

# 14.49 Offline Verification

Offline Verification evaluates a defined evidence set without network retrieval during the run.

An offline profile should identify:

- embedded Protocol Objects and dependencies;
- permitted preinstalled specifications and algorithms;
- local trust roots and registry snapshots;
- decryption-key delivery;
- Verification Time behavior;
- freshness limitations;
- checks requiring current external state and therefore not performed;
- expected Completeness and Trust Profile consequences.

Offline Verification improves resilience, privacy, and independence from live services.

It does not prove absence of later revocation, correction, conflict, or external evidence unless the claim intentionally fixes an earlier historical cutoff.

---

# 14.50 Caching, Replay, and Reverification

Verification may cache parsed objects, resolved dependencies, proof results, or earlier check outcomes.

Cache validity must consider:

- target identity and integrity;
- context identifier;
- Verification Time;
- resolver and registry snapshot;
- algorithm and Trust Profile versions;
- dependency availability;
- revocation, correction, or migration evidence;
- negative-result expiry;
- software and rule version.

A cached result from another context must not be replayed as current Verification.

Reverification may be required when new evidence appears, algorithms change, dependencies become available, a Pack is superseded, or a Trust Profile changes.

---

# 14.51 Verification Report Definition

A **Verification Report (VR)** is a durable Protocol Object representing results obtained from evaluating evidence under a defined Verification Context.

A VR may contain:

- report identifier and version;
- verifier and VE identity;
- Verification Time;
- evidence-set or Dispute Pack commitment;
- Verification Context;
- claims evaluated;
- checks performed;
- result dependency graph;
- missing, unavailable, unsupported, redacted, and conflicting material;
- Completeness results;
- Intended and Achieved Trust Profiles;
- warnings and limitations;
- overall bounded outcomes;
- cryptographic protection.

A VR records a conclusion reached at one boundary. It is not timeless truth.

---

# 14.52 Report Identity, Version, and Protection

Verification Report identity rules must define:

- namespace and stable identifier;
- report version;
- relationship to content digest;
- issuer or verifier PID;
- correction and supersession behavior;
- collision handling;
- comparison semantics.

Cryptographic protection must cover every property whose alteration could change evidence scope, context, checks, dependencies, outcome, Completeness, profile achievement, or limitation.

```text
Verification Report Identifier
≠
Run Identifier
≠
Report Digest
```

A signed report establishes responsibility for the represented result, subject to Historical Key State and Authority. It does not make the underlying evidence valid automatically.

---

# 14.53 Report Content and Evidence Binding

A reproducible VR should bind to:

- exact evidence and package versions;
- canonical or aggregate integrity values;
- embedded and external dependencies used;
- resolver snapshots;
- schemas, registries, algorithms, Policies, and Trust Profiles;
- VE software and rule versions where relevant;
- checks performed and not performed;
- inputs to any governed score;
- per-layer and per-claim outcomes;
- report generation and Signature context.

References must use sufficient integrity strength for the claim.

A report pointing only to mutable URLs, local database rows, or a dashboard session is not independently reproducible.

---

# 14.54 Later Evidence and Report Succession

Later evidence may change a Verification conclusion without making the earlier report fraudulent or malformed.

Examples include:

- newly discovered compromise;
- a later correction or revocation;
- recovered missing dependencies;
- a conflicting Chain or KT view;
- changed algorithm policy;
- a superseding Dispute Pack;
- improved completeness evidence.

A later report should identify the earlier report where relevant, new evidence, context changes, and changed conclusions.

The earlier report remains part of accountable history and must not be silently rewritten.

---

# 14.55 Machine and Human Interpretation

Verification outputs should support both deterministic machine processing and understandable human review.

The machine-readable layer should provide:

- stable outcome codes;
- target and claim identifiers;
- structured check and dependency graphs;
- versioned rule references;
- precise failure and absence reasons;
- Trust Profile control results.

The human-readable layer should explain:

- what was evaluated;
- the supported conclusion;
- material evidence and limitations;
- why stronger claims failed;
- relevant next actions.

Human wording must not contradict or override canonical machine-readable outcomes.

---

# 14.56 Privacy and Minimum Disclosure

Verification may reveal sensitive evidence, identity relationships, access patterns, compromise history, business Policy, and dispute scope.

The VE should support:

- claim-bounded evidence access;
- selective disclosure;
- compartmentalized Dispute Packs;
- local or offline resolution;
- minimized report contents;
- protected resolver queries;
- confidential error detail;
- separation of public outcome from restricted supporting evidence;
- governed retention of temporary data and reports.

Privacy controls must not silently convert unavailable mandatory evidence into Validity or Completeness.

The report should expose the verification effect of redaction without unnecessarily revealing protected values.

---

# 14.57 Performance and Safe Resource Limits

Verification must remain safe under malformed, adversarial, or very large evidence sets.

Implementations should bound:

- object and package size;
- entry count;
- decompression expansion;
- nesting and reference depth;
- graph nodes and edges;
- cryptographic operation count;
- algorithm cost;
- resolver requests;
- memory, disk, CPU, and wall time;
- generated report size.

Limits affecting a result are part of the Verification Context or implementation evidence.

Reaching a limit should produce an explicit unavailable, unsupported, incomplete, or indeterminate result as defined by TAIP, not partial success disguised as full Verification.

---

# 14.58 Conformance, Interoperability, and Test Vectors

Verification reproducibility requires conformance material covering:

- canonicalization;
- identifiers and digests;
- valid and invalid Signatures;
- Historical Key State;
- Authority and Policy cases;
- Hash Chain forks and gaps;
- Witness eligibility and quorum;
- Checkpoints and Anchors;
- Preservation and Dispute Pack cases;
- missing, unsupported, unavailable, and conflicting dependencies;
- outcome composition;
- Verification Report serialization.

Independent engines should be tested against positive, negative, boundary, adversarial, and cross-version vectors.

Conformance to a subset must not be represented as support for unimplemented mandatory semantics.

---

# 14.59 Audit, Legal, and Business Boundaries

Verification can support auditors, investigators, counterparties, regulators, insurers, and legal reviewers with reproducible technical evidence.

The protocol conclusion may concern:

- object integrity;
- historical state;
- Signatures and keys;
- Authority evidence;
- Chain continuity;
- Witness and Checkpoint controls;
- Preservation;
- Completeness;
- Trust Profile achievement.

Human decision makers determine how those results affect contracts, accounting, risk, admissibility, remedies, regulation, or factual judgment.

A VR must not label a protocol result as `legally valid`, `compliant`, `fraud-free`, or `business true` unless a separately governed layer explicitly performs and qualifies that analysis.

---

# 14.60 Anti-Patterns and Relationship to Other Controls

The following are architectural anti-patterns:

## Boolean-Only Verification

Collapsing invalid, incomplete, indeterminate, unsupported, unavailable, conflicting, and not-evaluated state into one `false` or, worse, one `true`.

## Signature Equals Authorization

Treating cryptographic Signature validity as business Authority or Policy compliance.

## Current-State Substitution

Using current keys, registries, Policy, or Trust Profiles to interpret every historical action.

## Valid Equals Complete

Treating individually valid objects as a complete evidence set.

## Configured Equals Achieved

Reporting the Intended Trust Profile from configuration without verifying its mandatory controls.

## Silent Downgrade

Falling to a weaker control set while still reporting the intended assurance level.

## Unsupported Equals Ignored

Skipping unknown mandatory semantics and continuing as Valid.

## Unavailable Equals Invalid

Claiming evidence failed a rule that could not be evaluated because a dependency was inaccessible.

## Opaque Score

Replacing deterministic outcomes with an unexplained confidence or trust number.

## Hidden Resolver State

Allowing mutable external lookups to affect conclusions without recording versions, integrity, or retrieval evidence.

## Producer-Only Verification

Requiring proprietary application logic or privileged database access for core protocol conclusions.

## Prior Report Equals Present Truth

Reusing an earlier result despite changed evidence, context, algorithms, or dependencies.

## Human Narrative Overrides Machine Result

Publishing an executive summary that contradicts the canonical structured outcomes.

## Resource-Limit Truncation

Stopping checks silently and reporting success after input or dependency limits are reached.

## Protocol Equals Legal Judgment

Representing technical Verification as automatic admissibility, Compliance, or business truth.

Verification composes all accountability controls:

```text
Protocol Objects          provide typed evidence
Hash Chains               provide ordered historical continuity
Witnesses                 provide independent observation
Checkpoints and Anchors   provide historical commitments
Key Transparency          provides historical signer context
Preservation              maintains evidence and dependencies
Dispute Packs             provide portable bounded inputs
Verification              derives explicit protocol conclusions
Trust Profiles            define required assurance controls
```

Verification evaluates these controls. It does not replace them.

---

# Verification Invariants

### INV-VER-001 — Explicit Context

Every material Verification conclusion MUST be bounded to an explicit Verification Context.

### INV-VER-002 — Claim Boundedness

Verification outcomes MUST identify the checks, targets, claims, and historical boundaries to which they apply.

### INV-VER-003 — Deterministic Interpretation

Equivalent evidence under equivalent Verification Contexts MUST produce equivalent canonical protocol conclusions.

### INV-VER-004 — Validity/Completeness Separation

Evidence Validity MUST remain distinguishable from evidence Completeness.

### INV-VER-005 — Intended/Achieved Separation

Intended Trust Profile MUST remain distinguishable from Achieved Trust Profile.

### INV-VER-006 — Protocol/Business Separation

Successful protocol Verification MUST NOT automatically establish business truth, Legal Validity, accounting approval, or Regulatory Compliance.

### INV-VER-007 — Layer Separation

Availability, structure, semantics, identifiers, cryptography, dependencies, history, lifecycle, Completeness, and profile results MUST remain separately evaluable.

### INV-VER-008 — Lower/Higher-Layer Separation

Success at one Verification layer MUST NOT imply success at a dependent higher layer.

### INV-VER-009 — Signature/Authority Separation

A valid Signature MUST NOT by itself establish Key Purpose, Historical Key State, Authority, Policy compliance, or claim truth.

### INV-VER-010 — Historical/Current Separation

Current identity, key, Authority, Policy, Registry, algorithm, or Trust Profile state MUST NOT silently replace required historical state.

### INV-VER-011 — Time-Dimension Separation

Event, Record, Signature, Submission, Acceptance, Commitment, Observation, Checkpoint, Publication, and Verification times MUST remain distinct where their semantics affect conclusions.

### INV-VER-012 — Object/History Separation

Object Integrity MUST remain distinguishable from Historical Integrity and historical Commitment.

### INV-VER-013 — Observation/Truth Separation

Valid Witness Observation MUST NOT be interpreted beyond its declared Observation Scope.

### INV-VER-014 — Count/Quorum Separation

Signature or Witness count MUST NOT substitute for eligibility, independence, or quorum evaluation.

### INV-VER-015 — Stored/Preserved Separation

Stored or retrievable evidence MUST NOT automatically be represented as Preserved across its required Evidence Lifetime.

### INV-VER-016 — Package/Evidence Separation

A valid Dispute Pack or Manifest MUST NOT make included evidence valid or complete automatically.

### INV-VER-017 — Dependency Explicitness

Mandatory dependencies and their resolution states MUST remain explicit in the result graph.

### INV-VER-018 — Absence-State Separation

Invalid, incomplete, indeterminate, unsupported, unavailable, conflicting, redacted, omitted, and not-evaluated states MUST remain distinguishable.

### INV-VER-019 — Unsupported Fail-Safe

Unsupported mandatory semantics MUST NOT be ignored or treated as Valid.

### INV-VER-020 — Uncertainty Preservation

Indeterminate or conflicting evidence MUST NOT be converted into certainty through undocumented defaults or heuristics.

### INV-VER-021 — Conflict Visibility

Competing evidence MUST be preserved and identified unless a governed deterministic resolution rule validly applies.

### INV-VER-022 — Dependency Propagation

A mandatory failed dependency MUST prevent only the conclusions that depend upon it and MUST NOT erase unrelated valid results.

### INV-VER-023 — No Silent Downgrade

Failure of Intended Trust Profile controls MUST remain visible in Achieved Trust Profile and claim outcomes.

### INV-VER-024 — No Opaque Override

An opaque score, confidence value, human narrative, or implementation preference MUST NOT override canonical mandatory outcomes.

### INV-VER-025 — Resolver Accountability

External resolver state materially affecting a conclusion MUST be identifiable, versioned or snapshotted, and integrity-evaluated as required.

### INV-VER-026 — Safe Processing

Malformed, hostile, unsupported, or resource-exhausting evidence MUST fail safely without manufacturing partial success.

### INV-VER-027 — Offline Boundary

Offline Verification MUST state the external or current-state checks it cannot perform and MUST preserve resulting limitations.

### INV-VER-028 — Report Context Binding

Every Verification Report MUST bind to the evidence, context, checks, dependencies, and outcomes it records.

### INV-VER-029 — Report Non-Rewrite

Later evidence or changed context MUST create a new Verification Report rather than silently rewrite an earlier report.

### INV-VER-030 — Machine/Human Consistency

Human-readable report content MUST NOT contradict canonical machine-readable outcomes.

### INV-VER-031 — Privacy Without False Success

Redaction, selective disclosure, or access restriction MUST NOT silently satisfy a mandatory Verification requirement.

### INV-VER-032 — Implementation Neutrality

No single Verification Engine's undocumented behavior MUST become the protocol definition.

---

# Architectural Requirements

### REQ-VER-001

TAIP MUST define versioned Verification Context, check, dependency, outcome, and Verification Report semantics.

### REQ-VER-002

Every Verification run MUST identify its Verification Context directly or through an immutable governed reference.

### REQ-VER-003

The Verification Context MUST identify focal claims, historical boundary, applicable protocol versions, and Intended Trust Profile where relevant.

### REQ-VER-004

The Verification Context MUST identify or bind to applicable schemas, canonicalization rules, algorithms, registries, Policies, trust roots, and resolver rules.

### REQ-VER-005

Verification Time and any distinct historical evaluation boundary MUST be represented explicitly.

### REQ-VER-006

The effective input set MUST identify focal evidence, supporting evidence, dependencies, disclosure state, and provenance sufficient for the claims.

### REQ-VER-007

Runtime defaults or external state that materially affect a conclusion MUST be included in or bound to the effective Verification Context.

### REQ-VER-008

The VE MUST construct or evaluate a typed Verification Dependency Graph for mandatory dependencies.

### REQ-VER-009

Dependency traversal MUST detect cycles, reject circular proof without an independent foundation, terminate safely, and enforce governed resource bounds.

### REQ-VER-010

Dependency resolution MUST validate identity, type, version, integrity, applicability, and resolver trust according to the context.

### REQ-VER-011

Missing mandatory dependencies MUST NOT be substituted with current defaults, guessed values, or unrelated cached state.

### REQ-VER-012

Verification MUST use historical identity, key, Authority, Policy, Registry, profile, and algorithm state when the claim requires historical interpretation.

### REQ-VER-013

Time claims MUST be evaluated according to their declared semantic type, source, precision, ordering evidence, and clock assumptions.

### REQ-VER-014

The VE MUST distinguish unavailable, malformed, unsupported, unsafe, and resource-limited input before semantic conclusions are produced.

### REQ-VER-015

Structural validation MUST evaluate applicable object type, namespace, version, schema, required fields, duplicate members, and extension placement.

### REQ-VER-016

Semantic validation MUST evaluate permitted values, combinations, transitions, relationships, conditional rules, and critical extensions.

### REQ-VER-017

Unknown mandatory object, field, extension, relationship, or transition semantics MUST produce an Unsupported outcome.

### REQ-VER-018

Identifier and canonicalization validation MUST apply the governed historical namespace, normalization, canonical input, digest, and domain-separation rules.

### REQ-VER-019

Transport, storage, presentation, encrypted, and package representations MUST NOT replace canonical cryptographic input unless the applicable binding defines them as such.

### REQ-VER-020

Cryptographic validation MUST bind exact algorithms, parameters, keys, inputs, encodings, purposes, and cryptographic-profile versions.

### REQ-VER-021

Signature results MUST distinguish mathematical validity from Historical Key State, Key Purpose, Authority, and Policy results.

### REQ-VER-022

Historical Signature Verification MUST evaluate exact public-key material and applicable Key Transparency evidence at the relevant boundary.

### REQ-VER-023

Authority validation MUST evaluate source, delegation chain, scope, effective interval, limits, revocation, and applicable Policy where required.

### REQ-VER-024

Circular Authority or delegation that supplies no independent authorization basis MUST be rejected.

### REQ-VER-025

Reference validation MUST identify required status, target type, version, identity, integrity, applicability, availability, and conflict state.

### REQ-VER-026

Hash Chain Verification MUST identify the exact Chain, range, positions, predecessor relationships, and historical conclusion evaluated.

### REQ-VER-027

Fork, gap, truncation, rollback, duplicate, merge, and migration states MUST remain explicit in Chain outcomes.

### REQ-VER-028

Witness Verification MUST evaluate Observation Scope, target, Witness identity, Historical Key State, eligibility, independence, and conflict state.

### REQ-VER-029

Witness Quorum Verification MUST apply the exact historical quorum rule and MUST NOT use raw Signature count as a substitute.

### REQ-VER-030

Checkpoint Verification MUST validate exact target scope, commitment, Authority, predecessor, cadence, conflict, and required dependency semantics.

### REQ-VER-031

External Anchor Verification MUST validate exact anchored commitment, external namespace, publication state, finality, retrieval proof, and trust-boundary assumptions.

### REQ-VER-032

Preservation Verification MUST distinguish retention, immutability, integrity, custody, availability, recoverability, migration, and dependency-preservation claims.

### REQ-VER-033

Dispute Pack Verification MUST distinguish container, DPM, entry integrity, dependency, omission, redaction, Completeness, and source-evidence results.

### REQ-VER-034

Lifecycle Verification MUST require the governed evidence for each claimed finalized, signed, submitted, accepted, committed, witnessed, checkpointed, anchored, preserved, corrected, revoked, migrated, or verified state.

### REQ-VER-035

Causal and execution validation MUST preserve the distinction between intent, authorization, decision, request, execution, result, correction, and reversal evidence.

### REQ-VER-036

Completeness evaluation MUST be bounded to defined claims, population, historical boundary, Verification Context, and Trust Profile.

### REQ-VER-037

Object Validity, evidence Completeness, and Trust Profile achievement MUST be reported separately.

### REQ-VER-038

The VE MUST evaluate every mandatory control of the Intended Trust Profile using actual evidence and satisfied conditions.

### REQ-VER-039

The Achieved Trust Profile MUST identify satisfied, failed, incomplete, unsupported, unavailable, conflicting, and not-evaluated controls.

### REQ-VER-040

Any profile downgrade MUST identify the intended profile, achieved profile, failed controls, and effect on claims.

### REQ-VER-041

TAIP MUST define outcome codes and composition rules for Valid, Invalid, Incomplete, Indeterminate, Unsupported, Unavailable, Conflicting, warning, and Not Evaluated states.

### REQ-VER-042

Invalid MUST be used only when understood available evidence fails or contradicts an applicable mandatory rule.

### REQ-VER-043

Incomplete MUST identify the known required evidence, dependency, or control that remains insufficient.

### REQ-VER-044

Indeterminate MUST identify the uncertainty or unresolved condition preventing a reliable Valid or Invalid conclusion.

### REQ-VER-045

Unsupported MUST identify the semantic, version, extension, algorithm, proof, profile, or binding the VE cannot interpret or execute.

### REQ-VER-046

Unavailable MUST identify the evidence, dependency, key, service, or representation that could not be accessed and the reason category where known.

### REQ-VER-047

Conflicting MUST identify competing evidence and MUST NOT be resolved through undocumented input order, resolver preference, or heuristic.

### REQ-VER-048

Warnings MUST NOT convert mandatory failure into Validity or full profile achievement.

### REQ-VER-049

Outcome dependency propagation MUST be deterministic and MUST affect only checks and claims that rely upon the failed dependency.

### REQ-VER-050

Per-check, per-dependency, per-object, per-claim, Completeness, control, and aggregate results MUST remain accessible when applicable.

### REQ-VER-051

Governed scores or confidence values MUST disclose inputs, method, missing-data behavior, thresholds, version, and relationship to canonical outcomes.

### REQ-VER-052

A score, narrative, or implementation preference MUST NOT override a mandatory Invalid, Unsupported, Unavailable, Incomplete, Indeterminate, or Conflicting result.

### REQ-VER-053

Resolver snapshots materially affecting reproducibility SHOULD record query, namespace, returned version, integrity, resolver role, retrieval time, cache state, and conflicts.

### REQ-VER-054

Offline Verification profiles MUST identify embedded and preinstalled dependencies, permitted trust roots, freshness limits, and checks not performed.

### REQ-VER-055

Cached results MUST be keyed by target integrity, Verification Context, dependency state, resolver snapshot, rule version, and relevant time semantics.

### REQ-VER-056

Reverification SHOULD occur when evidence, context, algorithms, dependencies, profiles, or Pack versions change materially.

### REQ-VER-057

Every Verification Report MUST identify report identity, version, verifier, Verification Time, evidence scope, and Verification Context.

### REQ-VER-058

Every VR MUST identify claims, checks, dependencies, individual outcomes, Completeness, Intended and Achieved Trust Profiles, warnings, and limitations applicable to its scope.

### REQ-VER-059

VR cryptographic protection MUST cover all properties whose alteration could change evidence, context, performed checks, dependencies, outcomes, profile achievement, or limitations.

### REQ-VER-060

Later evidence or changed context MUST produce a new VR and MUST NOT silently rewrite the earlier report.

### REQ-VER-061

Human-readable explanations MUST remain consistent with canonical machine-readable outcomes and MUST identify why stronger conclusions were not supported.

### REQ-VER-062

Unsupported mandatory Verification Report semantics or dependencies MUST produce an explicit non-success outcome when the report itself is evaluated.

---

# Security Considerations

Verification Engines process untrusted evidence, compose high-impact conclusions, and may become targets for manipulation, denial of service, or policy laundering.

## Parser Exploitation

Malformed objects, archives, media, or active content may exploit parsers before semantic validation. Implementations should isolate processing, use memory-safe components where practical, patch dependencies, reject ambiguous encodings, and disable unsafe execution.

## Canonicalization Differential

Different parsers may derive different signed inputs from the same logical data. Strict canonicalization, duplicate rejection, version dispatch, Unicode and number rules, and cross-implementation test vectors are required.

## Algorithm Confusion

An attacker may substitute an algorithm identifier, exploit permissive parameters, or trigger unsafe fallback. Algorithms, parameters, key types, Signature encodings, and purposes must be bound and allowlisted by the applicable profile.

## Key Substitution

A malicious resolver may return a different current key for a historical KID. Exact key material, Key Transparency proofs, historical boundaries, and resolver evidence prevent current-state substitution.

## Authority Laundering

A valid Signature may be presented as proof of business Authority. Verification must maintain separate Authority and Policy results and reject circular or out-of-scope delegation.

## Historical-State Rewrite

Current Policy, Registry, Witness eligibility, or Trust Profile may be substituted for the historical version. Versioned snapshots, Checkpoints, Preservation, and explicit context constrain this attack.

## Missing-Dependency Optimism

A VE may treat absent evidence as satisfied to maximize successful outcomes. Mandatory dependency rules and explicit Incomplete, Unavailable, or Indeterminate results prevent optimistic defaulting.

## Unsupported-Semantics Bypass

Unknown mandatory fields or algorithms may be ignored by permissive implementations. Criticality rules must fail safely and expose Unsupported outcomes.

## Conflict Suppression

Input ordering, cache preference, or resolver precedence may hide a competing object or history. Conflict handling must be deterministic, governed, and visible.

## Split-View Resolution

A verifier may receive a consistent local Chain, KT, or Registry view while another party receives an incompatible view. Witness comparison, gossip, Checkpoints, Anchors, multiple resolvers, and conflict evidence reduce undetected equivocation.

## Time Manipulation

Producer, resolver, or verifier clocks may change activation, expiry, commitment, freshness, or retention conclusions. Time sources, precision, ordering evidence, clock assumptions, and external boundaries must remain explicit.

## Resolver Poisoning

DNS, caches, registries, mirrors, or APIs may return substituted dependencies. Stable identity, version, cryptographic integrity, authoritative-source rules, and snapshot retention are required.

## Cache Replay

A valid earlier result may be reused after revocation, correction, profile change, or new evidence. Cache keys and freshness rules must include effective context and dependency state.

## Trust-Profile Downgrade

An attacker may select a weaker profile or misreport the configured profile as achieved. The report must show the intended profile, actual control results, downgrade rule, and achieved profile.

## Outcome Flattening

Converting rich outcomes to a Boolean can make Unsupported, Incomplete, or Conflicting evidence appear acceptable. Downstream APIs and interfaces should preserve canonical status codes and result details.

## Opaque-Score Manipulation

Undocumented scoring allows hidden weights, missing-data optimism, and business incentives to replace protocol rules. Scores cannot override mandatory outcomes and must be versioned and explainable if protocol-relevant.

## Resource Exhaustion

Large graphs, expensive algorithms, decompression bombs, repeated external lookups, or adversarial nesting may consume unbounded resources. VEs must impose limits and report the resulting non-success state honestly.

## Algorithmic Denial of Service

Mathematically valid but costly keys, proofs, or parameter sets can exhaust compute. Profiles should constrain sizes and algorithms before expensive operations.

## Circular Proof

An Authority, key, or dependency may ultimately rely upon itself. Graph evaluation must detect cycles and require an independently established trust foundation.

## Verification Report Forgery

Reports may be altered or attributed to another verifier. Canonical report protection, Historical Key State, report Authority, evidence binding, and version lineage are required.

## Misleading Human Summary

A UI may display `verified` while hiding failed controls. Human-readable summaries must derive from and remain consistent with machine-readable results, with material limitations visible.

## Partial-Run Success

A crash, timeout, missing plugin, or user cancellation may stop required checks. The run must not report aggregate Validity unless all mandatory checks completed successfully.

## Nonconforming Engine Claims

A product may claim TrustAgentAI Verification while implementing only Signature checking. Conformance scopes, supported profiles, negative tests, and capability declarations must remain explicit.

## Malicious Evidence Rendering

Human review tools may render scripts, external images, tracking links, macros, or misleading bidirectional text. Rendering should be isolated and canonical machine results must not depend upon unsafe content execution.

## Verifier Conflict of Interest

A technically conforming engine may be operated by a dispute party. Protocol correctness and organizational independence are separate. Trust Profiles or external processes may require independent operation or report countersignature.

## Report Staleness

A report valid at one Verification Time may become outdated after later evidence or algorithm changes. Reports must carry time and context and must not be presented as timeless conclusions.

## Error-Message Leakage

Detailed failures may expose identities, keys, confidential fields, or investigative scope. Reporting should separate authorized diagnostic detail from minimally disclosed outcomes.

---

# Privacy Considerations

Verification can expose more information than the final claim requires because resolving dependencies and explaining failures may traverse extensive identity, Policy, and historical graphs.

## Claim-Bounded Access

The VE should access and disclose only evidence required for the focal claims and applicable Trust Profile. Broad archive access is not justified merely because the engine can process it.

## Resolver Query Leakage

Identity, key, Policy, and Registry queries reveal which parties and historical periods are under review. Offline Packs, local mirrors, private retrieval, batching, and query minimization may reduce leakage.

## Dependency-Graph Exposure

The VDG can reveal organizational roles, key history, Witness relationships, incidents, and external systems. Reports may use protected references or summarized failure categories when full topology is not required.

## Historical-Key Sensitivity

Public keys are not secrets, but identity-key relationships, rotation cadence, compromise history, and recovery structure can be sensitive and correlatable.

## Policy and Authority Confidentiality

Policies, delegation chains, exceptions, and approvals may reveal commercial or security controls. Selective disclosure and role-restricted compartments should preserve the minimum semantics needed for Verification.

## Verification Time and Activity

Report creation and resolver timestamps may reveal investigations or disputes. Publication and access should be governed separately from report integrity.

## Redacted Evidence

The engine should distinguish redacted, unavailable, and proven-absent content without exposing the protected value. Redaction itself and its effect on outcomes must remain visible.

## Temporary Data

Parsing, decryption, canonicalization, graph construction, and report generation may create plaintext caches, temporary files, memory dumps, telemetry, and backups. These require lifecycle, access, and cleanup controls.

## Report Minimization

A durable VR should include enough evidence references and explanations for reproduction while avoiding unnecessary payload duplication, secret keys, personal data, and operational telemetry.

## Machine Logs

Debug logs can contain raw evidence, keys, tokens, paths, or resolver responses. Conforming implementations should support safe logging and separate diagnostic retention from canonical report content.

## Differential Disclosure

Different recipients may be authorized for different evidence compartments. Reports should identify which context and disclosed set produced each outcome rather than merge them into an apparently universal result.

## Verification as Surveillance

Continuous verification and monitoring can become a behavioral surveillance system. Deployments should define purpose, proportionality, access, retention, and aggregation limits.

## Right to Contest

Where applicable, affected parties should be able to identify the evidence and protocol rules supporting a conclusion and present corrections or counterevidence without rewriting the earlier report.

## Long-Term Report Retention

Reports may outlive the operational dispute and remain highly sensitive. Preservation, retention, Legal Hold, redaction, and disposition should be governed according to report purpose and applicable law.

TrustAgentAI makes the effect of privacy controls visible. It does not determine applicable privacy, disclosure, privilege, or records law.

---

# Design Rationale

## Why Verification Is Claim-Bounded

Evidence can be valid for one question and insufficient for another. Explicit claims prevent one successful check from becoming an unlimited assurance statement.

## Why Context Is First-Class

Versions, time, Policy, Trust Profile, resolver state, and available dependencies can change conclusions. Treating context as an input makes those differences reproducible rather than mysterious.

## Why Validation Is Layered

Structure, cryptography, history, Authority, Completeness, and profile achievement answer different questions. Layering preserves useful partial results and identifies the exact failure boundary.

## Why Outcomes Are Not Boolean

Invalid, Incomplete, Indeterminate, Unsupported, Unavailable, and Conflicting require different remediation and carry different meanings. One Boolean either loses that information or encourages false success.

## Why Valid Is Not Complete

Every included object may be valid while a required Authority record, Chain range, Witness quorum, schema, or key history is missing. Completeness must therefore be evaluated independently.

## Why Intended Is Not Achieved

Configuration describes what a deployment hoped or was required to do. Only verified controls and evidence establish achieved assurance.

## Why Historical State Is Required

Current keys, policies, registries, and algorithms answer present operational questions. Historical Accountability Claims require the state applicable when the action and evidence occurred.

## Why Dependencies Are a Graph

A conclusion may rely upon shared, nested, cyclic, or alternative objects. Graph semantics expose these relationships, allow bounded evaluation, and prevent hidden resolver assumptions.

## Why Determinism Matters

Accountability is weakened when vendor-specific heuristics or mutable defaults produce different answers. Equivalent canonical outcomes allow independent implementations to test and challenge one another.

## Why Scores Cannot Override Rules

Scores can summarize risk but can also hide mandatory failures. Canonical outcomes protect protocol meaning while allowing products to add clearly separated decision aids.

## Why Offline Verification Matters

Disputes may occur after providers fail, access is revoked, or network resolution is unsafe. Offline profiles test whether evidence and dependencies are genuinely portable and preserved.

## Why Reports Are Protocol Objects

A durable VR makes the evidence, context, performed checks, dependencies, and conclusion accountable. It supports later comparison without turning the conclusion into timeless truth.

## Why Later Reports Do Not Rewrite Earlier Ones

New evidence can legitimately change a conclusion. Preserving both reports shows what was known, checked, and concluded at each boundary.

## Why Protocol and Legal Conclusions Are Separate

Cryptographic and historical evidence can inform law, audit, and business decisions, but those domains apply additional facts, standards, burdens, and Authority outside TAIP.

## Why Independent Engines Are the Ultimate Test

If only the producer's proprietary system can verify the evidence, accountability still depends upon the producer. Open semantics and cross-engine reproducibility demonstrate that the evidence—not the vendor narrative—supports the conclusion.

---

# Summary

Verification is the claim-bounded, reproducible evaluation of TrustAgentAI evidence and dependencies under an explicit Verification Context.

It applies layered checks to:

- availability and safe parsing;
- structure and semantics;
- identifiers and canonicalization;
- cryptographic protection;
- Historical Key State, Key Purpose, Authority, and Policy;
- references and Verification Dependencies;
- Hash Chains and lifecycle state;
- Witness eligibility and quorum;
- Checkpoints and External Anchors;
- Preservation and Dispute Packs;
- causal and execution relationships;
- Completeness and Trust Profile achievement.

The central distinctions are:

```text
Valid
≠
Complete
```

```text
Intended Trust Profile
≠
Achieved Trust Profile
```

```text
Successful Protocol Verification
≠
Business Truth
≠
Legal Validity
≠
Regulatory Compliance
```

Verification Outcomes must distinguish Valid, Invalid, Incomplete, Indeterminate, Unsupported, Unavailable, Conflicting, warnings, and Not Evaluated state. A mandatory failure cannot be hidden by a score, narrative, cache, or silent downgrade.

Equivalent evidence evaluated under equivalent contexts should produce equivalent canonical conclusions across independent engines. Resolver state, historical versions, dependencies, resource limits, and performed checks must therefore remain explicit.

A Verification Report preserves the evidence scope, context, checks, result graph, Completeness, Intended and Achieved Trust Profiles, warnings, and limitations at a defined Verification Time. Later evidence creates a new report rather than rewriting the earlier conclusion.

The stable objective is a protocol conclusion that another authorized party can reproduce, inspect, challenge, and explain without relying upon one vendor's hidden logic or the producer's assertion about its own evidence.
