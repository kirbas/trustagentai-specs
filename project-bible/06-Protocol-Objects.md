# Chapter 6 — Protocol Objects

> **Protocol Objects turn accountability claims into structured, addressable, and independently interpretable evidence.**

## Purpose

This chapter defines the common object model used by TrustAgentAI and the TrustAgentAI Interoperability Protocol (TAIP).

Protocol Objects are the structured, identifiable, versioned, and cryptographically governable units through which TrustAgentAI represents:

- intent;
- Authority and Authorization;
- Policy evaluation;
- execution;
- observation;
- Accountability Evidence;
- historical Commitment;
- key lifecycle state;
- Preservation;
- evidence packaging;
- Verification;
- protocol evolution.

Protocol Objects provide the common language through which Evidence Producers, Evidence Registries, Commitment Services, Chain operators, Witnesses, Checkpoint Authorities, Key Transparency services, Preservation Services, Dispute Pack assemblers, and Verification Engines communicate.

This chapter establishes:

- the Protocol Object taxonomy;
- common object semantics;
- identifier and reference rules;
- versioning and extension behavior;
- canonicalization and cryptographic protection boundaries;
- lifecycle and immutability rules;
- validation layers;
- cross-object invariants;
- architectural requirements for interoperable implementations.

This chapter builds upon the system model in [05-System-Overview.md](05-System-Overview.md) and the principles in [04-Design-Principles.md](04-Design-Principles.md).

It uses the canonical terms defined in [Terminology.md](Terminology.md) and [Acronyms.md](Acronyms.md).

This chapter defines architectural object semantics.

It does not define final field names, concrete schemas, media types, wire encodings, transport procedures, database layouts, or cryptographic suites.

Those details belong to later chapters and to TAIP.

---

# 6.1 Protocol Object Definition

A **Protocol Object** is a governed unit of protocol meaning that can be created, identified, referenced, protected, exchanged, preserved, and evaluated according to TAIP rules.

A Protocol Object is more than an arbitrary data structure.

Its definition establishes:

- what the object asserts or represents;
- which role may create it;
- how its type and version are identified;
- which information forms its canonical representation;
- how its identifier is interpreted;
- which cryptographic protections apply;
- how it references other objects;
- which lifecycle states may apply;
- which validation rules determine object-level validity;
- how later objects may depend upon it;
- how its meaning remains interpretable over time.

```text
Structured Data
      +
Governed Type and Version
      +
Defined Semantics
      +
Deterministic Cryptographic Boundary
      +
Lifecycle and Validation Rules
      =
Protocol Object
```

Operational records, database rows, log events, and application messages are not automatically Protocol Objects.

They become Protocol Objects only when they conform to an applicable governed object definition.

---

# 6.2 Scope

This chapter applies to Protocol Objects that are:

- created or consumed by TrustAgentAI roles;
- required to support an Accountability Claim;
- exchanged between independent implementations;
- incorporated into historical Commitment;
- referenced by Witnesses or Checkpoints;
- included in a Dispute Pack;
- preserved as part of a Verification Dependency Graph;
- evaluated by a Verification Engine;
- used to govern protocol evolution.

This chapter covers logical object semantics rather than implementation storage.

It does not require every internal operational event to become a Protocol Object.

It does not require every Protocol Object to contain the same properties.

It does not require every object family to use a separate transport message or database table.

It does require that objects carrying equivalent protocol meaning remain interoperably distinguishable and interpretable.

---

# 6.3 Why a Common Object Model Is Required

Accountability evidence crosses systems, Organizations, vendors, and time periods.

Without a common object model, independent implementations may disagree about:

- what was asserted;
- which bytes were protected;
- which version applied;
- which identifier names the object;
- whether two references identify the same object;
- whether an object was finalized or merely drafted;
- whether a later object corrects, supersedes, or merely comments upon an earlier object;
- whether an unknown extension changes mandatory meaning;
- whether an object is valid but incomplete;
- whether preserved bytes remain interpretable.

The common object model reduces this ambiguity by separating protocol meaning from incidental implementation choices.

```text
Producer Implementation A
            │
            ▼
Governed Protocol Object
            │
            ├────────► Registry Implementation B
            ├────────► Witness Implementation C
            └────────► Verifier Implementation D
```

Interoperability depends upon B, C, and D interpreting the object consistently without executing A's proprietary application code.

---

# 6.4 Protocol Object Properties

A conforming Protocol Object possesses governed semantics for the properties applicable to its type.

These may include:

- object type;
- object version;
- applicable protocol version;
- stable identifier;
- producer or issuer identity;
- subject identity or scope;
- payload or statement body;
- time values with explicit meanings;
- typed references;
- extension data;
- cryptographic protection metadata;
- Signature or other integrity evidence;
- lifecycle evidence;
- status or relationship to prior objects.

These are logical properties, not mandated field names.

TAIP may encode them differently for different object families where interoperability remains unambiguous.

Not every object requires every property.

For example, a Chain Entry and a Verification Report have different roles and may require different information.

The applicable object type definition identifies which properties are mandatory, optional, prohibited, derived, or externally resolved.

---

# 6.5 Object Families

TrustAgentAI organizes Protocol Objects into semantic families.

| Object family | Primary purpose | Representative objects or classes |
|---|---|---|
| Accountability event | Represents an accountable event or assertion | Evidence Record, Intent Evidence, Authorization Evidence, Policy Evaluation Evidence, Execution Evidence |
| Historical Commitment | Binds evidence into protected ordered history | Chain Entry, Commitment Receipt |
| Independent observation | Represents observation by a Witness | Witness Observation |
| Historical boundary | Commits to a defined aggregate or chain state | Checkpoint, Anchor Evidence |
| Identity and key history | Preserves identity-key lifecycle evidence | Key Transparency Record |
| Preservation and transition | Supports retention, custody, renewal, and migration | Preservation Evidence, Migration Record |
| Portable packaging | Describes an exported evidence collection | Dispute Pack Manifest |
| Verification | Represents durable protocol evaluation results | Verification Report |
| Governance and dependency | Defines or preserves interpretation material | Trust Profile, schema, registry, extension, and algorithm records where specified |

The taxonomy is semantic rather than storage-specific.

TAIP MAY define a semantic class as:

- a distinct Protocol Object type;
- a governed subtype of an Evidence Record;
- a typed payload within another Protocol Object;
- a separately protected dependency.

The chosen representation MUST preserve the class's meaning and validation rules.

---

# 6.6 Accountability Event Objects

Accountability Event Objects represent events, states, decisions, assertions, or transitions relevant to an Accountable Action.

They may represent:

- expressed intent;
- requested action;
- Authority delegation;
- Authorization decision;
- Policy evaluation;
- Human approval;
- Agent decision;
- tool invocation;
- execution request;
- execution acceptance or rejection;
- settlement or business result;
- correction;
- administrative change.

The architecture does not assume that every event is independent.

Accountability Event Objects may form a causal graph through explicit references.

```text
Intent Evidence
      │
      ▼
Authorization Evidence
      │
      ▼
Policy Evaluation Evidence
      │
      ▼
Execution Evidence
```

The sequence is illustrative.

Real workflows may branch, merge, retry, reject, escalate, or require additional Actors.

---

# 6.7 Intent Evidence

Intent Evidence represents a proposed, requested, planned, or instructed action before its final business effect.

It may bind information concerning:

- the requested action;
- the requesting Actor or Protocol Identity;
- the intended target;
- parameters or constraints;
- applicable Authority and Policy references;
- predecessor events;
- required approvals;
- claimed Event Time or creation time;
- the intended Trust Profile.

Intent Evidence does not prove that the action was authorized, executed, accepted, or settled.

```text
Intent
≠
Authorization
≠
Execution
≠
Outcome
```

TAIP may represent Intent Evidence as a specialized Evidence Record rather than as a universally separate wire object.

The semantic distinction must remain available where required for Accountability Claims.

---

# 6.8 Authorization Evidence

Authorization Evidence represents a claim that an Actor, Agent, service, or key was permitted to perform or approve a defined action under specified Authority and Policy.

It may bind:

- the authorizing Protocol Identity;
- the authorized subject;
- the action scope;
- the Authority source;
- the applicable Policy and version;
- conditions or limits;
- validity interval or decision time;
- Key Purpose;
- approval evidence;
- references to intent or supporting state.

A valid Signature on Authorization Evidence does not by itself prove that the signer possessed the claimed Authority.

Authorization Verification may require historical identity, key, Policy, delegation, and organizational evidence.

Authorization Evidence must remain distinguishable from authentication evidence and from possession of a signing key.

---

# 6.9 Policy Evaluation Evidence

Policy Evaluation Evidence represents the result of applying a defined Policy to defined inputs under a defined evaluation context.

It may identify or bind to:

- the Policy identifier and version;
- the evaluator identity;
- the relevant inputs or their integrity references;
- the evaluation result;
- matched or failed conditions;
- exceptions or overrides;
- the evaluation time;
- references to intent, Authority, or approval evidence.

Policy Evaluation Evidence supports the claim that a particular evaluation occurred.

It does not automatically prove that:

- the Policy was lawful;
- the Policy was correctly authored;
- the evaluator implementation was free of defects;
- the inputs represented complete business reality;
- the resulting action was legally authorized.

Historical Verification must use the Policy version applicable to the evaluated event rather than silently substituting the current Policy.

---

# 6.10 Execution Evidence

Execution Evidence represents a request, acceptance, rejection, completion, settlement, or other business effect reported by an execution system or counterparty.

It may bind:

- the execution request;
- the executing or reporting Protocol Identity;
- an external transaction or operation reference;
- status with explicit semantics;
- structured result information;
- event and record times;
- error or rejection information;
- references to Intent, Authorization, Policy, or prior execution state.

Execution Evidence from an independent financial system may strengthen an Accountability Claim.

Its meaning still depends upon the reporting system, reference semantics, identity evidence, and applicable Trust Profile.

An execution request is not execution acceptance.

Acceptance is not settlement.

Settlement is not necessarily final business correctness.

---

# 6.11 Evidence Record

An **Evidence Record** is the primary Protocol Object representing an Accountable Action or another accountability-relevant event.

An Evidence Record may represent or bind together one or more semantic classes described above while preserving their distinctions.

At architectural level, an Evidence Record may contain or reference information concerning:

- the event or action;
- the responsible or reporting Protocol Identity;
- relevant Authority;
- applicable Policy;
- structured inputs and outputs;
- causal predecessors and successors;
- execution state;
- protocol and object versions;
- time semantics;
- cryptographic protection;
- intended assurance.

Chapter 7 defines the detailed Evidence Record specification.

This chapter establishes only the common rules shared with other Protocol Objects.

An Evidence Record is not required to contain every operational detail.

It should contain or reference information sufficient for applicable Accountability Claims while minimizing unnecessary sensitive data.

---

# 6.12 Historical Commitment Objects

Historical Commitment Objects establish that defined evidence entered protected historical state.

Their purposes include:

- ordered Commitment;
- continuity;
- linkage to prior state;
- evidence of acceptance or Commitment;
- stable references to chain state;
- support for Witnessing and Checkpointing.

Historical Commitment Objects do not replace the objects they commit to.

They establish relationships between evidence and history.

```text
Evidence Record
      │
      ▼
Chain Entry
      │
      ▼
Commitment Receipt
      │
      ▼
Witness Observation / Checkpoint
```

The exact ordering and optionality depend upon TAIP and the applicable Trust Profile.

---

# 6.13 Chain Entry

A **Chain Entry** is an element of the TrustAgentAI append-only Hash Chain.

A Chain Entry cryptographically binds committed evidence to defined preceding Chain state.

Its governed semantics may include:

- the Chain Identifier;
- the committed object or commitment reference;
- preceding Chain state;
- ordering information;
- commitment time with defined semantics;
- Chain Entry type and version;
- cryptographic linkage;
- applicable operator identity.

A Chain Entry is distinct from:

- the Evidence Record it commits;
- the Chain Identifier;
- the Chain Head;
- a storage row;
- a transport receipt;
- a Checkpoint.

A valid Chain Entry supports Object-to-History binding and continuity claims.

It does not by itself establish completeness, independent observation, or truth of the committed assertions.

---

# 6.14 Commitment Receipt

A **Commitment Receipt** is a Protocol Object or cryptographically protected artifact providing evidence that a defined Commitment occurred.

A Commitment Receipt may identify or bind to:

- the committed object;
- the relevant Chain Entry;
- the Chain Identifier;
- the resulting Chain Head or commitment state;
- the Commitment Service or Registry;
- the applicable commitment time;
- cryptographic proof material;
- applicable versions.

A submission acknowledgment is not automatically a Commitment Receipt.

```text
Submission Receipt
≠
Acceptance Receipt
≠
Commitment Receipt
```

TAIP must define the evidence required for a receipt to establish Commitment interoperably.

---

# 6.15 Independent Observation Objects

Independent Observation Objects represent what an eligible Witness claims to have observed.

They reduce exclusive reliance upon an Evidence Producer, Registry, or Chain operator.

An observation object must state its scope precisely.

It must not be interpreted as endorsing claims outside that scope.

For example, observing a Chain Head does not automatically mean the Witness:

- inspected every Evidence Record;
- evaluated Authority or Policy;
- verified the business outcome;
- guaranteed completeness;
- accepted legal responsibility for the action.

The Witness Observation is the principal object in this family.

---

# 6.16 Witness Observation

A **Witness Observation** is a signed Protocol Object representing a Witness's observation of defined protocol state.

Its semantics may include:

- Witness Protocol Identity;
- Key Identifier and Key Purpose;
- Observation Scope;
- observed object or state reference;
- observed digest or Chain state;
- Observation Time;
- applicable protocol and object versions;
- relevant eligibility or profile context;
- cryptographic protection.

Observation Scope determines exactly what the Witness claims to have observed.

A Witness Observation may concern:

- an Evidence Record commitment;
- a Chain Entry;
- a Chain Head;
- a Commitment Receipt;
- a Checkpoint candidate;
- another governed historical state.

Witness eligibility, independence, timing, and quorum are evaluated separately from object-level Signature validity.

---

# 6.17 Checkpoint

A **Checkpoint** is a cryptographically protected commitment to defined historical protocol state.

A Checkpoint may bind:

- one or more Chain Identifiers;
- Chain Heads;
- entry ranges;
- Witness quorum evidence;
- relevant time boundaries;
- profile or version context;
- references required for Verification;
- the Checkpoint Authority identity and Signature.

A Checkpoint creates a stable historical boundary.

Its scope must be explicit.

A Checkpoint that commits to a Chain Head does not automatically make every underlying object available or semantically valid.

The Checkpoint's object validity, Authority, historical context, and covered state must be evaluated independently.

---

# 6.18 Anchor Evidence

**Anchor Evidence** supports Verification that a TrustAgentAI commitment was placed into an External Anchor.

Anchor Evidence may bind:

- the TrustAgentAI commitment;
- the external system or namespace;
- an external transaction, publication, or record identifier;
- the anchoring method;
- publication or confirmation state;
- time evidence;
- retrieval or proof material;
- applicable algorithm and version information.

The External Anchor itself may use a non-TAIP native representation.

Anchor Evidence provides the governed bridge between TrustAgentAI state and that external representation.

A locator alone is not sufficient when the external record cannot be authenticated, resolved, or related to the claimed TrustAgentAI commitment.

---

# 6.19 Identity and Key History Objects

Identity and Key History Objects preserve protocol evidence required to interpret cryptographic actions historically.

They may represent:

- identity registration;
- key binding;
- Key Purpose;
- activation;
- rotation;
- suspension;
- reactivation;
- retirement;
- revocation;
- compromise;
- recovery;
- delegated signing relationships.

These objects support Historical Key State.

They do not automatically establish business Authority.

Identity, key, Key Purpose, Authority, and Policy remain distinct concepts.

---

# 6.20 Key Transparency Record

A **Key Transparency Record** is a Protocol Object representing an accountability-relevant identity-key relationship or key lifecycle event.

It may identify or bind to:

- the Protocol Identity;
- the affected Key Identifier;
- Key Purpose;
- lifecycle action;
- prior key state;
- resulting key state;
- effective and recorded times;
- authorizing evidence;
- related transparency history;
- cryptographic protection.

Current Key State MUST NOT silently replace Historical Key State during historical Verification.

A later revocation does not necessarily mean every earlier Signature was invalid.

The relevant question is the key's governed state and purpose at the historical boundary applicable to the Signature, including any later evidence that changes interpretation according to TAIP rules.

---

# 6.21 Preservation Evidence

**Preservation Evidence** supports claims concerning durable retention and future interpretability.

It may concern:

- retained objects;
- integrity verification;
- custody;
- replication;
- archival tier;
- retention policy;
- recovery testing;
- Legal Hold;
- migration;
- deletion or erasure;
- verification dependency availability.

Preservation Evidence does not make an invalid object valid.

It supports defined Preservation claims about evidence and dependencies.

Storage telemetry is not automatically Preservation Evidence unless it conforms to governed semantics and protection requirements.

---

# 6.22 Migration Record

A **Migration Record** is a Protocol Object or signed evidence describing an accountable transition between protocol-relevant states.

A Migration Record may bind:

- source and target protocol versions;
- source and target object representations;
- source and target Chains;
- old and new cryptographic algorithms;
- old and new infrastructure operators;
- old and new Preservation Services;
- source and target Trust Profiles;
- transformation rules;
- validation results;
- authorizing identity and Policy;
- time and sequence information.

Migration must not rewrite historical evidence as though the new state always existed.

```text
Original Historical Object
          │
          ▼
Migration Record
          │
          ▼
New Representation or Protected State
```

The original evidence, migration relationship, and resulting evidence must remain distinguishable.

---

# 6.23 Dispute Pack Manifest

A **Dispute Pack Manifest** is the canonical manifest describing the contents, integrity references, dependencies, omissions, and claims associated with a Dispute Pack.

It may identify:

- the pack identifier and version;
- focal Accountability Claims;
- included Protocol Objects;
- integrity references;
- embedded and external dependencies;
- known omissions;
- redactions;
- unavailable material;
- packaging time and assembler identity;
- expected Verification Context;
- confidentiality or handling metadata.

The Manifest makes the package boundary explicit.

A file archive without a governed Manifest is not automatically a conforming Dispute Pack.

The Manifest's structural validity does not prove that the package is complete for every possible claim.

---

# 6.24 Verification Report

A **Verification Report** is a durable Protocol Object representing the result of evaluating evidence under a defined Verification Context.

It may identify or bind to:

- the evidence evaluated;
- the Verification Context;
- protocol, schema, registry, and Trust Profile versions;
- checks performed;
- individual check results;
- missing or unavailable dependencies;
- conflicts;
- warnings;
- unsupported semantics;
- Intended Trust Profile;
- Achieved Trust Profile;
- overall Verification Outcome;
- verifier identity and software information where applicable.

A Verification Report records a conclusion reached at a defined Verification Time.

Later evidence may produce a different report.

The earlier report should not be silently rewritten to match the later conclusion.

A Verification Report does not automatically constitute Legal Validity, Regulatory Compliance, accounting approval, or business truth.

---

# 6.25 Governance and Dependency Objects

Some interpretation material may itself be represented as a governed Protocol Object or historically preserved protocol resource.

Examples include:

- Trust Profile definitions;
- schema definitions;
- object type registry entries;
- algorithm registry entries;
- extension definitions;
- identifier namespace rules;
- compatibility declarations;
- conformance metadata;
- governance decisions.

Whether each item is a first-class Protocol Object is defined by TAIP and its governed registries.

The architectural requirement is that accountability-critical interpretation material be:

- versioned;
- identifiable;
- historically available;
- integrity-protected where required;
- unambiguous about status and applicability.

An implementation must not assume that the latest registry or schema applies to every historical object.

---

# 6.26 Common Object Envelope

The **Common Object Envelope** is the logical set of properties required to identify, interpret, protect, and route a Protocol Object.

It may express:

- object type and namespace;
- object version;
- applicable TAIP version;
- stable object identifier;
- producer or issuer Protocol Identity;
- creation or issuance time;
- typed payload;
- references;
- extensions;
- cryptographic protection information.

The envelope is a semantic abstraction.

TAIP may define:

- one shared envelope structure;
- family-specific envelopes preserving equivalent rules;
- detached cryptographic envelopes;
- nested or packaged objects.

The envelope must not obscure which properties are part of the canonical object, which are external transport metadata, and which are derived during processing.

```text
Transport Metadata
        │
        ▼
Protocol Object Envelope
        │
        ▼
Typed Protocol Payload
```

Transport metadata is not automatically covered by the object's identifier or Signature.

Cryptographic coverage must be defined explicitly.

---

# 6.27 Object Type and Type Namespace

Every Protocol Object must have an unambiguous governed type.

The type determines:

- semantic purpose;
- applicable schema;
- permitted producer roles;
- required properties;
- canonicalization rules;
- cryptographic requirements;
- reference behavior;
- lifecycle rules;
- validation procedures.

Object type identifiers should be governed within a namespace that prevents accidental collision.

A type name must not be silently reassigned to incompatible semantics.

Aliases and deprecated names may be supported for compatibility only when their relationship to the canonical type remains explicit.

Type discovery from an ungoverned filename, media type parameter, database table, or transport endpoint is insufficient when the object's own interpretation depends upon that type.

---

# 6.28 Versioning Dimensions

Protocol Object interpretation may depend upon several distinct versions.

These include:

- TAIP version;
- Protocol Object type version;
- schema version;
- canonicalization version;
- cryptographic algorithm or suite version;
- Trust Profile version;
- registry version;
- extension version;
- packaging version.

```text
Protocol Version
≠
Object Version
≠
Schema Version
≠
Trust Profile Version
```

Implementations must not collapse these dimensions into one unspecified "version" value where the distinction affects interpretation.

A compatible object revision may preserve semantics while adding optional information.

An incompatible revision requires explicit version change and compatibility behavior.

Historical objects must remain interpreted under their applicable historical versions.

---

# 6.29 Object Identifiers

A Protocol Object identifier provides stable reference to a governed object within an identified namespace.

Identifier rules must define:

- namespace;
- syntax;
- uniqueness expectations;
- generation responsibility;
- comparison behavior;
- case and normalization behavior where applicable;
- collision handling;
- persistence expectations;
- relationship to object type;
- relationship to cryptographic digest.

An identifier may be globally unique without requiring one universal global registry.

Uniqueness may result from a governed namespace, issuer-scoped construction, random generation, cryptographic derivation, or another TAIP-defined method.

Possession of an identifier does not prove that the identified object exists, is available, is valid, or is authorized.

---

# 6.30 Identifier and Digest Separation

A stable Protocol Object identifier is not automatically the object's cryptographic digest.

```text
Object Identifier
≠
Object Digest
≠
Storage Locator
≠
Network Locator
```

An Evidence Record Identifier is specifically distinct from the Evidence Record digest.

A Chain Identifier is distinct from a Chain Entry identifier, Chain Head, Registry identifier, storage location, or network location.

Some TAIP object types MAY use content-derived identifiers.

Others MAY use persistent identifiers combined with explicit integrity references.

The applicable type definition must state which model applies.

Implementations must not infer digest semantics from identifier appearance.

---

# 6.31 Object Digests and Integrity References

An object digest is the result of applying a defined cryptographic digest algorithm to defined cryptographic input.

The digest semantics depend upon:

- canonicalization rules;
- domain separation;
- included and excluded properties;
- object type and version;
- algorithm identifier;
- digest encoding.

A digest may support:

- Object Integrity;
- stable integrity references;
- Chain Commitment;
- deduplication under constrained rules;
- inclusion in Witness Observations or Checkpoints;
- package integrity.

A matching digest does not prove Authority, historical Commitment, Completeness, or business truth.

An implementation must not hash incidental serialization bytes and label the result a canonical object digest unless the applicable TAIP rules define those bytes as the cryptographic input.

---

# 6.32 Typed References

References connect Protocol Objects into causal, historical, identity, policy, and verification graphs.

A governed reference may identify:

- target object identifier;
- expected target type;
- target version or version range;
- integrity digest;
- reference relationship;
- namespace;
- resolution information;
- whether the target is embedded or external.

Logical property names are illustrative; TAIP defines concrete representation.

Reference relationships may include:

- caused-by;
- authorizes;
- evaluates;
- executes;
- commits;
- observes;
- checkpoints;
- anchors;
- preserves;
- corrects;
- supersedes;
- migrates;
- verifies;
- includes.

A reference must not rely solely upon a mutable network locator where historical identity or integrity matters.

---

# 6.33 Reference Strength

References may support different levels of assurance.

## Semantic Reference

Identifies a related object by governed identifier and relationship.

## Integrity-Bound Reference

Identifies the target and binds to a defined digest or equivalent integrity commitment.

## Historically Committed Reference

Is supported by evidence that the relationship or target entered protected historical state.

## Embedded Dependency

Includes the referenced object or dependency within the containing package or object structure.

```text
Named Relationship
        │
        ▼
Integrity-Bound Relationship
        │
        ▼
Historically Committed Relationship
```

These levels are not interchangeable.

The applicable object type and Trust Profile define the required reference strength.

---

# 6.34 Reference Resolution and Missing Targets

A resolver may use registries, Dispute Packs, Preservation Services, local caches, or other governed sources to locate referenced material.

Resolution must preserve the distinction between:

- object identity;
- object integrity;
- network location;
- resolver trust;
- current availability.

A reference is not valid merely because a server returns some object at the referenced location.

The returned object must satisfy applicable identity, type, version, and integrity expectations.

If a mandatory target cannot be resolved, the unresolved dependency must remain explicit.

A verifier must not silently omit a failed reference and continue as though the evidence graph were complete.

---

# 6.35 Reference Graphs and Cycles

Protocol Object references form directed graphs.

Different graph relationships serve different purposes:

- causal graph;
- Chain history;
- Authority graph;
- identity-key history;
- Preservation graph;
- Verification Dependency Graph;
- migration graph.

Not every graph must be acyclic.

However, cryptographic input construction and dependency evaluation must remain deterministic and terminate safely.

TAIP must define or prohibit cycles where they could create:

- self-referential digest ambiguity;
- non-terminating resolution;
- inconsistent canonicalization;
- circular Authority claims;
- circular proof of validity;
- denial-of-service risk.

An object must not be considered verified solely because two unverified objects refer to each other.

---

# 6.36 Time Properties

Protocol Objects may carry several time values with different meanings.

Relevant time semantics include:

- Event Time;
- Record Time;
- Signature Time;
- Submission Time;
- Acceptance Time;
- Commitment Time;
- Observation Time;
- Checkpoint Time;
- Publication Time;
- Verification Time;
- effective time;
- expiration time.

```text
Claimed Event Time
≠
Cryptographically Supported Commitment Time
≠
Independent Observation Time
```

An object type need not carry every time value.

When time affects an Accountability Claim, the applicable meaning, source, precision, clock assumptions, and supporting evidence must be explicit.

A producer-supplied timestamp is an assertion unless supported by additional trusted evidence.

---

# 6.37 Actor, Subject, Producer, and Issuer Semantics

Protocol Objects may concern several distinct identities.

These may include:

- the Actor who performed an action;
- the subject affected by an action;
- the Evidence Producer that created the object;
- the issuer that takes responsibility for a statement;
- the signer whose key protected the object;
- the Organization controlling a relevant role;
- the verifier that produced a Verification Report.

These identities may be the same in a simple deployment.

They must not be assumed to be the same where the distinction affects accountability.

```text
Actor
≠
Evidence Producer
≠
Signer
≠
Authority
```

A Protocol Identity reference does not replace evidence concerning control, delegation, Key Purpose, or Authority.

---

# 6.38 Canonicalization Boundary

Cryptographic protection requires deterministic input.

Each cryptographically protected Protocol Object type must define or reference a canonicalization boundary identifying:

- the logical properties included;
- the representation rules;
- character encoding;
- number and string normalization;
- ordering rules;
- handling of absent, null, and default values;
- extension treatment;
- referenced versus embedded content;
- domain separation;
- algorithm identification.

The canonical representation is not necessarily identical to:

- transport encoding;
- pretty-printed representation;
- database serialization;
- user-interface rendering;
- encrypted package representation.

Independent implementations must derive equivalent cryptographic input from equivalent conforming objects.

Canonicalization details belong to TAIP, schemas, and test vectors.

---

# 6.39 Cryptographic Coverage

A Protocol Object's cryptographic protection must state exactly what is covered.

Possible coverage models include:

- the complete canonical object;
- a canonical payload plus selected envelope properties;
- a detached object digest;
- multiple independently signed components;
- an aggregate commitment to several objects.

The model must not be inferred from storage layout.

Security-critical type, version, identifier, issuer, purpose, and reference semantics should be cryptographically bound where omission would permit substitution or ambiguity.

Unprotected metadata must not be treated as though it were signed.

An implementation must not add a trusted interpretation to unsigned transport metadata unless the applicable protocol explicitly establishes that trust.

---

# 6.40 Signatures

A Signature protects defined cryptographic input and attributes use of a signing capability associated with a key.

Signature-related semantics may include:

- algorithm identifier;
- Key Identifier;
- signer Protocol Identity;
- Key Purpose;
- signature scope;
- signing input domain;
- claimed Signature Time;
- required certificate, key, or transparency dependencies;
- multi-signature or threshold relationship.

A valid Signature does not automatically prove:

- the signer's real-world identity;
- exclusive key control;
- Authority for the action;
- validity of every assertion;
- historical Commitment;
- Completeness;
- business truth.

Historical Signature Verification must use applicable Historical Key State and algorithm policy.

TAIP may permit embedded or detached Signatures if their semantics and verification inputs remain unambiguous.

---

# 6.41 Multiple Signatures and Endorsements

A Protocol Object may require or permit more than one cryptographic statement.

Examples include:

- multiple organizational approvals;
- Human and Agent Signatures;
- threshold authorization;
- countersignature;
- Witness endorsement;
- migration authorization;
- archival custody acknowledgment.

Multiple Signatures must not be interpreted solely as a count.

Their meaning depends upon:

- signer roles;
- independence;
- Signature scope;
- ordering;
- Key Purpose;
- applicable Policy;
- threshold rules;
- Historical Key State.

An additional Signature may endorse the object, a prior Signature, a digest, or a transition.

The signed subject must remain explicit.

---

# 6.42 Confidentiality and Encryption

Protocol Objects may contain sensitive information and may require encryption or access control.

Encryption may protect:

- the complete object;
- selected payload elements;
- an exported package;
- transport between roles;
- stored representations;
- external attachments.

The architecture does not prescribe one universal encryption model.

The chosen model must make explicit:

- which representation is encrypted;
- which metadata remains visible;
- how integrity and identity are evaluated;
- whether signing occurs before or after encryption;
- how authorized verifiers obtain required plaintext;
- how key rotation and long-term recovery are handled;
- how encryption affects Preservation.

Encryption does not replace canonicalization, Signature verification, access governance, or evidence minimization.

Inability to decrypt mandatory evidence must remain visible during Verification.

---

# 6.43 Extensions

Extensions allow Protocol Objects to evolve without forcing every deployment to support identical optional semantics.

An extension definition should establish:

- stable extension identifier and namespace;
- version;
- applicable object types;
- value semantics;
- canonicalization behavior;
- cryptographic coverage;
- validation rules;
- criticality;
- compatibility expectations;
- registry and governance status.

Extensions must not silently redefine Core properties.

An extension namespace must not be used to evade governance for semantics required to evaluate a Core Accountability Claim.

Vendor-specific extensions may be permitted while remaining distinguishable from Core TAIP semantics.

---

# 6.44 Critical and Non-Critical Extensions

An extension may be critical or non-critical according to governed rules.

## Critical Extension

Changes meaning required for correct interpretation or Verification.

An implementation that does not understand a required critical extension must not report the object as fully valid under the affected claim.

## Non-Critical Extension

Provides optional information that may be ignored without changing defined Core conclusions.

Ignoring a non-critical extension does not permit an implementation to discard its bytes from cryptographic processing when the extension lies within the signed or digested scope.

Unknown extension behavior must be deterministic.

Silent best-effort interpretation is not acceptable for mandatory semantics.

---

# 6.45 Object Lifecycle

A Protocol Object may pass through local and protocol lifecycle states.

Possible local construction states include:

- draft;
- validated for creation;
- finalized;
- signed;
- queued for submission.

Possible protocol states include:

- submitted;
- accepted;
- committed;
- witnessed;
- checkpointed;
- anchored;
- preserved;
- verified.

Not every object type or deployment uses every state.

The state semantics must remain distinct.

A mutable local draft is not the same object state as a finalized canonical object.

A finalized object is not automatically committed.

A committed object is not automatically witnessed, preserved, or complete.

---

# 6.46 Finalization and Immutability

Finalization establishes the object content intended for identifier calculation, digesting, signing, submission, or Commitment according to the applicable type rules.

After finalization, properties within the protected object boundary must not be changed without creating a new object or new version relationship.

After Commitment, the committed object and its historical relationship MUST NOT be silently modified.

Operational metadata outside the canonical object may change, such as:

- cache status;
- local retrieval location;
- processing status;
- index metadata;
- access-control state.

Such metadata must not be confused with canonical object content or committed history.

```text
Mutable Draft
      │ finalize
      ▼
Canonical Object
      │ commit
      ▼
Immutable Historical Relationship
```

---

# 6.47 Correction, Supersession, and Revocation

TrustAgentAI does not assume that every Protocol Object is correct.

It requires errors and changes to remain accountable.

A later object may:

- correct a factual error;
- supersede an earlier instruction;
- revoke Authority;
- reverse an operational action;
- invalidate a key;
- annotate a conflict;
- provide missing evidence;
- migrate cryptographic protection.

The later object must identify its relationship to the earlier object according to governed semantics.

The earlier committed object remains part of history.

```text
Original Object
      │
      ├── corrected-by ──► Correction Object
      ├── superseded-by ─► Superseding Object
      └── revoked-by ────► Revocation Object
```

Correction, supersession, and revocation are not interchangeable.

TAIP defines their effects on Verification.

---

# 6.48 Validation Layers

Protocol Object evaluation is layered.

## Availability and Parsing

Can the object and required representation be obtained and parsed safely?

## Structural Validation

Does the object conform to the applicable type and schema structure?

## Semantic Validation

Are values, combinations, relationships, and declared meanings permitted?

## Identifier and Canonicalization Validation

Do identifiers, canonical representation, and digests satisfy applicable rules?

## Cryptographic Validation

Are Signatures and other cryptographic protections valid under applicable algorithms and keys?

## Reference and Dependency Validation

Are required referenced objects available, correctly typed, and integrity-bound as required?

## Historical Validation

Do Historical Key State, Authority, Policy, Chain state, and applicable versions support interpretation at the relevant boundary?

## Lifecycle Validation

Does available evidence support the claimed submitted, accepted, committed, witnessed, checkpointed, anchored, or preserved state?

## Profile and Completeness Validation

Are all controls and evidence required by the intended Trust Profile and Accountability Claim satisfied?

## Protocol Conclusion

Which bounded Verification Outcome follows from the preceding layers?

Successful structural validation does not imply successful later layers.

---

# 6.49 Object Validity and Evidence Completeness

An individual Protocol Object may be valid while the evidence set remains incomplete.

Examples include:

- a valid Evidence Record missing Authority evidence;
- a valid Signature missing Historical Key State;
- a valid Chain Entry outside the required Checkpoint range;
- valid Witness Observations that do not form an eligible quorum;
- a valid Checkpoint whose underlying evidence is unavailable;
- a valid Dispute Pack Manifest omitting a mandatory schema;
- a valid Verification Report based upon a narrower Verification Context.

```text
Object Validity
≠
Evidence Completeness
≠
Trust Profile Achievement
```

Verification must report these dimensions separately where conflation could overstate assurance.

---

# 6.50 Duplicate Objects and Idempotency

Distributed systems may submit, receive, or store the same Protocol Object more than once.

Implementations should support idempotent handling where applicable.

Duplicate detection must use governed identity and integrity rules rather than incidental transport identifiers.

The following cases must remain distinguishable:

- the same canonical object delivered twice;
- two objects sharing an identifier but differing in protected content;
- equivalent semantic assertions represented by different objects;
- a correction or retry with a new identifier;
- two conflicting objects from the same producer;

An identifier collision with different protected content is not an ordinary duplicate.

It is an integrity or protocol conflict requiring explicit handling.

Deduplication must not erase historically meaningful repeated events or competing assertions.

---

# 6.51 Batching and Aggregation

TAIP may permit Protocol Objects or commitments to be batched or aggregated for efficiency.

Examples include:

- multiple Evidence Records in one submission;
- aggregate Hash Chain commitments;
- batched Witness Observations;
- Checkpoints covering entry ranges;
- package-level integrity structures;
- aggregate Signatures where supported.

Batching must preserve:

- each object's identity and type;
- inclusion semantics;
- ordering where relevant;
- failure isolation;
- cryptographic coverage;
- reference integrity;
- lifecycle interpretation;
- portable Verification.

A valid batch envelope does not automatically make every included object valid.

A rejected object must not be silently reported as committed because other batch members succeeded.

---

# 6.52 Large or External Payloads

Some accountability-relevant material may be too large, sensitive, or externally governed to embed directly.

A Protocol Object may reference external payloads where the applicable type permits it.

The reference may need to bind:

- payload identity;
- media type or representation;
- size;
- digest;
- encryption information;
- access requirements;
- retention expectations;
- resolver information;
- redaction or disclosure status.

Externalization must not convert a mandatory dependency into an invisible assumption.

If the payload is unavailable, cannot be decrypted, or fails integrity validation, the limitation must affect Verification explicitly.

Network location is not a substitute for durable identity and integrity.

---

# 6.53 Storage Independence

Protocol Object meaning must not depend upon one database, filesystem, object store, ledger, or archival vendor.

Implementations may store Protocol Objects as:

- relational records;
- document records;
- immutable blobs;
- append-only logs;
- content-addressed objects;
- encrypted archives;
- distributed replicas;
- Write Once, Read Many storage.

These choices may affect operational assurance and Preservation.

They must not alter normative object meaning.

Database primary keys, row versions, bucket paths, and internal storage metadata are not automatically protocol identifiers.

An object exported from one implementation should remain interpretable by another conforming implementation without access to the original storage engine.

---

# 6.54 Serialization and Transport Independence

Protocol Object semantics are distinct from transport and presentation.

TAIP may define one or more conforming encodings and transport bindings.

The architecture does not assume that:

- JavaScript Object Notation (JSON) is the only encoding;
- transport bytes are automatically canonical bytes;
- one application programming interface style is mandatory;
- an object must be online to be valid;
- an object URL is its identity;
- a successful network response proves protocol acceptance.

```text
Protocol Meaning
      │
      ├── JSON representation
      ├── CBOR representation
      ├── archived representation
      └── future conforming representation
```

Multiple representations may be supported only when their mapping to canonical protocol meaning and cryptographic input is deterministic.

Transport procedures are defined later and in TAIP.

---

# 6.55 Privacy, Redaction, and Derived Representations

Protocol Objects should contain or reference only information required for applicable Accountability Claims and Trust Profiles.

A disclosed representation may:

- redact selected values;
- omit optional dependencies;
- reveal only a permitted subset;
- replace sensitive payloads with integrity commitments;
- encrypt content for selected verifiers.

The disclosed representation must not be misrepresented as the complete canonical object.

The relationship between canonical evidence and a derived representation should be explicit and integrity-protected where applicable.

Redaction must not silently remove mandatory meaning while preserving a successful outcome.

A Verification Engine must report how unavailable or undisclosed material affects Completeness and Trust Profile achievement.

Privacy mechanisms should preserve accountability boundaries without requiring unnecessary disclosure.

---

# 6.56 Unknown, Unsupported, and Malformed Objects

Implementations must handle unrecognized or invalid input safely.

Relevant conditions include:

- unknown object type;
- unsupported object version;
- unknown critical extension;
- malformed structure;
- ambiguous canonicalization;
- invalid identifier;
- unsupported algorithm;
- invalid Signature;
- conflicting duplicate;
- unresolved mandatory reference;
- excessive nesting or reference depth;
- prohibited field or semantic combination.

These conditions must not be silently converted into a Valid result.

An implementation may preserve an unsupported object for future analysis without claiming to understand or verify it.

Error reporting should distinguish unsupported semantics from cryptographic invalidity, missing evidence, and ordinary processing failure.

---

# 6.57 Relationship to Chapter 7 and Later Chapters

This chapter defines the common architecture for Protocol Objects.

Chapter 7 defines the Evidence Record in detail.

Later chapters define:

- evidence lifecycle and state transitions;
- Trust and Security Models;
- cryptographic algorithms and inputs;
- reference architecture and deployment roles;
- Evidence Registry behavior;
- Witness Observation and quorum behavior;
- append-only Hash Chains;
- Checkpoints;
- Key Transparency;
- Preservation and storage;
- Verification Engines and Reports;
- Dispute Packs;
- Trust Profiles and conformance;
- application programming interface and software development kit boundaries;
- TAIP normative mapping;
- governance, versioning, compatibility, and global indexes.

The detailed specifications may refine object-specific requirements.

They must preserve the cross-object invariants defined here.

---

# Protocol Object Invariants

### INV-OBJ-001 — Governed Meaning

Every Protocol Object MUST possess an unambiguous governed type and version sufficient for interpretation.

### INV-OBJ-002 — Deterministic Representation

Finalized Protocol Objects participating in cryptographic operations MUST possess deterministic cryptographic representation.

### INV-OBJ-003 — Identifier Semantics

Object identifiers MUST be interpreted according to their governed namespace and MUST NOT be silently treated as digests or locators.

### INV-OBJ-004 — Identifier/Digest Separation

A stable object identifier MUST remain distinguishable from an object digest unless the applicable type explicitly defines a content-derived identifier.

### INV-OBJ-005 — Type Stability

A published object type identifier MUST NOT be silently reassigned to incompatible semantics.

### INV-OBJ-006 — Version Explicitness

Protocol, object, schema, profile, registry, and extension versions MUST remain distinguishable where they affect interpretation.

### INV-OBJ-007 — Cryptographic Scope

Cryptographically protected and unprotected object properties MUST remain distinguishable.

### INV-OBJ-008 — Signature/Authorization Separation

Signature validity MUST NOT automatically be interpreted as Authority or Authorization.

### INV-OBJ-009 — Identity/Key Separation

Protocol Identity, Key Identifier, Key Purpose, Authority, and Policy MUST remain semantically distinct.

### INV-OBJ-010 — Object/History Separation

Object Integrity MUST remain distinguishable from Historical Integrity and Commitment.

### INV-OBJ-011 — Lifecycle Separation

Finalization, Signature, Submission, Acceptance, Commitment, Witnessing, Checkpointing, Anchoring, Preservation, and Verification MUST NOT be conflated.

### INV-OBJ-012 — Committed Immutability

Committed Protocol Object content and its committed historical relationship MUST NOT be silently rewritten.

### INV-OBJ-013 — Append-Only Correction

Correction, supersession, revocation, and migration of committed state MUST create additional accountable state.

### INV-OBJ-014 — Typed References

Accountability-critical object relationships MUST possess governed semantics and MUST NOT depend solely upon mutable locators.

### INV-OBJ-015 — Explicit Dependencies

Mandatory object dependencies MUST remain explicit, resolvable, embedded, or reported as unavailable.

### INV-OBJ-016 — Extension Safety

Unknown mandatory or critical semantics MUST NOT be silently ignored during Verification.

### INV-OBJ-017 — Validity/Completeness Separation

Individual object validity MUST remain distinguishable from evidence Completeness and Trust Profile achievement.

### INV-OBJ-018 — Historical Interpretation

Historical Protocol Objects MUST be interpreted using applicable historical keys, versions, schemas, registries, Policies, and Trust Profiles.

### INV-OBJ-019 — Representation Independence

Normative Protocol Object meaning MUST NOT depend upon one storage system, transport, vendor, or presentation format.

### INV-OBJ-020 — Explicit Derivation

Redacted, selectively disclosed, migrated, or otherwise derived representations MUST remain distinguishable from complete original canonical evidence.

### INV-OBJ-021 — Explicit Uncertainty

Malformed, unsupported, conflicting, unavailable, or incomplete mandatory object state MUST remain visible.

### INV-OBJ-022 — Bounded Conclusion

Protocol Object Verification MUST NOT be represented as business truth, Legal Validity, Regulatory Compliance, or accounting judgment beyond the evaluated claims.

---

# Architectural Requirements

### REQ-OBJ-001

TAIP MUST define or reference a stable identifier and namespace for every interoperable Protocol Object type.

### REQ-OBJ-002

Each Protocol Object type MUST define its semantic purpose, applicable versions, required properties, permitted producers, and validation rules.

### REQ-OBJ-003

Each cryptographically protected Protocol Object type MUST define or reference deterministic cryptographic input construction.

### REQ-OBJ-004

Object type, version, and other substitution-sensitive semantics MUST be cryptographically bound where omission could change interpretation.

### REQ-OBJ-005

Identifier generation and comparison rules MUST define normalization, uniqueness expectations, and collision handling.

### REQ-OBJ-006

Implementations MUST NOT infer canonical digest semantics from identifier syntax unless the applicable object type defines that relationship.

### REQ-OBJ-007

Accountability-critical references MUST identify their relationship and MUST satisfy the integrity strength required by the applicable object type or Trust Profile.

### REQ-OBJ-008

Mandatory referenced objects MUST be validated for expected identity, type, version, and integrity.

### REQ-OBJ-009

Unresolved mandatory references MUST be reported and MUST affect Verification according to applicable TAIP rules.

### REQ-OBJ-010

Time values affecting Accountability Claims MUST identify their semantics and supporting trust assumptions.

### REQ-OBJ-011

Historical Signature evaluation MUST resolve applicable Historical Key State, Key Purpose, and algorithm policy.

### REQ-OBJ-012

Implementations MUST distinguish the Actor, subject, Evidence Producer, issuer, signer, and Authority where those roles differ.

### REQ-OBJ-013

Unknown critical extensions MUST produce an explicit unsupported or indeterminate outcome rather than silent success.

### REQ-OBJ-014

Non-critical extensions MAY be ignored semantically only when doing so does not change defined Core conclusions.

### REQ-OBJ-015

Finalized protected object content MUST NOT be mutated in place after identifier, digest, Signature, or Commitment generation.

### REQ-OBJ-016

Correction, supersession, revocation, and migration relationships MUST be represented through additional governed evidence.

### REQ-OBJ-017

Validation results SHOULD distinguish structural, semantic, cryptographic, historical, lifecycle, dependency, Completeness, and profile outcomes.

### REQ-OBJ-018

An object MUST NOT be reported as complete merely because it passes structural and cryptographic validation.

### REQ-OBJ-019

Conflicting objects that reuse an identifier with different protected content MUST be surfaced as a protocol or integrity conflict.

### REQ-OBJ-020

Batching and aggregation MUST preserve object identity, inclusion, failure, ordering, and cryptographic semantics required for independent Verification.

### REQ-OBJ-021

External payload references MUST preserve identity and integrity expectations required by the applicable Accountability Claim.

### REQ-OBJ-022

Portable Protocol Objects SHOULD remain interpretable without access to the originating database or proprietary application code.

### REQ-OBJ-023

Redacted or selectively disclosed representations MUST identify or preserve their relationship to canonical evidence and MUST NOT be represented as complete originals.

### REQ-OBJ-024

Preservation planning SHOULD include Protocol Object schemas, registries, algorithms, profiles, extensions, and other dependencies required for future interpretation.

### REQ-OBJ-025

Verification Engines MUST expose malformed, unsupported, conflicting, unavailable, and incomplete mandatory object state.

### REQ-OBJ-026

Independent implementations SHOULD derive equivalent object-level conclusions from equivalent Protocol Objects under equivalent Verification Contexts.

---

# Security Considerations

Protocol Objects form a primary security boundary because they determine which assertions are represented and which data is cryptographically protected.

Major risks include:

- type confusion between object families;
- identifier collision or substitution;
- treating a locator as stable identity;
- hashing non-canonical serialization;
- excluding security-critical properties from Signature coverage;
- replaying a valid object in an unintended context;
- substituting current key or Policy state for Historical State;
- interpreting a valid Signature as Authorization;
- accepting a submission receipt as Commitment evidence;
- presenting a valid object without required dependencies;
- circular references that create false support or denial of service;
- unknown critical extensions being ignored;
- modifying finalized content after signing;
- rewriting a committed object during correction or migration;
- duplicate handling erasing conflicting assertions;
- batch validation hiding failure of individual objects;
- external payloads disappearing or changing;
- redacted evidence being presented as complete;
- encryption making required evidence permanently unverifiable;
- unbounded parsing, nesting, reference depth, or object size;
- trusting transport metadata not covered by cryptographic protection;
- allowing vendor-specific behavior to redefine Core semantics.

Implementations should apply defensive controls including:

- strict type and version dispatch;
- schema and semantic validation;
- bounded parsing and resolution;
- deterministic canonicalization;
- domain-separated cryptographic inputs;
- explicit algorithm and key-purpose evaluation;
- Historical Key State resolution;
- reference integrity validation;
- cycle and depth controls;
- explicit extension criticality;
- immutable committed history;
- conflict-preserving duplicate handling;
- privacy-aware packaging;
- reproducible Verification.

Protocol Object integrity does not guarantee source-data truth.

The object model makes provenance, scope, cryptographic protection, relationships, history, and limitations explicit so that bounded claims can be evaluated independently.

Later security and cryptographic chapters define detailed attacker models and controls.

---

# Design Rationale

TrustAgentAI uses explicit Protocol Objects because ordinary logs and application records do not provide sufficiently stable interoperability semantics.

An operational log is usually designed for one application's diagnostics.

A Protocol Object is designed to survive transfer across applications, Organizations, storage systems, verification tools, and time.

The object taxonomy separates different accountability properties:

- Evidence Records represent events and assertions;
- Chain Entries and Commitment Receipts bind evidence to history;
- Witness Observations represent independent observation;
- Checkpoints establish historical boundaries;
- Anchor Evidence bridges external trust domains;
- Key Transparency Records preserve Historical Key State;
- Preservation Evidence supports durable availability and interpretability;
- Migration Records preserve accountable evolution;
- Dispute Pack Manifests define portable evidence boundaries;
- Verification Reports preserve bounded evaluation results.

The Common Object Envelope reduces repeated ambiguity while permitting object-specific semantics.

Identifier and digest separation permits persistent reference without forcing every object type into one content-addressing model.

Typed, integrity-aware references allow Protocol Objects to form causal and verification graphs without relying upon mutable URLs.

Versioning and critical-extension rules permit evolution without silent reinterpretation.

Layered validation prevents a structurally valid or correctly signed object from being mistaken for complete, historically committed, authorized, or true evidence.

Append-only correction preserves both the original statement and the later accountable response.

Storage and transport independence prevent one product implementation from becoming the protocol definition.

The stable objective is:

> **Independent implementations must be able to identify what an object means, determine what was protected, resolve what it depends upon, and report what the evidence does and does not establish.**

---

# Summary

Protocol Objects are the governed units of meaning used throughout TrustAgentAI and TAIP.

They represent:

- accountable events;
- Intent, Authorization, Policy evaluation, and execution evidence;
- historical Commitment;
- independent observation;
- Checkpoints and External Anchors;
- identity and key history;
- Preservation and migration;
- portable evidence packages;
- Verification results;
- governance and interpretation dependencies.

All Protocol Objects follow a common architectural discipline:

1. type and version are explicit;
2. identifiers have governed namespaces and semantics;
3. identifiers, digests, and locators remain distinguishable;
4. cryptographic inputs are deterministic;
5. Signature coverage is explicit;
6. Signature validity remains separate from Authorization;
7. references are typed and use the required integrity strength;
8. time meanings remain explicit;
9. extensions have deterministic compatibility behavior;
10. lifecycle states remain distinct;
11. finalized and committed state is not silently mutated;
12. correction and migration create additional accountable history;
13. object validity remains separate from evidence Completeness;
14. redacted and derived representations remain explicit;
15. storage, serialization, transport, and vendor choices do not redefine protocol meaning;
16. unsupported or missing mandatory semantics remain visible;
17. Verification conclusions remain bounded to evaluated claims.

Chapter 7 applies these rules to the primary Accountability Evidence object: the Evidence Record.

The foundational object rule is:

> **Make every assertion typed, every dependency explicit, every protected boundary deterministic, and every limitation visible.**

The broader architectural principle remains:

> **Proof, not logs.**
