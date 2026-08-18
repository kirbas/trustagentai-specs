# Chapter 15 — Trust Profiles

> **A Trust Profile names versioned assurance requirements; achieved assurance is earned by verified controls and evidence, never inherited from configuration, product tier, or profile label.**

## Purpose

This chapter defines the architectural specification for TrustAgentAI **Trust Profiles (TPs)**, profile authorities, profile definitions, control requirements, Intended Trust Profiles, Achieved Trust Profiles, reference assurance classes `TP0` through `TP4`, profile registries, profile composition, graceful degradation, and profile conformance.

Trust Profiles make assurance explicit, selectable, measurable, composable, historically interpretable, and independently verifiable. They connect an Accountability Claim or Accountable Action to the exact controls and evidence required for a bounded assurance objective.

This chapter establishes:

- the identity, version, lifecycle, and governance of Trust Profile definitions;
- the model for mandatory, conditional, optional, and prohibited controls;
- profile dependencies, thresholds, independence criteria, exceptions, and evidence bindings;
- selection and durable binding of a profile to an Accountable Action or claim;
- separation of Intended Trust Profile from Achieved Trust Profile;
- deterministic profile evaluation and control-level outcomes;
- a reference assurance progression for `TP0`, `TP1`, `TP2`, `TP3`, and `TP4`;
- profile inheritance, composition, extension, equivalence, upgrade, downgrade, and migration;
- explicit graceful degradation without silent loss of assurance;
- profile registries, discovery, conformance declarations, test vectors, and governance;
- the boundaries among protocol assurance, certification, Compliance, Legal Validity, and business truth;
- Trust Profile invariants and architectural requirements.

This chapter applies the common Protocol Object rules defined in [06-Protocol-Objects.md](06-Protocol-Objects.md).

It composes the Evidence Record model in [07-Evidence-Record-Specification.md](07-Evidence-Record-Specification.md), the Hash Chain model in [08-Hash-Chain-Specification.md](08-Hash-Chain-Specification.md), the Witness model in [09-Witness-Observation-Specification.md](09-Witness-Observation-Specification.md), the Checkpoint and External Anchor model in [10-Checkpoints-and-External-Anchors.md](10-Checkpoints-and-External-Anchors.md), the Historical Key State model in [11-Key-Transparency.md](11-Key-Transparency.md), the Preservation model in [12-Preservation.md](12-Preservation.md), the Dispute Pack model in [13-Dispute-Packs.md](13-Dispute-Packs.md), and the Verification model in [14-Verification.md](14-Verification.md).

It also applies the system model in [05-System-Overview.md](05-System-Overview.md), the principles in [04-Design-Principles.md](04-Design-Principles.md), and the canonical terms in [Terminology.md](Terminology.md) and [Acronyms.md](Acronyms.md).

This chapter defines architectural semantics.

It does not require one business risk model, profile authority, assurance ladder, cryptographic suite, infrastructure provider, deployment topology, certification body, user interface, or legal framework. It does not assign universal monetary limits to profile names, define every concrete field or wire encoding, or make `TP0` through `TP4` sufficient by label alone.

Exact normative profile documents, schemas, registries, algorithm suites, control identifiers, APIs, test vectors, and conformance suites belong to TAIP and applicable governance. Those materials must preserve the invariants in this chapter.

---

# 15.1 Trust Profile Definition

A **Trust Profile** is a uniquely identified, versioned, governed set of assurance requirements for evaluating defined Accountable Actions, Accountability Claims, evidence populations, or protocol states.

A profile may define:

- required Protocol Objects and lifecycle evidence;
- permitted algorithms, key types, parameters, and Key Purposes;
- identity, Historical Key State, Authority, and Policy requirements;
- Hash Chain, Witness, Checkpoint, and External Anchor controls;
- independence, diversity, quorum, cadence, and threshold rules;
- Preservation, Evidence Lifetime, availability, and recoverability requirements;
- Verification Dependencies and resolver constraints;
- Completeness, degradation, exception, and outcome-composition rules;
- conformance evidence and test-vector obligations.

```text
Trust Profile
    =
Versioned Control Requirements
    +
Evidence and Evaluation Rules
    +
Governance and Historical Context
```

A profile is a specification of conditions. It is not proof that those conditions were satisfied.

---

# 15.2 Assurance Objective and Claim Boundedness

Every Trust Profile must serve an explicit **assurance objective**.

An assurance objective describes the bounded failure modes, claims, actions, populations, time horizons, and trust assumptions the profile is intended to address. Examples include:

- producer-attributable creation of an Evidence Record;
- verifiable ordering within an accountable history;
- observation by eligible parties outside the producer's Control Domain;
- resistance to producer-only history rewrite;
- durable verification across organizational or infrastructure change;
- portable dispute evaluation after live services become unavailable.

A profile is not universally strong or weak without scope.

```text
Profile Strength
≠
Universal Truth or Universal Risk Fitness
```

A profile suitable for one claim, action class, value band, jurisdiction, or evidence lifetime may be unsuitable for another. Profile selection therefore remains claim-bounded and Policy-governed.

---

# 15.3 Scope

Trust Profiles may apply to:

- one Accountable Action;
- one Accountability Claim;
- an action or claim class;
- one Evidence Record type;
- a Chain or Chain segment;
- a workflow, tenant, Organization, or counterparty relationship;
- a Dispute Pack or Verification run;
- a defined historical interval;
- a deployment capability declaration.

These scopes are not interchangeable.

A deployment-wide profile setting does not prove that every action used that profile. A profile applied to a Chain does not automatically govern every referenced object outside the Chain. A Verification Engine capability declaration does not establish the Achieved Trust Profile of evidence it has not evaluated.

The effective profile scope must be explicit whenever ambiguity could change a conclusion.

---

# 15.4 What a Trust Profile Establishes

A complete profile definition establishes the rules needed to evaluate a bounded assurance claim, including:

- which controls are relevant;
- which controls are mandatory, conditional, optional, or prohibited;
- which evidence demonstrates each control;
- which dependencies are required to interpret that evidence;
- which thresholds and independence criteria apply;
- how failures, omissions, conflicts, and exceptions affect outcomes;
- whether and how a lower profile may be achieved;
- which profile identifier and version describe the result.

When a Verification Engine determines that every mandatory requirement is satisfied under the applicable Verification Context, it may report the profile as achieved for the defined scope.

The conclusion remains bounded to that scope, evidence set, historical boundary, and Verification Time.

---

# 15.5 What a Trust Profile Does Not Establish

A Trust Profile does not automatically establish:

- the truth of producer-supplied business facts;
- the wisdom or fairness of an Agent's decision;
- the absence of fraud, collusion, compromise, or implementation defects;
- legal identity beyond supported identity evidence;
- Authority beyond evaluated delegation and Policy evidence;
- execution, settlement, or external-world effects not supported by evidence;
- evidentiary admissibility, Legal Validity, or Regulatory Compliance;
- fitness for every risk model or jurisdiction;
- independence merely because several services or Signatures are present;
- achievement merely because software was configured with the profile name.

```text
Profile Definition
≠
Profile Achievement
≠
Certification
≠
Compliance or Legal Judgment
```

Parties may use profile results as inputs to broader decisions, but those decisions must identify their additional rules and authorities.

---

# 15.6 Conceptual Roles

Trust Profile workflows may involve:

- a **Profile Authority** that governs a profile namespace or family;
- a **Profile Author** that drafts a profile definition;
- a **Profile Registry Operator** that publishes identifiers, versions, status, and integrity material;
- a **Policy Authority** that selects profiles for action classes or claims;
- an **Evidence Producer** that attempts to satisfy selected controls;
- control providers such as Witnesses, Checkpoint Authorities, Anchor Systems, Preservation Services, and key custodians;
- a **Verifier** or **Verification Engine** that evaluates profile requirements;
- a **Conformance Assessor** that evaluates an implementation, deployment, or service capability;
- a **Certification Body** that issues a separately governed attestation;
- relying parties that interpret profile outcomes for bounded decisions.

One entity may perform multiple roles. Role combination must remain visible where separation, independence, or conflict of interest affects assurance.

---

# 15.7 Profile Authority and Governance Boundary

A **Profile Authority** is the governed source authorized to define, version, publish, deprecate, or withdraw profiles within a namespace.

The authority model should identify:

- the responsible Organization or governance body;
- Authority and delegation evidence;
- authorized Key Purposes and signing keys;
- approval, review, and change-control procedures;
- conflict-of-interest and transparency rules;
- publication and registry responsibilities;
- emergency suspension or withdrawal procedures;
- preservation obligations for historical definitions.

Cryptographic protection of a profile proves only bounded integrity and signer control. It does not by itself prove that the signer possessed governance Authority.

Profile governance and profile evaluation are separate responsibilities. A verifier must be able to identify why a profile definition is authoritative for the evaluated scope.

---

# 15.8 Profile Identity and Namespace

Every Trust Profile must have a stable identifier within an explicit namespace.

Profile identity may include:

- namespace identifier;
- local profile name or code;
- major, minor, or other governed version components;
- immutable content identifier or digest;
- issuing Profile Authority;
- family or lineage identifier;
- lifecycle status.

```text
Display Name
≠
Stable Profile Identifier
≠
Profile Version
≠
Profile Content Digest
```

Short names such as `TP3` are meaningful only within the applicable profile family, namespace, and version. Implementations must not infer universal semantics from the lexical form or numeric suffix alone.

Human-readable aliases may change. They must not replace immutable identity where historical interpretation depends upon exact profile content.

---

# 15.9 Versioning and Historical Immutability

A profile version identifies a fixed set of semantics.

A material change requires a new version. Material changes include changes to:

- mandatory or prohibited controls;
- thresholds, quorum, independence, or cadence;
- permitted algorithms or parameters;
- evidence and dependency requirements;
- exception or downgrade behavior;
- outcome composition;
- inheritance or composition dependencies;
- profile scope or assurance objective.

Editorial corrections that cannot affect interpretation may use governed errata, but the original and corrected representations must remain distinguishable.

The latest profile version must not be applied retroactively to historical evidence unless an explicit reassessment is requested and reported as a new conclusion.

```text
Profile at Action Time
≠
Current Profile
≠
Profile Used for Later Reassessment
```

Historical profile definitions, registries, algorithms, and dependency versions must remain preservable and resolvable.

---

# 15.10 Profile Lifecycle State

A Profile Authority may assign lifecycle states such as:

- `draft`;
- `candidate`;
- `active`;
- `deprecated`;
- `superseded`;
- `suspended`;
- `withdrawn`;
- `archived`.

Lifecycle state must have defined semantics and an effective time or sequence boundary.

`Deprecated` generally means new selection is discouraged while historical interpretation remains valid. `Superseded` identifies a governed successor but does not make the versions equivalent. `Suspended` may temporarily prohibit new use. `Withdrawn` may identify a material defect, but must not erase the historical definition or prior evidence.

Lifecycle changes are Accountable Actions when they affect assurance interpretation. They should be protected, published, preserved, and independently verifiable according to applicable governance.

---

# 15.11 Canonical Profile Definition

A canonical Trust Profile definition should identify or bind to:

- profile identity, version, digest, authority, and lifecycle state;
- assurance objective and scope;
- applicable TAIP, schema, registry, and algorithm versions;
- parent, component, or extension profiles;
- control definitions and stable control identifiers;
- criticality, conditions, thresholds, and prohibited combinations;
- evidence requirements and Verification Dependencies;
- independence and Control Domain criteria;
- degradation, fallback, exception, and reassessment rules;
- conformance and test-vector references;
- effective and deprecation boundaries;
- human-readable rationale that does not override machine-readable rules.

A profile definition may be represented as a governed Protocol Object or as a cryptographically bound protocol resource.

The exact wire representation belongs to TAIP. Canonical meaning must not depend upon undocumented parser behavior, database defaults, or a vendor-specific UI.

---

# 15.12 Control Requirement Model

A **Profile Control** is a named, versioned assurance condition whose satisfaction can be evaluated from defined evidence and context.

A control should identify:

- stable control identifier and version;
- control family and assurance purpose;
- applicability condition;
- required inputs and evidence types;
- evaluation algorithm or rule reference;
- dependencies and trust roots;
- success, failure, and non-evaluated semantics;
- thresholds, time windows, and cardinality;
- relationships to other controls;
- effect on profile and claim outcomes.

Control descriptions should be testable.

Statements such as `use strong security`, `use trusted Witnesses`, or `retain evidence appropriately` are not sufficient normative controls without measurable criteria.

---

# 15.13 Control Evaluation Status

Control evaluation must preserve more information than a Boolean.

Control-level states may include:

- `satisfied`;
- `failed`;
- `incomplete`;
- `indeterminate`;
- `unsupported`;
- `unavailable`;
- `conflicting`;
- `not-applicable`;
- `not-evaluated`;
- `satisfied-with-warning` where the profile permits it.

Each result should identify the evaluated control version, scope, evidence, dependencies, Verification Context, and reason.

```text
Control Not Applicable
≠
Control Satisfied
≠
Control Not Evaluated
```

Profile aggregation must use governed composition rules. An implementation must not convert unknown or unavailable mandatory state into satisfaction.

---

# 15.14 Requirement Criticality and Cardinality

A profile may classify requirements as:

- **mandatory** — must be satisfied for the profile to be achieved;
- **conditional** — mandatory when a defined predicate is true;
- **optional** — may contribute evidence or a claim but is not required for profile achievement;
- **recommended** — expected absent a documented reason, without becoming mandatory unless the profile says so;
- **prohibited** — must not be present, selected, or used in the prohibited manner;
- **informational** — explanatory and non-normative.

Profiles may also define cardinality such as exactly one, at least one, at least `k`, no more than `n`, one per Control Domain, or continuous coverage for a time interval.

Criticality and cardinality must be machine-interpretable where they affect results.

An optional control must not be marketed as a mandatory property of the profile. A recommended control must not silently become mandatory through one implementation's default.

---

# 15.15 Conditions, Thresholds, and Exceptions

Conditional controls require deterministic predicates.

Predicates may depend upon:

- action or claim type;
- amount, value band, risk class, or Policy category;
- jurisdiction or counterparty class;
- evidence sensitivity;
- Evidence Lifetime;
- elapsed time or lifecycle state;
- availability of an external system;
- presence of another control or object type.

Thresholds must define units, comparison semantics, boundary behavior, input provenance, missing-data handling, and version.

An **exception** or **waiver** does not make an unsatisfied control satisfied. A governed exception may change which rule applies only when the profile explicitly permits it and the exception evidence identifies scope, authority, reason, effective interval, and consequences.

Undocumented exceptions are nonconformance. Retroactive exceptions must not rewrite the Intended Trust Profile or earlier Verification results.

---

# 15.16 Profile Dependency Graph

A profile may depend upon other governed material, including:

- base or component profiles;
- TAIP versions;
- Protocol Object schemas;
- canonicalization rules;
- algorithm and identifier registries;
- key and Authority registries;
- Policy definitions;
- Witness eligibility registries;
- Checkpoint or Anchor bindings;
- Preservation and retention specifications;
- conformance suites and test vectors.

These relationships form a **Profile Dependency Graph**.

Each mandatory dependency must be identified by stable identity, version, integrity, and applicability. Mutable `latest` references are insufficient for historical conclusions.

Dependency evaluation must detect cycles, conflicting versions, unresolved critical semantics, and circular assurance. A profile cannot prove its own Authority or integrity merely by referencing itself through a dependency cycle.

---

# 15.17 Mapping Accountable Actions and Claims

Profiles must connect to the things they govern.

A mapping rule may select a profile based upon:

- Accountable Action type;
- Accountability Claim type;
- subject, actor, Organization, or counterparty class;
- value or risk band;
- Policy and Authority scope;
- workflow or lifecycle phase;
- required Evidence Lifetime;
- regulatory or contractual category;
- exception or emergency state.

The mapping source, version, and effective boundary must remain explicit.

One action may support several claims governed by different profiles. One profile result must not be copied across claims whose required evidence or scope differs.

```text
Action Profile
≠
Every Claim Profile
```

Conflicting mapping rules must produce a governed conflict outcome or deterministic resolution. Implementations must not select the weakest profile merely because it is easiest to satisfy.

---

# 15.18 Profile Selection and Binding

The Intended Trust Profile should be selected and bound before, or no later than, the accountability-critical transition whose assurance it governs.

The binding should identify:

- profile identifier and exact version;
- scope and subject;
- selecting Policy and Authority;
- selection time or sequence boundary;
- applicable mapping rule;
- permitted degradation or exception behavior;
- integrity protection sufficient to detect substitution.

Binding may be direct or through an immutable governed reference.

Selecting a profile after observing which controls happened to succeed is outcome shopping, not prior assurance intent. A later reviewer may evaluate evidence against an additional profile, but that reassessment must be identified separately from the profile intended at action time.

---

# 15.19 Intended Trust Profile

The **Intended Trust Profile** is the exact profile identifier and version selected or required for a defined scope.

It expresses expected assurance, not achieved assurance.

The intended profile may be established by:

- applicable Policy;
- contract or counterparty agreement;
- protocol rule;
- workflow configuration bound into evidence;
- risk classification;
- explicit authorized selection;
- a stricter rule chosen by the producer or relying party.

Where several rules apply, the conflict or composition rule must be explicit. `Highest number wins` is not a safe universal rule because profile families may be non-linear or incomparable.

The intended profile remains historically fixed for the action. A later Policy or configuration change does not rewrite it.

---

# 15.20 Achieved Trust Profile

The **Achieved Trust Profile** is the exact profile whose mandatory requirements are supported by verified controls and available evidence for a defined scope under an explicit Verification Context.

```text
Observed Evidence
    +
Verified Control Results
    +
Profile Composition Rules
    =
Achieved Trust Profile or Explicit Non-Achievement
```

The Achieved Trust Profile may be:

- equal to the Intended Trust Profile;
- a lower profile in a declared ordered family;
- another profile whose requirements are independently satisfied;
- partial or incomplete where no complete registered profile is achieved;
- indeterminate, unsupported, unavailable, or conflicting;
- not evaluated.

A profile is achieved only when all applicable mandatory controls and dependencies satisfy the profile's rules. Product licensing, deployment topology, service availability, a configuration label, or a prior certificate is not sufficient evidence of achievement.

---

# 15.21 Intended/Achieved Gap

The difference between Intended and Achieved Trust Profiles is an accountability result.

A gap report should identify:

- intended profile identity and version;
- achieved profile identity and version, if any;
- every unsatisfied intended control;
- control states and reasons;
- missing, unavailable, unsupported, or conflicting dependencies;
- exceptions or degradation rules applied;
- claims that remain supported;
- claims that became incomplete, indeterminate, unsupported, or invalid;
- whether later evidence can close the gap.

```text
Intended: TP3
Achieved: TP2
Reason: independent Witness quorum incomplete
Effect: producer commitment supported; independent observation claim incomplete
```

The gap must not be hidden in a warning while the intended label remains the primary reported result.

---

# 15.22 Profile Evaluation Pipeline

Profile evaluation should proceed through explicit stages:

```text
Resolve Profile Identity and Historical Version
          ▼
Validate Profile Definition and Authority
          ▼
Resolve Profile Dependencies
          ▼
Determine Scope and Applicability
          ▼
Evaluate Individual Controls
          ▼
Evaluate Thresholds, Independence, and Exceptions
          ▼
Compose Control and Claim Outcomes
          ▼
Determine Achieved Profile and Intended/Achieved Gap
```

Each stage may produce valid lower-layer results even if a later stage cannot complete.

An unsupported profile definition must not be treated as a failed evidence control. An unavailable dependency must not be reported as evidence contradiction. A malformed profile must fail definition validation before its aggregation rules are trusted.

Equivalent evidence and equivalent Verification Contexts should produce equivalent profile conclusions across conforming Verification Engines.

---

# 15.23 Control Families

A profile may select controls from families including:

- structural and semantic validity;
- canonicalization and identifier integrity;
- cryptographic integrity and authenticity;
- identity and Historical Key State;
- Authority, delegation, and Policy;
- Evidence Record lifecycle state;
- Hash Chain continuity and commitment;
- Witness observation, eligibility, independence, and quorum;
- Checkpoints and External Anchors;
- Preservation, migration, and Evidence Lifetime;
- Dispute Pack portability and Completeness;
- Verification reproducibility and reporting;
- privacy, minimization, selective disclosure, and access control;
- availability, recovery, monitoring, and incident response;
- implementation and deployment conformance.

Control families address different failure modes.

No family should be represented as a universal substitute for another. A Signature does not replace Historical Key State. Replication does not replace independence. An External Anchor does not prove source truth. Preservation does not make invalid evidence valid.

---

# 15.24 Cryptographic Controls

Cryptographic profile controls may specify:

- algorithm and parameter identifiers;
- minimum security strength;
- key types, sizes, generation, and custody;
- Signature, digest, commitment, and proof formats;
- canonicalization and domain-separation rules;
- permitted multi-Signature or threshold schemes;
- cryptographic agility and deprecation policy;
- renewal and migration requirements;
- random-number and nonce constraints;
- validation and failure behavior.

Algorithm names alone are insufficient. Profiles must bind the parameters, encodings, purposes, and historical policy needed for deterministic verification.

Mathematical validity does not establish Authority, Historical Key State, or claim truth. A profile that requires those properties must include separate controls and evidence.

Unsafe algorithm fallback is prohibited. If a mandatory algorithm or proof is unsupported, the result is Unsupported rather than silently evaluated under a different suite.

---

# 15.25 Identity, Key Purpose, and Historical Key State

Identity-related profile controls may require:

- stable Protocol Identity;
- exact Key Identifier and public-key material;
- Key Purpose compatible with the protected object or action;
- Historical Key State at the relevant boundary;
- issuance, activation, rotation, suspension, revocation, compromise, and retirement evidence;
- key-custody properties;
- separation of signing roles;
- Key Transparency inclusion, consistency, or gossip evidence;
- recovery and migration records.

```text
Valid Signature
≠
Valid Historical Key State
≠
Authorized Action
```

A profile must state which historical boundary controls key validity and how conflicting or unavailable key history affects achievement.

Current key-directory state must not silently replace historical evidence.

---

# 15.26 Authority and Policy Controls

Authority-related controls may require:

- an authoritative grant or delegation chain;
- bounded subject, action, resource, amount, and time scope;
- applicable Policy identity and version;
- required approvals or separation of duties;
- revocation, suspension, or emergency-control evidence;
- evidence that the selected profile itself was required or authorized;
- deterministic handling of conflicts among Policies or Authorities.

Possession of a credential or signing key is not unlimited business Authority.

Profile evaluation must keep distinct:

- cryptographic signer validity;
- Key Purpose;
- Historical Key State;
- Authority;
- Policy satisfaction;
- resulting claim support.

A profile cannot make an unauthorized action authorized merely by assigning stronger cryptography or more Witnesses.

---

# 15.27 Evidence Record and Hash Chain Controls

Evidence and history controls may require:

- typed and schema-valid Evidence Records;
- stable ERIDs and canonical representations;
- provenance and producer attribution;
- explicit lifecycle events;
- Commitment Receipts;
- Chain identity, positions, predecessor bindings, and Chain Head state;
- defined handling of gaps, forks, merges, corrections, and migration;
- required commitment latency;
- Chain inclusion or consistency proofs;
- defined coverage for one record, range, or population.

```text
Valid Evidence Record
≠
Committed Evidence Record
≠
Complete Accountable History
```

A profile must identify the exact historical conclusion required. A hash-linked sequence controlled entirely by the producer may provide tamper evidence without satisfying independent historical assurance.

---

# 15.28 Witness Controls

Witness controls may specify:

- eligible Witness identities and registries;
- Observation Scope;
- observed object, Chain state, Checkpoint, or other target;
- observation timing and freshness;
- Witness Historical Key State and Key Purpose;
- conflict and equivocation detection;
- quorum and threshold rules;
- required failure-domain diversity;
- Witness retention and availability;
- gossip or cross-observation behavior.

A Witness Observation supports only its declared Observation Scope.

It does not automatically prove producer intent, source truth, execution, settlement, or the absence of collusion.

Profiles requiring Witness assurance must define what is observed, by whom, under which historical eligibility rules, and how observations compose.

---

# 15.29 Independence Criteria

Independence is evaluated from actual control and correlated-failure relationships.

A profile may require separation across:

- Organizations or legal control;
- administrative personnel;
- cloud or infrastructure accounts;
- deployment pipelines;
- key custodians and KMS boundaries;
- databases and Preservation domains;
- network or geographic regions;
- economic incentives or contractual relationships;
- jurisdictions;
- incident-response authority.

No single criterion proves universal independence.

```text
Different Instances
≠
Different Control Domains
≠
Independent Assurance
```

A profile should define an independence predicate, the evidence required to evaluate it, the time at which it applies, and how disclosed conflicts affect results.

Unknown ownership or control relationships must not be optimistically treated as independent.

---

# 15.30 Quorum, Threshold, and Diversity

A Witness or approval quorum is a composition rule, not merely a count.

A profile may define:

- `k-of-n` eligible observations;
- at least `k` distinct Control Domains;
- minimum Organization, infrastructure, or jurisdiction diversity;
- weighted thresholds;
- role-specific participation;
- maximum contribution from one domain;
- time-window or cadence requirements;
- rules for abstention, conflict, or equivocation.

The quorum population and eligibility snapshot must be historically identifiable.

Three Signatures controlled by one administrator do not satisfy a three-domain quorum. Ten replicas of one Witness do not provide ten independent observations.

Weighted or probabilistic rules must define their inputs, calculation, missing-data behavior, and threshold exactly. An opaque trust score cannot replace a deterministic quorum rule.

---

# 15.31 Checkpoint and External Anchor Controls

Checkpoint controls may specify:

- Checkpoint Authority and Key Purpose;
- target Chain, range, state, or commitment root;
- predecessor and consistency relationships;
- issuance cadence and maximum latency;
- Witness or countersignature requirements;
- publication, discovery, and retention;
- conflict, fork, and missed-cadence handling.

External Anchor controls may additionally specify:

- external system and namespace;
- exact anchored commitment;
- submission and publication evidence;
- inclusion, finality, or confirmation rule;
- trust-boundary and availability assumptions;
- proof retention and independent resolvability.

An anchor proves only that a defined commitment reached a defined external state. It does not validate the truth, Authority, or Completeness of committed evidence.

---

# 15.32 Preservation and Evidence Lifetime Controls

Preservation controls may specify:

- Evidence Lifetime and retention boundaries;
- integrity monitoring and fixity checks;
- immutability or WORM properties;
- custody and access records;
- geographic, organizational, or provider diversity;
- availability and recoverability targets;
- dependency preservation;
- format, algorithm, and media migration;
- cryptographic renewal;
- destruction, legal hold, and end-of-life rules;
- preservation test frequency and evidence.

```text
Stored
≠
Replicated
≠
Preserved
```

A profile requiring future Verification must preserve the relevant Verification Dependency Graph, not only focal Evidence Records.

Availability at one Verification Time does not prove compliance with an entire Evidence Lifetime. Time-spanning controls require time-spanning evidence or appropriately bounded results.

---

# 15.33 Dispute Pack and Portability Controls

A profile may require a Dispute Pack that contains or binds to:

- focal evidence and related causal records;
- Chain, Witness, Checkpoint, and Anchor evidence;
- Historical Key State, Authority, and Policy evidence;
- exact profile definitions and registry snapshots;
- schemas, algorithms, and extension definitions;
- Preservation Evidence and migration records;
- a DPM identifying included, omitted, redacted, external, and unavailable material;
- prior or newly produced Verification Reports.

A valid Pack container does not make its evidence valid or complete.

Portability controls should ensure that an independent verifier can evaluate the claimed profile without privileged access to the producer's live production environment, except where a clearly identified external dependency is intentionally required.

---

# 15.34 Verification and Reporting Controls

Profile evaluation uses the Verification architecture defined in Chapter 14.

A profile may require:

- a specified Verification Context;
- supported Verification Engine capabilities;
- deterministic dependency resolution;
- required checks and outcome taxonomy;
- independent operation or report countersignature;
- offline-verifiable dependency packaging;
- periodic reverification;
- a durable Verification Report;
- machine-readable control-level results;
- human-readable explanations consistent with canonical results.

Every profile conclusion should bind to the evidence, profile version, control results, dependencies, Verification Time, and scope that produced it.

A prior Verification Report is evidence of a prior evaluation. It is not timeless proof of present achievement.

---

# 15.35 Completeness and Dependency States

Profile achievement depends upon evidence Completeness but is not identical to it.

```text
Object Validity
≠
Evidence Completeness
≠
Trust Profile Achievement
```

A profile must define how at least the following states affect mandatory controls:

- evidence proven absent;
- evidence omitted from a package;
- evidence redacted;
- evidence unavailable at Verification Time;
- dependency unresolved;
- semantic or algorithm unsupported;
- competing evidence conflicting;
- check not performed;
- control not applicable.

These states must not collapse into one generic `missing` result when the distinction changes claim support or remediation.

A complete set for one claim may remain incomplete for another claim or profile.

---

# 15.36 Privacy and Minimum Disclosure Controls

A stronger profile must not automatically require broader collection or disclosure.

Privacy-related controls may define:

- data minimization and purpose limitation;
- selective disclosure or cryptographic proof alternatives;
- compartmentalized Pack entries;
- role- and claim-bounded access;
- retention and deletion limits;
- protected resolver behavior;
- disclosure logging;
- treatment of redacted or encrypted dependencies;
- linkage and correlation constraints;
- privacy-preserving evidence of independence or conformance.

Privacy mechanisms may satisfy a control without revealing the protected value only when the profile defines the proof semantics and the verifier supports them.

Redaction must remain visible and must not silently preserve full profile achievement when mandatory meaning cannot be verified.

---

# 15.37 Availability and Degradation Objectives

Profiles may define how operational availability interacts with assurance services.

Possible modes include:

- fail closed before action execution;
- accept evidence in a pending state until a control completes;
- continue the operational action while recording a lower assurance state;
- queue Witnessing, Checkpointing, anchoring, or Preservation for later completion;
- require human or Policy override;
- prohibit degradation for selected action classes.

The permitted behavior must identify:

- triggering failure condition;
- authorizing Policy and Authority;
- maximum duration or recovery window;
- minimum surviving controls;
- required warning and evidence;
- effect on Intended and Achieved profiles;
- claim and downstream-action restrictions;
- reassessment requirements.

Operational continuity does not justify assurance misrepresentation.

---

# 15.38 Reference Profile Family

TrustAgentAI reserves identifiers such as `TP0`, `TP1`, `TP2`, `TP3`, and `TP4` for standard Trust Profile specifications.

This chapter defines their **reference architectural progression**:

```text
TP0  Explicit producer assertion and structured evidence boundary
TP1  Cryptographically attributable evidence
TP2  Verifiable historical continuity and commitment
TP3  Independently observed and checkpointed accountability
TP4  Multi-domain, externally anchored, durably preservable assurance
```

The progression communicates the primary additional assurance objective of each class. It is not a substitute for a registered profile specification.

Exact mandatory controls, algorithms, thresholds, independence rules, Evidence Lifetime, permitted degradation, and conformance tests are defined by the applicable profile namespace and version.

The numeric labels must not be compared across unrelated profile families unless a governed mapping declares the relationship.

---

# 15.39 Reference TP0 — Explicit Evidence Boundary

Reference **TP0** establishes an explicit, typed accountability evidence boundary without claiming strong cryptographic attribution, independent historical commitment, or external corroboration.

A registered TP0 profile should at minimum require:

- identifiable evidence type and schema version;
- explicit producer and source attribution as assertions;
- stable object or event identity;
- declared action, claim, and lifecycle semantics;
- relevant time values with stated meaning;
- explicit Intended Trust Profile and limitations;
- visible missing, unsupported, or unavailable state;
- preservation or export sufficient for the stated short-lived objective.

TP0 may be useful for migration, observability, low-risk internal workflows, or environments beginning accountability instrumentation.

TP0 does not imply that the producer assertion is cryptographically protected, historically committed, independently observed, authorized, true, or durable. A TP0 result must not be marketed as independent verification.

---

# 15.40 Reference TP1 — Cryptographically Attributable Evidence

Reference **TP1** adds cryptographic object integrity and bounded producer attribution.

A registered TP1 profile should normally require:

- TP0 structured-evidence properties;
- canonical representation and stable digest or identifier semantics;
- Signature protection using permitted algorithms and parameters;
- exact signer key material, KID, and Key Purpose;
- Historical Key State sufficient for the relevant Signature boundary;
- producer identity binding;
- basic Authority or Policy evidence required by the focal claim;
- portable verification of the protected object and dependencies.

TP1 supports conclusions such as: the protected representation has not changed, and the applicable key produced a valid Signature under the evaluated historical context.

TP1 does not by itself establish ordered history, Chain commitment, independent observation, external anchoring, source truth, or complete business Authority. Producer-domain storage and verification may remain acceptable only when the exact TP1 specification permits them and reports the resulting trust boundary.

---

# 15.41 Reference TP2 — Verifiable Historical Continuity

Reference **TP2** adds verifiable historical continuity and commitment beyond isolated signed objects.

A registered TP2 profile should normally require:

- applicable TP1 controls;
- Hash Chain identity, predecessor binding, position, and Chain Head semantics;
- Commitment Receipts or equivalent proof of admission to accountable history;
- governed correction, fork, gap, merge, and migration behavior;
- periodic Checkpoints or another defined commitment boundary;
- Key Transparency sufficient to interpret historical signer state;
- preservation of Chain and profile dependencies for the required Evidence Lifetime;
- deterministic Verification of the relevant record, range, or history claim.

TP2 supports claims that evidence was committed into a defined, verifiable history under the evaluated rules.

TP2 does not automatically establish that the history was observed outside the producer's Control Domain. A producer-controlled Chain can provide valuable tamper evidence while remaining vulnerable to producer-only split views, withholding, or coordinated rewrite before external commitment.

---

# 15.42 Reference TP3 — Independently Observed Accountability

Reference **TP3** adds independent observation and stronger protection against producer-only equivocation.

A registered TP3 profile should normally require:

- applicable TP2 controls;
- eligible Witness Observations with explicit Observation Scope;
- a historically defined Witness Quorum;
- evidence-based independence across the Control Domains required by the profile;
- checkpointing that commits the observed state;
- conflict, equivocation, and missed-cadence handling;
- portable evidence sufficient for independent Verification;
- a Verification Report exposing control-level and profile-level results.

TP3 supports claims that defined evidence or history was observed and committed under an independently evaluable quorum rule.

TP3 does not imply that every Witness verified source truth or business execution. Its conclusion is limited by Observation Scope, eligibility, independence evidence, and the exact profile definition.

If the Witness quorum fails but TP2 controls remain satisfied, TP2 may be achieved only when the profile family explicitly declares that fallback and the downgrade is reported.

---

# 15.43 Reference TP4 — Multi-Domain Durable Assurance

Reference **TP4** adds resilience across several trust, infrastructure, and time domains.

A registered TP4 profile should normally require:

- applicable TP3 controls;
- stronger or multi-dimensional independence criteria;
- quorum diversity with bounded contribution from any one Control Domain;
- one or more External Anchors with defined publication and finality evidence;
- independent or multi-domain Preservation;
- complete preservation of critical Verification Dependencies;
- stronger key custody, separation of duties, and algorithm policy;
- long-term renewal, migration, recovery, and reverification procedures;
- portable Dispute Packs suitable for offline or independently resolved review;
- continuous monitoring of time-spanning controls and explicit incident evidence.

TP4 is intended for high-consequence, cross-organizational, or long-lived accountability where producer-only, single-provider, and short-term dependencies are insufficient.

TP4 is not absolute certainty. Collusion, false source data, governance failure, cryptographic breaks, and external-system failure remain possible and must be represented in the threat model and Verification Context.

---

# 15.44 Ordering, Cumulative Semantics, and Incomparability

Within one registered reference family, a later profile may declare that it **extends** an earlier profile and satisfies all of its mandatory requirements.

If `TP3 v1` explicitly extends `TP2 v1`, verified achievement of `TP3 v1` may imply `TP2 v1` for the same scope and context. That implication exists because of the declared dependency and verified controls, not because `3 > 2`.

Profiles may also be:

- incomparable because they address different claims;
- stronger in one control family and weaker in another;
- cumulative only for selected major versions;
- forks with no canonical total order;
- domain-specific extensions of a common base.

```text
Higher Numeric Suffix
≠
Universal Superset
```

A verifier must use declared inheritance and equivalence relationships rather than guess ordering from names.

---

# 15.45 Profile Inheritance

Profile inheritance allows a derived profile to reuse a base profile's requirements.

A derived profile must identify:

- exact base profile identity, version, and digest;
- inherited controls;
- added controls;
- permitted parameter specializations;
- any prohibited override;
- conflict-resolution semantics;
- resulting conformance and downgrade relationships.

A derived profile must not weaken a mandatory base control while claiming unqualified inheritance unless the base profile explicitly permits that variation and the new identity communicates it.

Inheritance graphs must be acyclic or have a deterministic, non-circular resolution model. Diamond inheritance must resolve duplicate or conflicting controls explicitly.

A base profile update does not mutate an already published derived version. A new derived version is required to adopt changed base semantics.

---

# 15.46 Profile Composition

Composition combines independently defined profile modules or control sets.

Examples include:

- a core evidence profile plus a financial-authorization module;
- a Chain profile plus a Witness profile;
- a privacy module plus a long-term Preservation module;
- a domain profile plus a jurisdiction-specific Policy profile.

Composition must define:

- component identities and exact versions;
- control namespace and parameter merging;
- duplicate-control handling;
- conflict and prohibition precedence;
- combined thresholds and dependencies;
- outcome composition;
- the identity of the resulting composed profile.

Runtime assembly of unnamed control fragments must not be reported as a registered profile unless the composition itself is deterministically identified and governed.

Adding controls may increase evidence but can also introduce incompatible assumptions. Composition therefore requires validation, not set union alone.

---

# 15.47 Profile Equivalence and Mapping

Organizations may need to map profiles across namespaces, versions, or standards.

A mapping may state:

- exact equivalence for a bounded scope;
- one-way implication;
- partial overlap by control family;
- stronger-than or weaker-than under defined assumptions;
- no established relationship.

Mappings must identify:

- both profile identities and versions;
- compared scopes and claims;
- control-by-control correspondence;
- unmatched requirements;
- assumptions and dependencies;
- mapping authority, version, and effective boundary;
- evidence used to justify the relationship.

Similarity of names, marketing tiers, or control counts does not prove equivalence.

One-way implication must not be reversed. A mapping that expires or is withdrawn remains historically identifiable for conclusions that previously used it.

---

# 15.48 Custom and Domain-Specific Profiles

Profile Authorities may define custom profiles for a domain, Organization, contract, workflow, risk class, or jurisdiction.

A custom profile should:

- use a distinct governed namespace;
- state its assurance objective and scope;
- reuse standard controls where semantics match;
- identify every extension and criticality rule;
- avoid redefining a standard identifier with incompatible meaning;
- publish conformance material;
- preserve historical versions;
- declare mappings to standard profiles only when justified.

Private profiles may be appropriate when requirements are confidential. The verifier still needs sufficient authenticated semantics to evaluate them. An undisclosed mandatory rule produces an Unsupported or Incomplete outcome for a verifier that cannot access or interpret it.

Proprietary implementation details must not become hidden normative semantics for an otherwise interoperable profile claim.

---

# 15.49 Profile Upgrade

A **profile upgrade** selects or evaluates a profile with additional or stronger requirements.

An upgrade may occur:

- before an Accountable Action because Policy or risk changed;
- after action time when later Witness, Checkpoint, Anchor, or Preservation evidence becomes available;
- during migration to a new algorithm or profile family;
- through independent reassessment against a stricter profile.

An upgrade must not rewrite history.

The result should identify:

- original Intended and Achieved profiles;
- target profile and version;
- new evidence or controls;
- effective scope and Verification Time;
- whether the target profile was intended at action time or only achieved later;
- claims whose support changed.

Later evidence may strengthen a current conclusion while the earlier Verification Report remains an authentic record of what was knowable at its Verification Time.

---

# 15.50 Downgrade and Graceful Degradation

A **downgrade** occurs when the Intended Trust Profile is not achieved and a lower or alternate profile is reported from the controls that remain satisfied.

Graceful degradation is permitted only when:

- governing Policy authorizes continued operation or evaluation;
- the fallback profile and version are defined;
- every mandatory fallback control is independently verified;
- the intended profile remains visible;
- failed, missing, unavailable, unsupported, or conflicting controls remain visible;
- affected claims and downstream restrictions are identified;
- no prohibited action proceeds under the weaker result;
- recovery and reassessment rules are applied.

```text
Useful Lower-Layer Evidence
    +
Explicit Assurance Loss
    =
Graceful Degradation

Useful Lower-Layer Evidence
    +
Hidden Assurance Loss
    =
Silent Downgrade
```

A verifier must not invent a fallback profile from arbitrary surviving controls. If no registered profile is fully satisfied, the outcome is partial or non-achievement with detailed control results.

---

# 15.51 Recovery, Late Evidence, and Reassessment

Some controls complete asynchronously.

Examples include:

- delayed Witness Observations;
- later Checkpoint issuance;
- External Anchor finality;
- restoration of a temporarily unavailable dependency;
- receipt of Historical Key State or Authority evidence;
- Preservation testing after a retention interval;
- migration or cryptographic renewal.

Profiles should define pending states, deadlines, maximum delay, and late-evidence admissibility.

Later evidence may trigger a new Verification run and a new Achieved Trust Profile. It must not silently modify an earlier result or pretend the later control existed before its actual time.

If a deadline is missed, a later control may support a different claim while failing the original timeliness requirement.

Reassessment must bind to the new evidence set, context, profile version, and Verification Time.

---

# 15.52 Profile Registry and Discovery

A **Profile Registry** publishes discoverable metadata and integrity material for profile definitions.

A registry entry should identify:

- profile namespace, identifier, version, and digest;
- Profile Authority and Authority evidence;
- canonical definition location or embedded representation;
- lifecycle status and effective boundaries;
- parent, component, successor, and mapping relationships;
- required TAIP, schema, algorithm, and control registries;
- conformance suite and test-vector references;
- preservation and mirror information;
- supersession, suspension, or withdrawal reasons.

Registry lookup success does not prove profile Authority or content integrity. Verifiers must validate the returned entry under the applicable trust model.

Historical registry snapshots or cryptographically bound entries are required where mutable current state could change interpretation.

Multiple registries may coexist. Conflict and precedence rules must be explicit.

---

# 15.53 Governance and Change Control

Profile governance should include:

- transparent proposal and review procedures;
- threat and privacy analysis;
- compatibility assessment;
- implementation experience and test evidence;
- approval thresholds and accountable signers;
- public or stakeholder notice where appropriate;
- scheduled and emergency release procedures;
- vulnerability handling;
- appeals, dispute, and errata mechanisms;
- historical preservation.

Changes that weaken assurance, expand permitted degradation, reduce independence, or deprecate algorithms require explicit security rationale.

Emergency changes may protect current operations but must not rewrite the semantics of already bound historical profiles. A new version, suspension record, or reassessment is required.

Governance capture is a security risk. Profile consumers should evaluate the authority, transparency, incentives, and failure modes of the Profile Authority and Registry Operator.

---

# 15.54 Deprecation, Migration, and Compatibility

Profile evolution must preserve historical meaning while allowing safer future requirements.

A migration specification should identify:

- source and target profile identities and versions;
- reason and authorization;
- compatibility or mapping relationship;
- evidence and controls carried forward;
- controls that require new evidence;
- algorithm, schema, registry, or infrastructure transitions;
- historical boundary and effective time;
- rollback or failure behavior;
- Migration Records and Verification requirements.

Deprecation does not invalidate evidence automatically. A deprecated algorithm may remain mathematically verifiable for a historical period while failing a current acceptance Policy.

A target profile is not achieved merely because a migration process was configured. The target controls must be evaluated.

Historical evidence may retain its original profile result and also receive a separately scoped current reassessment.

---

# 15.55 Conformance and Capability Declarations

Trust Profile work involves distinct conformance questions:

- **definition conformance** — whether a profile document follows TAIP profile semantics;
- **implementation capability** — whether software implements required controls and evaluation rules;
- **deployment conformance** — whether a deployed configuration and its control relationships satisfy operational requirements;
- **service conformance** — whether a control provider exposes the specified behavior and evidence;
- **evidence achievement** — whether particular evidence satisfies the profile;
- **Verification Engine conformance** — whether an engine produces required results for the profile.

```text
Conforming Product
≠
Conforming Deployment
≠
Profile Achieved for Particular Evidence
```

Capability declarations must identify exact profiles, versions, optional features, unsupported controls, environmental assumptions, and test-suite versions.

Conformance to a subset must not be represented as full support for a profile containing unimplemented mandatory semantics.

---

# 15.56 Certification, Attestation, and Assurance Claims

A certification or attestation may report that an implementation, deployment, operator, or process was assessed against a defined scope at a defined time.

It should identify:

- subject and assessed environment;
- assessor and Authority;
- profile and conformance-suite versions;
- assessment period and methods;
- evidence sampled or tested;
- exclusions, assumptions, exceptions, and findings;
- expiry, surveillance, and revocation rules.

Certification is separate from per-action profile achievement.

A certified deployment can produce evidence that fails a mandatory control. An uncertified implementation can produce evidence whose protocol controls verify. Certification may be required by Policy, but its role must be explicit.

Self-attestation, independent assessment, accreditation, and regulatory approval are distinct claims and must not share an ambiguous `certified` label.

---

# 15.57 Continuous Assurance and Monitoring

Some profile controls are point-in-time; others span an interval.

Continuous or periodic controls may include:

- Witness and Checkpoint cadence;
- key-custody posture;
- Registry and resolver availability;
- Preservation integrity and recoverability testing;
- External Anchor publication;
- incident, compromise, or conflict monitoring;
- dependency freshness;
- conformance surveillance.

A profile should define observation frequency, evidence, gap tolerance, failure state, remediation, and reassessment.

An earlier successful point-in-time Verification does not prove continuous satisfaction. Conversely, a later operational incident does not rewrite the valid bounded conclusion of an earlier report; it creates new evidence that may affect later or time-spanning claims.

Monitoring dashboards are not canonical assurance evidence unless their underlying events and results are preserved and verifiable.

---

# 15.58 Interoperability and Test Vectors

Interoperable profiles require shared conformance material for:

- definition parsing and canonicalization;
- identity, version, digest, and registry resolution;
- inheritance and composition;
- control applicability and criticality;
- thresholds, quorum, and independence;
- positive and negative evidence cases;
- missing, unavailable, unsupported, conflicting, and redacted states;
- exception and degradation behavior;
- Intended/Achieved evaluation;
- cross-version mapping and migration;
- control-level and aggregate Verification Reports.

Test vectors should include valid, invalid, boundary, adversarial, ambiguous, historical, and resource-limited cases.

Two conforming engines should produce equivalent canonical profile conclusions for equivalent evidence and context, even if their presentation differs.

Passing only positive examples is insufficient. Negative tests are essential to detect permissive fallback and silent downgrade.

---

# 15.59 Deployment Patterns and Profile Selection

Different actions within one deployment may use different profiles.

Illustrative patterns include:

- TP0 for transitional internal observability;
- TP1 for signed low-consequence internal decisions;
- TP2 for accountable histories requiring verifiable commitment;
- TP3 for cross-team or cross-Organization actions requiring independent observation;
- TP4 for high-value, long-lived, or dispute-prone actions requiring multi-domain resilience.

These examples do not assign universal risk or monetary thresholds.

Profile selection should consider:

- consequence and reversibility;
- adversary and correlated-failure model;
- counterparty and organizational boundaries;
- evidence lifetime and dispute horizon;
- availability tolerance;
- privacy and data-minimization constraints;
- dependency and operational cost;
- applicable contract, Policy, and regulation;
- feasibility of independent Verification.

The goal is proportionate, explicit assurance—not maximal controls for every event and not minimum controls chosen for convenience.

---

# 15.60 Anti-Patterns and Relationship to Other Controls

The following are architectural anti-patterns:

## Profile Name as Proof

Reporting `TP3` because a configuration field contains `TP3`, without evaluating mandatory controls.

## Numeric Ordering by Assumption

Assuming every profile ending in a larger number is a superset across namespaces or versions.

## Intended Equals Achieved

Displaying the selected profile after control failure without an explicit achieved result.

## Silent Downgrade

Continuing with weaker controls while preserving the stronger assurance label.

## Arbitrary Fallback

Calling a surviving control subset a lower profile when no registered fallback is fully satisfied.

## Instance Count as Independence

Counting replicas, processes, regions, or Signatures without evaluating common control.

## Opaque Threshold

Using an undocumented risk score, confidence value, or weighted quorum to determine canonical achievement.

## Latest-Version Substitution

Applying current profile semantics to a historical action bound to another version.

## Mutable Profile Definition

Changing requirements in place while retaining the same identifier and version.

## Hidden Extension

Depending upon proprietary mandatory semantics absent from the governed profile definition.

## Waiver as Satisfaction

Marking an unsatisfied control as satisfied because an exception exists, rather than evaluating the exception rule and reporting its effect.

## Certification as Per-Action Evidence

Treating a product or deployment certificate as proof that particular evidence achieved a profile.

## Control Count as Strength

Comparing profiles by number of controls rather than failure modes, scope, thresholds, and evidence.

## Stronger Means More Disclosure

Collecting or publishing unnecessary sensitive data merely because a higher assurance profile was selected.

## Profile Equals Compliance

Representing protocol profile achievement as automatic Legal Validity, Regulatory Compliance, business truth, or absence of fraud.

Trust Profiles compose the preceding architecture:

```text
Protocol Objects          provide typed evidence
Evidence Records          represent accountable assertions and events
Hash Chains               provide historical continuity
Witnesses                 provide bounded independent observation
Checkpoints and Anchors   provide historical commitments
Key Transparency          preserves Historical Key State
Preservation              maintains evidence and dependencies
Dispute Packs             provide portable bounded evidence sets
Verification              evaluates claims and controls
Trust Profiles            select and compose required assurance
```

A Trust Profile coordinates these controls. It does not replace them, change their bounded semantics, or manufacture evidence that is absent.

---

# Trust Profile Invariants

### INV-TP-001 — Requirements/Evidence Separation

A Trust Profile definition MUST remain distinguishable from evidence that its requirements were satisfied.

### INV-TP-002 — Intended/Achieved Separation

Intended Trust Profile MUST remain distinguishable from Achieved Trust Profile for every material scope.

### INV-TP-003 — Evidence-Based Achievement

Profile achievement MUST be derived from verified mandatory controls and dependencies rather than configuration, product tier, deployment claim, or profile label.

### INV-TP-004 — Explicit Identity

Every Trust Profile conclusion MUST identify the applicable profile namespace, stable identifier, and exact version.

### INV-TP-005 — Historical Immutability

Published profile semantics MUST NOT change in place under the same identity and version.

### INV-TP-006 — Historical Interpretation

Historical actions MUST remain interpretable under the profile, registries, algorithms, and dependencies applicable to their defined historical boundary.

### INV-TP-007 — Claim and Scope Boundedness

Profile selection and achievement MUST remain bounded to identified actions, claims, evidence populations, and time boundaries.

### INV-TP-008 — Control Explicitness

Every mandatory profile property MUST be represented by an explicit, testable control and evaluation rule.

### INV-TP-009 — Criticality Preservation

Mandatory, conditional, optional, recommended, prohibited, and informational semantics MUST remain distinguishable.

### INV-TP-010 — Dependency Explicitness

Dependencies whose state can affect profile achievement MUST remain explicit, versioned, and resolvable or be reported as unavailable or unsupported.

### INV-TP-011 — Control-State Fidelity

Satisfied, failed, incomplete, indeterminate, unsupported, unavailable, conflicting, not-applicable, and not-evaluated control states MUST NOT be silently conflated.

### INV-TP-012 — No Silent Downgrade

Failure of an Intended Trust Profile control MUST remain visible and MUST NOT preserve the intended profile as achieved.

### INV-TP-013 — Fallback Integrity

A lower or alternate profile MUST be reported as achieved only when all of its own mandatory requirements are verified.

### INV-TP-014 — Lower-Layer Preservation

Failure of a higher-assurance control MUST NOT erase unrelated valid lower-layer evidence or supported claims.

### INV-TP-015 — Numeric Neutrality

A numeric profile suffix MUST NOT imply ordering, inheritance, or equivalence absent a governed declaration within an identified profile family.

### INV-TP-016 — Inheritance Integrity

A derived profile MUST NOT claim unqualified inheritance while silently weakening a mandatory base requirement.

### INV-TP-017 — Composition Determinism

Profile composition MUST resolve identity, version, duplicate controls, conflicts, thresholds, dependencies, and outcome rules deterministically.

### INV-TP-018 — Independence Evidence

Claims of independence MUST be evaluated from explicit Control Domain and correlated-failure criteria rather than role names, instance counts, or deployment labels.

### INV-TP-019 — Replication/Independence Separation

Replication, geographic distribution, or operational redundancy MUST NOT automatically be represented as independent assurance.

### INV-TP-020 — Count/Quorum Separation

Raw Signature, Witness, service, or instance count MUST NOT substitute for eligibility, independence, diversity, and quorum evaluation.

### INV-TP-021 — Exception Visibility

Exceptions and waivers MUST remain attributable, bounded, historically visible, and distinct from ordinary control satisfaction.

### INV-TP-022 — Non-Retroactivity

Later profile, Policy, registry, mapping, exception, or configuration changes MUST NOT silently rewrite the Intended or Achieved profile of an earlier action.

### INV-TP-023 — Reassessment Non-Rewrite

Later evidence or changed context MUST create a new profile evaluation rather than mutate an earlier Verification result.

### INV-TP-024 — Validity/Completeness/Achievement Separation

Object Validity, evidence Completeness, control satisfaction, and Trust Profile achievement MUST remain separately evaluable.

### INV-TP-025 — Protocol/Business Separation

Trust Profile achievement MUST NOT automatically establish source truth, execution, settlement, Legal Validity, Regulatory Compliance, or absence of fraud.

### INV-TP-026 — Conformance/Achievement Separation

Implementation, service, deployment, or certification conformance MUST remain distinguishable from profile achievement for particular evidence.

### INV-TP-027 — Governance Accountability

Profile Authority, Registry operation, selection Policy, change control, and lifecycle transitions MUST remain attributable to governed roles and evidence.

### INV-TP-028 — Registry/Authority Separation

Successful registry retrieval MUST NOT by itself establish profile Authority, integrity, applicability, or current acceptability.

### INV-TP-029 — Privacy Proportionality

Stronger assurance MUST NOT be interpreted as requiring unnecessary collection, retention, linkage, or disclosure of sensitive information.

### INV-TP-030 — Unsupported Fail-Safe

Unknown or unsupported mandatory profile semantics MUST NOT be ignored, guessed, or treated as satisfied.

### INV-TP-031 — Reproducible Evaluation

Equivalent evidence under equivalent Verification Contexts MUST produce equivalent canonical profile-control and achievement conclusions.

### INV-TP-032 — Implementation Neutrality

No vendor, product, API, deployment topology, certificate, or undocumented implementation behavior MUST define Trust Profile meaning.

---

# Architectural Requirements

### REQ-TP-001

TAIP MUST define a canonical, versioned Trust Profile definition model with stable profile and control identifiers.

### REQ-TP-002

Every profile definition MUST identify its namespace, stable identifier, exact version, Profile Authority, lifecycle state, and assurance objective.

### REQ-TP-003

Every profile definition MUST identify its applicable scope, supported claim or action classes, and any explicit exclusions.

### REQ-TP-004

Profile content whose alteration could affect interpretation or achievement MUST be canonically integrity-protected or bound to an immutable content identifier.

### REQ-TP-005

The Authority to issue, revise, suspend, deprecate, supersede, or withdraw a profile MUST be verifiable under the applicable governance model.

### REQ-TP-006

A material semantic change to a profile MUST create a new version and MUST NOT mutate an already published version in place.

### REQ-TP-007

Historical profile versions and mandatory dependencies MUST remain preservable and resolvable for their required Evidence Lifetime.

### REQ-TP-008

Lifecycle transitions MUST identify effective boundaries, responsible Authority, reason, predecessor state, and integrity protection.

### REQ-TP-009

Every mandatory, conditional, optional, recommended, prohibited, or informational requirement MUST declare its criticality explicitly.

### REQ-TP-010

Every profile control MUST identify a stable control ID, version, purpose, applicability rule, required inputs, evaluation semantics, and effect on outcomes.

### REQ-TP-011

Controls affecting canonical conclusions MUST use measurable criteria rather than undefined terms such as `strong`, `trusted`, `sufficient`, or `appropriate`.

### REQ-TP-012

Conditional controls MUST define deterministic predicates, input provenance, missing-data behavior, and boundary semantics.

### REQ-TP-013

Thresholds MUST define units, population, comparison operation, inclusivity, precision, time window, and missing or conflicting input behavior.

### REQ-TP-014

Cardinality rules MUST define whether counts apply to observations, Signatures, identities, Organizations, Control Domains, time intervals, or another explicit population.

### REQ-TP-015

Prohibited controls, algorithms, combinations, and degradation paths MUST produce explicit nonconformance when used within their prohibited scope.

### REQ-TP-016

Every mandatory profile dependency MUST identify stable identity, exact version, integrity, applicability, and resolution rules.

### REQ-TP-017

Profile dependency resolution MUST detect unsupported versions, conflicting definitions, missing mandatory resources, cycles, and circular assurance.

### REQ-TP-018

Mutable `latest`, unversioned URLs, implementation defaults, or current registry state MUST NOT replace historically bound dependencies in a profile conclusion.

### REQ-TP-019

Profile mapping rules MUST identify the governed action, claim, risk, Policy, or context inputs used to select the Intended Trust Profile.

### REQ-TP-020

Conflicting profile-selection rules MUST be resolved by explicit governed precedence or reported as a conflict.

### REQ-TP-021

The Intended Trust Profile MUST be selected and bound no later than the accountability-critical transition it governs, unless the result is explicitly labeled a later reassessment.

### REQ-TP-022

An Intended Trust Profile binding MUST identify profile ID, version, scope, selecting Authority or Policy, selection boundary, and permitted degradation behavior.

### REQ-TP-023

The profile binding MUST be integrity-protected sufficiently to detect profile substitution, version substitution, or scope alteration.

### REQ-TP-024

One profile result MUST NOT be applied to another action, claim, population, historical boundary, or evidence set without verifying scope equivalence.

### REQ-TP-025

A Verification Engine MUST validate profile definition identity, integrity, Authority, lifecycle applicability, and dependency closure before using its aggregation rules.

### REQ-TP-026

The Verification Engine MUST evaluate every applicable mandatory and prohibited control of the Intended Trust Profile.

### REQ-TP-027

Control evaluation MUST preserve satisfied, failed, incomplete, indeterminate, unsupported, unavailable, conflicting, not-applicable, and not-evaluated states where applicable.

### REQ-TP-028

Every control result MUST identify the evaluated control version, scope, evidence, dependencies, Verification Context, and reason sufficient for reproducibility.

### REQ-TP-029

An unknown or unsupported mandatory control, extension, algorithm, proof, or composition rule MUST produce an Unsupported profile result.

### REQ-TP-030

An unavailable mandatory dependency MUST NOT be reported as a satisfied control or as evidence contradiction unless available evidence actually contradicts the rule.

### REQ-TP-031

Evidence Completeness MUST be evaluated for the defined claims, scope, historical boundary, and Intended Trust Profile.

### REQ-TP-032

Object Validity, evidence Completeness, individual control results, claim outcomes, and profile achievement MUST be reported separately.

### REQ-TP-033

An Achieved Trust Profile MUST identify the exact profile ID and version, evaluated scope, Verification Context, Verification Time, and supporting control results.

### REQ-TP-034

A profile MUST be reported as achieved only when every applicable mandatory control and dependency satisfies its governed composition rules.

### REQ-TP-035

Configuration, deployment intent, product licensing, service labels, implementation capability, or certification MUST NOT substitute for evidence-based achievement.

### REQ-TP-036

When the Intended Trust Profile is not achieved, the result MUST identify every material unsatisfied control and its state.

### REQ-TP-037

A downgrade result MUST identify the intended profile, achieved fallback profile if any, applied downgrade rule, and effect on each material claim.

### REQ-TP-038

A fallback profile MUST be reported as achieved only after all of its own mandatory requirements are independently evaluated and satisfied.

### REQ-TP-039

If no registered fallback profile is fully satisfied, the result MUST report partial or non-achievement rather than invent an unnamed achieved level.

### REQ-TP-040

Operational degradation MUST identify authorizing Policy, trigger, minimum surviving controls, duration, restrictions, recovery deadline, and profile consequence.

### REQ-TP-041

Failure of a higher-layer control MUST propagate only to dependent profiles and claims and MUST NOT erase unrelated valid lower-layer results.

### REQ-TP-042

Late evidence or changed context MUST produce a new Verification result and MUST NOT silently rewrite the earlier Achieved Trust Profile.

### REQ-TP-043

Profiles with asynchronous controls MUST define pending states, deadlines, late-evidence rules, missed-deadline behavior, and reassessment requirements.

### REQ-TP-044

Claims of Witness, verifier, Preservation, or infrastructure independence MUST be evaluated against explicit historical Control Domain criteria and evidence.

### REQ-TP-045

Raw instance, service, Signature, or Witness counts MUST NOT satisfy a quorum that requires eligibility, independence, diversity, or role composition without evaluating those properties.

### REQ-TP-046

An independence rule MUST define relevant control dimensions, required evidence, evaluation boundary, unknown-state behavior, and conflict handling.

### REQ-TP-047

Profile inheritance MUST bind exact base versions and MUST define inherited, added, specialized, and prohibited override semantics.

### REQ-TP-048

Inheritance and composition resolution MUST detect cycles, diamond conflicts, incompatible parameters, duplicate controls, and unsatisfied component dependencies.

### REQ-TP-049

A composed profile MUST have a deterministic identity that binds every component profile, version, parameter, precedence rule, and resulting control set.

### REQ-TP-050

A derived or composed profile MUST NOT claim a base profile relationship if it weakens a mandatory base control outside an explicitly permitted and identified variation.

### REQ-TP-051

Profile equivalence or implication mappings MUST identify both exact profile versions, compared scope, control correspondence, unmatched requirements, assumptions, authority, and direction.

### REQ-TP-052

Profile order, inheritance, equivalence, or relative strength MUST NOT be inferred solely from display names, numeric suffixes, marketing tiers, or control counts.

### REQ-TP-053

A Profile Registry entry MUST identify profile identity, version, digest, Authority, lifecycle state, dependency references, and canonical definition location or representation.

### REQ-TP-054

Registry responses used for historical or reproducible conclusions MUST be integrity-validated and versioned, snapshotted, or otherwise bound to the evaluated context.

### REQ-TP-055

Conflicting profile definitions or registry entries for the same identity and version MUST produce an explicit conflict and MUST NOT be resolved by undocumented lookup order.

### REQ-TP-056

Custom profiles MUST use governed namespaces, identify all mandatory extensions, and MUST NOT redefine standard profile identifiers with incompatible semantics.

### REQ-TP-057

Private mandatory profile semantics inaccessible to a verifier MUST produce an Unsupported or Incomplete result rather than presumed conformance.

### REQ-TP-058

Capability and conformance declarations MUST identify exact profile versions, implemented options, environmental assumptions, unsupported mandatory features, and applicable test-suite versions.

### REQ-TP-059

Profile conformance suites MUST include positive, negative, boundary, historical, cross-version, degradation, conflict, unsupported, and independence test cases.

### REQ-TP-060

Certification and attestation claims MUST identify subject, assessor, scope, profile and suite versions, assessment period, exclusions, exceptions, and validity conditions.

### REQ-TP-061

Privacy mechanisms, redaction, selective disclosure, or access restriction MUST NOT preserve full profile achievement unless the profile defines verifiable substitute evidence for every affected mandatory control.

### REQ-TP-062

Profile achievement and conformance reports MUST NOT represent protocol assurance as automatic business truth, execution, settlement, Legal Validity, Regulatory Compliance, or absence of fraud.

---

# Security Considerations

Trust Profiles concentrate assurance policy into reusable definitions. That makes them powerful coordination mechanisms and high-value targets for substitution, weakening, ambiguity, governance capture, and misleading reporting.

## Profile Substitution

An attacker may replace the intended profile with a weaker profile or an older vulnerable version. Integrity-protected bindings to exact identity, version, scope, Policy, and selection boundary are required.

## Version Confusion

Display names such as `TP3` can conceal different namespaces or major versions. Verifiers must compare canonical identifiers and exact versions rather than aliases or lexical similarity.

## Mutable Definition Attack

A Registry Operator may change profile content behind an unchanged URL. Immutable content identifiers, profile Signatures, historical snapshots, Checkpoints, and Preservation constrain in-place semantic rewrite.

## Registry Split View

Different verifiers may receive conflicting definitions or lifecycle states for the same profile version. Registry gossip, Witnessing, signed snapshots, conflict evidence, Checkpoints, and multiple governed resolvers can expose equivocation.

## Governance Capture

A captured Profile Authority may weaken controls, suppress vulnerabilities, approve friendly mappings, or broaden degradation. Transparent change records, multi-party approval, review periods, independent security analysis, and preserved historical versions reduce hidden capture.

## Authority Laundering

A valid Signature over a profile or exception may be presented as proof of governance Authority. Profile issuance, modification, waiver, and withdrawal require separate Historical Key State, Key Purpose, Authority, and Policy evaluation.

## Silent Downgrade

Operational pressure may cause a system to bypass Witnessing, Checkpointing, anchoring, or Preservation while continuing to display the intended profile. Canonical reports must derive achieved state from controls and make downgrade prominent to downstream consumers.

## Fallback Shopping

A producer may search many profiles after failure and report whichever profile produces the most favorable label. Permitted fallbacks should be bound by the intended profile or Policy before the event, and arbitrary later reassessment must be identified as such.

## Numeric-Level Inflation

An issuer may create a custom `TP9` or similarly impressive label with weak requirements. Names and numeric suffixes convey no assurance outside a governed namespace, version, and control definition.

## Control Omission

A malicious or defective parser may skip a mandatory control, unknown extension, or component profile. Criticality semantics, dependency closure, negative tests, and control-level reporting must fail safely.

## Optional-Control Marketing

An implementation may advertise optional controls as if every action received them. Capability declarations and per-action results must distinguish available, selected, evaluated, and satisfied controls.

## Independence Laundering

Several nominal Witnesses, regions, providers, or certificates may share administrators, keys, pipelines, funding, or incident authority. Independence predicates require evidence of actual control relationships and conservative handling of unknown relationships.

## Sybil Quorum

An attacker may create many eligible-looking identities under one Control Domain. Quorum rules should constrain domain contribution, eligibility, identity issuance, economic incentives, and historical population snapshots.

## Opaque Score Manipulation

A hidden weighting model can turn failed mandatory controls into a high aggregate score. Scores may supplement analysis but cannot override deterministic profile requirements, and protocol-relevant models must be versioned and explainable.

## Threshold Boundary Exploitation

Ambiguous units, rounding, time zones, inclusivity, or missing-data defaults can change profile selection or achievement. Threshold semantics must be exact and cross-implementation test vectors must cover boundaries.

## Exception Abuse

Broad, permanent, retroactive, or self-approved waivers can eliminate meaningful assurance. Exceptions should be narrow, time-bounded, authorized, integrity-protected, visible, and evaluated as governed alternate rules rather than erased failures.

## Dependency Poisoning

Profile evaluation may retrieve schemas, algorithms, registries, or base profiles from compromised resolvers. Stable identity, cryptographic integrity, authoritative-source rules, snapshots, and conflict handling are required.

## Circular Assurance

A profile, certificate, Registry, or Authority may rely on itself through a dependency cycle. Graph evaluation must reject circular proof that provides no independently established trust foundation.

## Algorithm Downgrade

A profile may permit several algorithms, and an implementation may choose the weakest or fall back after failure. Selection, negotiation, deprecation, and failure behavior must be explicit and resistant to downgrade.

## Current-State Substitution

Current profiles, keys, Policies, Witness eligibility, or registries may be used to reinterpret historical evidence. Verification must bind the historically applicable state and distinguish later reassessment.

## Profile Mapping Abuse

A one-way, partial, or assumption-heavy mapping may be marketed as exact equivalence. Mappings need direction, scope, unmatched controls, authority, version, and evidence, and must fail closed outside their stated domain.

## Certification Laundering

A product certificate may be presented as proof that every action achieved a profile. Per-action control evidence remains necessary, and assessment scope, period, environment, and exceptions must remain visible.

## Conformance Claim Inflation

An implementation that handles only Signatures may claim support for a profile requiring Chains, Witnesses, Preservation, and rich outcomes. Exact capability declarations and negative conformance tests prevent subset support from masquerading as full support.

## Late-Evidence Backdating

A later Witness Observation, Anchor confirmation, or recovered dependency may be presented as if it existed at action time. Evidence time semantics and reassessment lineage must preserve when each control actually became supportable.

## Monitoring Suppression

A time-spanning profile may appear achieved because monitoring gaps or failed Preservation tests were omitted. Continuous controls require explicit cadence, gap, incident, and not-evaluated evidence.

## Report Flattening

APIs or user interfaces may reduce a detailed profile result to a green badge. Consumers should preserve intended and achieved identities, control failures, claim effects, limitations, and Verification Time.

## Denial of Assurance

Attackers may make optional or mandatory assurance services unavailable to force operational shutdown or downgrade. Profiles should define availability, queuing, fallback, recovery, and downstream restrictions without rewarding silent degradation.

## Resource Exhaustion

Complex inheritance graphs, large control sets, many candidate fallbacks, or expensive proofs may exhaust Verification Engines. Evaluators must enforce safe limits and report incomplete or unsupported evaluation rather than partial success.

## Malicious Profile Content

Profile documents are untrusted inputs. Parsers must reject ambiguous encodings, duplicate identifiers, unsafe references, executable content, excessive nesting, and unsupported critical extensions before evaluation.

## Time and Freshness Manipulation

Clock changes may alter profile selection, certificate validity, Witness eligibility, Checkpoint cadence, retention, or exception expiry. Profiles must define time sources, precision, ordering evidence, and boundary behavior.

## Cross-Tenant Profile Leakage

A resolver or cache may return another tenant's private profile, Policy, or mapping. Namespace, authorization, cache-key, integrity, and scope validation must prevent cross-context substitution.

## Strongest-Profile Denial

Automatically selecting the numerically highest available profile can cause infeasible controls, unnecessary data collection, or denial of service. Selection must remain Policy- and risk-bounded rather than driven by label maximization.

---

# Privacy Considerations

Trust Profiles can improve minimization by defining exactly which evidence a claim requires. They can also expand surveillance if stronger assurance is incorrectly equated with collecting everything.

## Profile Selection Sensitivity

The selected profile may reveal transaction value, risk classification, regulatory status, dispute likelihood, or the sensitivity of an action. Profile identifiers and mappings should be disclosed only to parties that need them.

## Control-Domain Disclosure

Independence evidence may reveal ownership, administrators, infrastructure accounts, jurisdictions, contracts, conflicts, and incident-response relationships. Profiles should permit privacy-preserving or selectively disclosed evidence where the independence predicate can still be verified.

## Witness and Quorum Linkage

Repeated Witness identities and quorum composition can expose organizational relationships and activity patterns. Rotating identifiers, scoped credentials, aggregation, and minimum-disclosure proofs may reduce linkage without obscuring eligibility or accountability.

## Registry Query Leakage

Queries for profiles, key history, eligibility, or exceptions may reveal which action or party is under review. Local mirrors, private resolvers, batching, caching, and offline Dispute Packs may reduce observable query patterns.

## Profile Dependency Graph Exposure

The dependency graph can reveal Policies, vendors, security architecture, key custody, external anchors, and preservation topology. Pack compartmentalization and protected references should disclose only the dependencies needed for the authorized claim.

## Exception Confidentiality

Waivers may contain incident, vulnerability, personnel, or commercial information. A profile may permit a minimally disclosed proof of authorized exception while preserving detailed reason material for authorized reviewers.

## Retention Proportionality

Higher assurance may require longer preservation, but retention must remain tied to Evidence Lifetime, claim purpose, legal obligations, and deletion rules. Indefinite retention is not a default assurance property.

## Selective Disclosure

Profiles should identify whether commitments, redacted representations, zero-knowledge proofs, attestations, or compartmentalized evidence can satisfy a control. The proof must bind to the same subject, scope, and historical state as the undisclosed evidence.

## Redaction Semantics

Redaction, encryption, withholding, proven absence, and unavailability must remain distinct. A verifier may report a bounded result without learning protected content only when the profile provides sufficient proof semantics.

## Cross-Profile Correlation

Stable profile IDs, action IDs, Witness sets, and registry references can correlate activity across Organizations. Pairwise or scoped identifiers may reduce correlation where global linkage is not required for Verification.

## Conformance Assessment Data

Deployment assessments may expose network topology, key custody, vulnerabilities, personnel roles, and control failures. Certificates should disclose conclusions and material limitations without publishing unnecessary operational secrets.

## Verification Report Disclosure

Control-level reports can reveal more than the final claim. Access-controlled detailed reports and minimally disclosed derived reports may coexist, but the derived report must bind to the canonical result and must not hide material failure from an authorized relying party.

## Degradation Signaling

Publicly revealing a downgrade may expose a live outage or control weakness. Profiles may support audience-specific detail, but the party relying upon the assurance must receive enough information to avoid acting on a false stronger result.

## Deletion and Historical Accountability

Privacy deletion obligations may conflict with long-term accountability. Profiles should define what can be deleted, transformed, cryptographically erased, retained as a commitment, or placed under legal hold, and how deletion affects later achievement.

## Data Minimization as a Control

Profiles may treat minimization itself as an assurance property. Evidence Producers should avoid including raw personal or confidential data when a typed assertion, commitment, reference, or selective proof supports the bounded claim.

---

# Design Rationale

TrustAgentAI needs Trust Profiles because no single deployment pattern or control set is proportionate for every Accountable Action.

A low-consequence internal decision, a high-value cross-organizational transfer, a long-lived governance action, and a dispute-prone autonomous execution face different adversaries, evidence lifetimes, availability constraints, and privacy costs. Treating them identically would either under-protect consequential actions or overburden ordinary ones.

Profiles therefore make assurance composable.

They translate an objective into verifiable requirements across cryptography, identity, Authority, history, Witnessing, external commitment, Preservation, portability, and Verification. This preserves the distinction among control families while allowing them to form a coherent result.

The Intended/Achieved distinction is central.

Configuration records what a system hoped or was required to do. Evidence records what can later be supported. Distributed services fail, dependencies disappear, and controls complete asynchronously. The architecture preserves useful lower-layer evidence without allowing operational continuity to become false assurance.

The `TP0` through `TP4` reference progression supplies a common vocabulary:

1. explicit structured evidence;
2. cryptographic attribution;
3. historical continuity;
4. independent observation;
5. multi-domain durable assurance.

The progression is intentionally architectural rather than a universal risk score. Exact registered profiles must define measurable controls, and unrelated families may remain incomparable.

Versioning and preservation are required because profile meaning is itself a Verification Dependency. Evidence can remain byte-for-byte intact yet become uninterpretable if the governing profile, schema, algorithm, Registry, or mapping disappears or is silently replaced by a current version.

Independence is modeled explicitly because instance count is easy to manipulate. Different hostnames, keys, regions, or legal brands may still share effective control. Profiles need evidence-based predicates tied to relevant correlated-failure dimensions.

Conformance is separated from achievement because products, deployments, services, and individual evidence answer different questions. A conforming engine may correctly report failure. A certified deployment may produce incomplete evidence. A valid Evidence Record may fail a profile whose required Chain, Witness, or Preservation controls are absent.

Finally, profile achievement remains a protocol conclusion. It can strengthen audits, contracts, investigations, and compliance processes, but it does not decide business truth or law. Keeping that boundary explicit prevents a technical label from becoming an unqualified institutional claim.

---

# Summary

A Trust Profile is a versioned, governed set of measurable assurance requirements for a bounded action, claim, evidence population, or protocol state.

Trust Profiles:

- identify required controls, evidence, dependencies, thresholds, and independence criteria;
- bind an assurance objective to exact profile identity and version;
- distinguish Intended Trust Profile from Achieved Trust Profile;
- derive achievement from verified mandatory controls;
- preserve detailed failure, uncertainty, and dependency states;
- permit graceful degradation only through explicit governed fallback;
- support inheritance, composition, extension, mapping, migration, and registries;
- separate per-evidence achievement from implementation, deployment, service, and certification conformance;
- preserve historical profile semantics and Verification Dependencies;
- remain implementation-neutral and claim-bounded.

The reference progression is:

```text
TP0  Explicit evidence boundary
TP1  Cryptographically attributable evidence
TP2  Verifiable historical continuity
TP3  Independently observed accountability
TP4  Multi-domain durable assurance
```

These identifiers acquire exact normative meaning only through an applicable registered Trust Profile specification.

The governing rule is:

```text
Intended Assurance
    +
Verified Controls and Evidence
    +
Explicit Dependencies and Context
    =
Achieved Trust Profile or Honest Non-Achievement
```

Trust Profiles do not manufacture trust by name.

They make assurance requirements explicit, composable, historically interpretable, and independently verifiable—and make every failure to achieve them visible.
