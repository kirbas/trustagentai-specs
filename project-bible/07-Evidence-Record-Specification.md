# Chapter 7 — Evidence Record Specification

> **An Evidence Record is a bounded, attributable, and cryptographically governable statement about an accountability-relevant event.**

## Purpose

This chapter defines the architectural specification for the TrustAgentAI **Evidence Record**.

The Evidence Record is the primary Protocol Object used to represent an Accountable Action or another accountability-relevant event in a form that can be:

- identified;
- attributed;
- interpreted;
- cryptographically protected;
- linked to Authority, Policy, causality, and outcome evidence;
- submitted for historical Commitment;
- preserved;
- packaged;
- independently verified.

This chapter establishes:

- the Evidence Record's semantic boundary;
- the logical information model;
- Evidence Record Identifier semantics;
- producer, issuer, Actor, subject, and counterparty distinctions;
- event, Authority, Policy, and outcome representation;
- causal and dependency references;
- time semantics;
- finalization, cryptographic protection, and correction rules;
- privacy and disclosure behavior;
- validation layers;
- Evidence Record invariants;
- architectural requirements for interoperable implementations.

This chapter applies the common Protocol Object rules defined in [06-Protocol-Objects.md](06-Protocol-Objects.md).

It builds upon the system model in [05-System-Overview.md](05-System-Overview.md), the principles in [04-Design-Principles.md](04-Design-Principles.md), and the problem boundaries in [03-Problem-Statement.md](03-Problem-Statement.md).

It uses the canonical terms defined in [Terminology.md](Terminology.md) and [Acronyms.md](Acronyms.md).

This chapter defines architectural semantics.

It does not define final field names, concrete schemas, media types, wire encodings, canonicalization algorithms, cryptographic suites, transport endpoints, database layouts, or jurisdiction-specific legal conclusions.

Those details belong to later chapters, governed registries, schemas, test vectors, and the TrustAgentAI Interoperability Protocol (TAIP).

---

# 7.1 Evidence Record Definition

An **Evidence Record** is the primary Protocol Object representing an Accountable Action or another accountability-relevant event.

It is a structured statement produced at a defined point in an evidence lifecycle by an identifiable Evidence Producer or issuer.

An Evidence Record describes what the producer claims occurred, was requested, was evaluated, was authorized, was attempted, was rejected, was completed, or otherwise became relevant to accountability.

An Evidence Record may contain information directly or bind to separately governed evidence through typed references.

```text
Accountability-Relevant Event
              │
              ▼
       Evidence Producer
              │
              ▼
        Evidence Record
              │
              ├── identity and role context
              ├── Authority and Policy context
              ├── causal and dependency references
              ├── inputs, outputs, and outcome claims
              ├── explicit time semantics
              └── cryptographic protection
```

An Evidence Record is evidence of a bounded assertion.

It is not automatic proof that every assertion inside the record is true, complete, authorized, lawful, or independently corroborated.

---

# 7.2 Scope

This chapter applies to Evidence Records that represent or support:

- intent;
- instruction;
- approval;
- Authority delegation or use;
- Authorization decisions;
- Policy evaluation;
- Human intervention;
- Agent decisions;
- tool or service invocation;
- execution requests;
- execution acceptance or rejection;
- settlement or business outcome;
- correction, reversal, or supersession;
- accountability-relevant administrative change;
- another event required by an applicable Trust Profile.

An implementation does not need to convert every operational event into an Evidence Record.

The relevant business rules, Policy, Trust Profile, and Accountability Claims determine which events require evidence.

This chapter does not require one Evidence Record for an entire workflow.

A workflow may produce multiple Evidence Records so that distinct assertions, Actors, times, issuers, and outcomes remain independently attributable.

---

# 7.3 Evidence Record and Operational Logs

An operational log entry and an Evidence Record serve different purposes.

| Operational log entry | Evidence Record |
|---|---|
| Usually optimized for diagnostics | Optimized for durable accountability |
| May depend upon one application | Designed for independent interpretation |
| Often mutable or retention-limited | Governed by finalization and historical rules |
| May use local identifiers | Uses governed identifier semantics |
| May omit version and Authority context | Preserves interpretation dependencies |
| Often trusts current application state | Supports historical Verification |
| Commonly tied to one storage format | Storage and transport independent |

An implementation may derive an Evidence Record from operational state.

The derivation must be accountable and must not be represented as contemporaneous evidence if it was reconstructed later without disclosure.

```text
Operational Telemetry
        ≠
Evidence Record
        ≠
Historically Committed Evidence Record
```

---

# 7.4 Bounded Assertion Model

Every Evidence Record has an assertion boundary.

The boundary identifies what the producer takes responsibility for stating and what remains outside the record's claim.

For example, an Evidence Record may assert that:

- a payment instruction was received;
- a Policy engine returned an approval result;
- an Agent invoked a tool with defined parameters;
- a financial institution accepted a request;
- a settlement system reported completion;
- a Human approved an exception;
- a prior record was corrected.

The same Evidence Record does not necessarily establish that:

- the instruction reflected the principal's true intent;
- the applicable Policy was lawful or correct;
- the signer possessed the required Authority;
- the external system executed the action accurately;
- all relevant evidence has been disclosed;
- no competing or contradictory record exists.

Verification must evaluate the claim actually made rather than a broader claim inferred from convenient wording.

---

# 7.5 Evidence Record Classes

TAIP may define Evidence Record classes or governed event types for semantically distinct accountability events.

Representative classes include:

- Intent Evidence;
- Instruction Evidence;
- Authority Evidence;
- Authorization Evidence;
- Policy Evaluation Evidence;
- Approval Evidence;
- Agent Decision Evidence;
- Tool Invocation Evidence;
- Execution Request Evidence;
- Execution Result Evidence;
- Settlement Evidence;
- Correction Evidence;
- Revocation Evidence;
- Administrative Change Evidence.

These classes may be represented as:

- distinct Evidence Record types;
- governed subtypes;
- a common Evidence Record type with a governed event classification;
- typed payloads within a common envelope.

The chosen representation must preserve semantic distinctions required for Validation and Verification.

An implementation must not use one vague event type where the distinction between request, approval, execution, and outcome affects an Accountability Claim.

---

# 7.6 One Record, One Primary Event

An Evidence Record should describe one primary accountability-relevant event or one atomic statement boundary.

Related details may be included when they form part of that event.

Unrelated events should not be combined merely for storage convenience.

The one-primary-event discipline improves:

- attribution;
- time interpretation;
- causal linkage;
- correction behavior;
- failure isolation;
- privacy minimization;
- partial disclosure;
- independent Verification.

TAIP may permit composite records for genuinely atomic multi-part actions.

The composite boundary and the relationship between its parts must be explicit.

---

# 7.7 Logical Information Model

The Evidence Record logical model contains governed properties applicable to the record's class and version.

| Logical property group | Purpose |
|---|---|
| Type and version | Determines semantics and validation rules |
| Evidence Record Identifier | Provides stable governed reference |
| Producer and issuer | Identifies who created or stands behind the statement |
| Event classification | Identifies what kind of event is asserted |
| Action and subject | Describes the accountable action and affected scope |
| Actor and role context | Identifies relevant participants and capacities |
| Authority context | Links the event to claimed Authority or Authorization |
| Policy context | Links the event to applicable Policy and evaluation evidence |
| Causality and correlation | Relates the event to predecessor and workflow state |
| Inputs and outputs | Binds relevant parameters, results, or external payloads |
| Outcome and status | States the asserted event result with governed semantics |
| Time properties | Distinguishes event, record, signature, and other times |
| Trust context | Identifies intended assurance requirements where applicable |
| References and dependencies | Connects required evidence and interpretation material |
| Extensions | Adds governed optional or critical semantics |
| Cryptographic protection | Defines integrity and attribution evidence |

The model is logical rather than a final wire schema.

Not every Evidence Record requires every property group.

The applicable class, version, schema, and Trust Profile determine which properties are mandatory, optional, prohibited, derived, or externally resolved.

---

# 7.8 Common Object Envelope

An Evidence Record is a Protocol Object and therefore participates in the Common Object Envelope defined by Chapter 6.

The Evidence Record envelope must make it possible to determine:

- that the object is an Evidence Record;
- the applicable Evidence Record version;
- the applicable TAIP version;
- the Evidence Record Identifier;
- the producer or issuer identity;
- the typed Evidence Record body;
- applicable references and extensions;
- the cryptographic protection model.

Transport metadata is not automatically part of the Evidence Record.

Database columns, message headers, request paths, queue metadata, and storage locators must not silently supply security-critical Evidence Record meaning unless TAIP explicitly binds them into the governed object.

---

# 7.9 Evidence Record Identifier

The **Evidence Record Identifier (ERID)** is a stable identifier for an Evidence Record.

ERID rules must define:

- identifier namespace;
- generation responsibility;
- syntax;
- uniqueness expectations;
- comparison and normalization behavior;
- persistence expectations;
- collision handling;
- relationship to the producer or issuer;
- relationship to the Evidence Record digest.

```text
ERID
≠
Evidence Record Digest
≠
Database Primary Key
≠
Network Locator
```

TAIP may define an issuer-scoped, random, structured, or content-derived ERID model.

If a content-derived model is used, the applicable version must define the derivation precisely.

An implementation must not infer digest semantics merely because an ERID resembles a hash.

---

# 7.10 Identifier Assignment and Collision

An ERID must be assigned no later than the point at which the finalized Evidence Record is signed, submitted, referenced, or committed.

The same ERID must not identify different finalized protected content.

Two conditions must remain distinguishable:

## Duplicate

The same ERID and equivalent protected content are received more than once.

## Collision or Conflict

The same ERID is associated with different protected content or incompatible semantic identity.

A duplicate may support idempotent processing.

A collision or conflict is an integrity or protocol event and must not be silently resolved by overwriting one version.

---

# 7.11 Record Type, Event Type, and Version

The Evidence Record object type identifies the object family.

The event type or class identifies the accountability-relevant event represented by the record.

These are separate dimensions.

```text
Protocol Object Type: Evidence Record
                │
                └── Event Type: Policy Evaluation
```

The applicable versions may include:

- TAIP version;
- Evidence Record version;
- event schema version;
- canonicalization version;
- extension versions;
- Trust Profile version.

Versions affecting interpretation must be explicit and cryptographically bound where substitution could change meaning.

A version identifier must not be silently reused for incompatible semantics.

---

# 7.12 Evidence Producer and Issuer

The **Evidence Producer** creates the Evidence Record.

The **issuer** is the Protocol Identity that takes responsibility for the record's statement according to applicable semantics.

They may be the same.

They may differ when, for example:

- an application constructs a record on behalf of an Organization;
- an Agent runtime emits evidence attributed to an Agent identity;
- a gateway normalizes evidence from an external system;
- an archival process reconstructs a derived representation;
- a service signs statements produced by another controlled component.

The record must not imply that the producer, issuer, signer, Actor, or Authority are identical unless the evidence supports that relationship.

Producer and issuer references must use governed Protocol Identity semantics.

---

# 7.13 Actor, Subject, Principal, and Counterparty

An Evidence Record may concern several participants.

Relevant roles include:

- **Actor** — the human, Agent, service, or system claimed to have acted;
- **Principal** — the person or Organization on whose behalf the action was performed;
- **Subject** — the person, account, asset, instruction, Policy, or other entity affected by the event;
- **Counterparty** — another party participating in the action or outcome;
- **Approver** — an Actor that approved or rejected the action;
- **Executor** — the system or party that attempted or completed execution;
- **Beneficiary** — the party receiving a benefit where the domain defines one.

One Protocol Identity may occupy multiple roles.

Distinct roles must remain explicit where their separation affects Authority, responsibility, conflict of interest, or Verification.

```text
Principal
≠
Agent
≠
Approver
≠
Executor
≠
Evidence Producer
```

---

# 7.14 Accountable Action Description

The Evidence Record must describe the Accountable Action with sufficient precision for the applicable Accountability Claim.

The description may identify or bind to:

- action type;
- action target;
- amount, quantity, asset, or other governed parameters;
- requested constraints;
- permitted or prohibited conditions;
- external transaction or operation identifiers;
- relevant jurisdiction or business domain where required;
- prior state and intended state transition;
- result or failure semantics.

Free-form prose may supplement structured meaning.

It must not replace structured properties required for interoperable evaluation.

A display string must not be treated as the canonical semantic representation unless the applicable schema explicitly defines it as such.

---

# 7.15 Event Status and Outcome

Status and outcome values must have governed semantics.

Representative distinctions include:

- proposed;
- requested;
- approved;
- rejected;
- attempted;
- accepted;
- pending;
- completed;
- settled;
- failed;
- cancelled;
- reversed;
- disputed;
- unknown.

These values are not universally interchangeable.

```text
Requested
≠
Accepted
≠
Completed
≠
Settled
≠
Irreversible
```

The event type definition must identify which statuses are permitted and what each status asserts.

An outcome reported by one system is still a statement by that system unless independently corroborated.

---

# 7.16 Authority Context

An Evidence Record may identify or reference the Authority under which an Actor or Agent purportedly acted.

Authority context may include:

- Authority source;
- delegating Protocol Identity;
- authorized subject;
- permitted action scope;
- limits and conditions;
- effective interval;
- applicable Key Purpose;
- approval requirements;
- revocation or suspension state;
- supporting Authority or Authorization Evidence.

The record must distinguish:

- a claim that Authority existed;
- a reference to evidence of Authority;
- a verified conclusion that applicable Authority requirements were satisfied.

A valid Signature does not establish Authority by itself.

Possession of an API credential, session token, or signing key does not automatically establish business Authorization.

---

# 7.17 Policy Context

An Evidence Record may identify or bind to the Policy applicable to the event.

Policy context may include:

- Policy identifier;
- Policy version;
- Policy issuer or governing Organization;
- effective interval;
- evaluation inputs or integrity references;
- evaluator identity;
- evaluation result;
- exception or override evidence;
- related Policy Evaluation Evidence.

Historical Verification must use the Policy version applicable to the event and must not silently substitute the current Policy.

Policy presence does not prove correct application.

Policy evaluation success does not prove legal or regulatory validity.

An override must remain visible and attributable.

---

# 7.18 Causal References

Evidence Records form a causal graph.

A causal reference states how one accountability-relevant event relates to another.

Representative relationships include:

- caused-by;
- responds-to;
- authorized-by;
- approved-by;
- evaluates;
- executes;
- retries;
- reverses;
- corrects;
- supersedes;
- disputes;
- derived-from.

The relationship type must be governed.

A reference to another record does not make that record valid.

A verifier must evaluate the referenced record, the relationship, and the required reference strength separately.

---

# 7.19 Workflow and Correlation Context

An Evidence Record may carry or reference workflow context used to associate related events.

Examples include:

- workflow identifier;
- conversation or session identifier;
- business case identifier;
- transaction identifier;
- idempotency identifier;
- correlation identifier;
- parent event identifier;
- step or attempt identifier.

These identifiers serve different purposes and must not be silently collapsed.

A correlation identifier does not establish causality.

A workflow identifier does not establish ordering.

An idempotency identifier does not replace an ERID.

The security and privacy consequences of linkable identifiers must be considered.

---

# 7.20 Inputs

An Evidence Record may contain or reference inputs necessary to interpret the event.

Inputs may include:

- instructions;
- structured parameters;
- source documents;
- Policy inputs;
- model or tool context;
- account or asset state;
- Human approvals;
- prior Evidence Records;
- external data or attestations.

The record should bind only to inputs required for applicable Accountability Claims.

Input representation may use:

- embedded structured values;
- integrity-bound references;
- encrypted attachments;
- commitments to undisclosed values;
- governed external references.

The record must not imply that referenced inputs were complete, accurate, or trustworthy merely because their integrity can be verified.

---

# 7.21 Outputs and Results

An Evidence Record may contain or reference outputs produced by the event.

Outputs may include:

- a decision;
- an approval or rejection;
- a tool result;
- an execution response;
- a transaction identifier;
- a resulting state;
- an error;
- a settlement report;
- a generated document;
- a follow-up action.

The record must distinguish:

- the output claimed by the producer;
- an output returned by an external system;
- an independently observed result;
- a later business or settlement outcome.

Large or sensitive results may be represented through integrity-bound external payload references.

Unavailable mandatory result material must affect Verification explicitly.

---

# 7.22 External Payload References

An Evidence Record may reference material that is not embedded in the record.

An external payload reference may bind:

- payload identifier;
- expected media type or representation;
- size;
- digest and algorithm;
- encryption metadata;
- access or disclosure requirements;
- preservation expectation;
- resolver or locator information;
- redaction status.

The locator is not the payload's identity.

The retrieved content must satisfy applicable type, size, and integrity expectations.

If the payload is required but unavailable, undecryptable, malformed, or inconsistent, the Evidence Record must not be treated as complete for the affected claim.

---

# 7.23 Time Semantics

An Evidence Record may carry multiple time values.

Relevant meanings include:

- Event Time;
- Record Time;
- creation time;
- finalization time;
- Signature Time;
- Submission Time;
- Acceptance Time;
- Commitment Time;
- effective time;
- expiration time;
- external-system time.

The Evidence Record itself normally carries only times asserted or known at record creation.

Later lifecycle times may be established by separate Protocol Objects such as a Commitment Receipt, Chain Entry, Witness Observation, or Checkpoint.

```text
Producer-Claimed Event Time
        ≠
Record Creation Time
        ≠
Cryptographically Supported Commitment Time
        ≠
Independent Observation Time
```

Time values must identify their semantics, source, precision, and uncertainty where relevant.

A producer clock value is an assertion unless supported by additional trusted evidence.

---

# 7.24 Event Time and Record Time

**Event Time** is the time at which the represented event is claimed to have occurred.

**Record Time** is the time at which the Evidence Record is claimed to have been created or finalized according to the applicable definition.

They may differ because:

- evidence was created after receiving an external result;
- a delayed system reported an earlier event;
- a record was reconstructed;
- a batch process created records later;
- clocks differed;
- network or queue delays occurred.

The difference is not automatically invalid.

It must remain visible when relevant.

Backdated evidence must not be represented as contemporaneously committed merely because it carries an earlier Event Time.

---

# 7.25 Intended Trust Profile

An Evidence Record may identify the Intended Trust Profile applicable to its creation or processing.

The Intended Trust Profile expresses requested or expected assurance requirements.

It does not prove that those requirements were achieved.

```text
Intended Trust Profile
≠
Achieved Trust Profile
```

The Achieved Trust Profile is determined through Verification using the available evidence and dependencies.

A producer must not self-assert successful achievement merely by placing a profile identifier in the record.

Where the Intended Trust Profile is accountability-critical, its identifier and version should be cryptographically bound.

---

# 7.26 Extensions

Evidence Record extensions follow the extension rules defined for Protocol Objects.

An extension must define:

- stable identifier and namespace;
- version;
- applicable Evidence Record classes;
- value semantics;
- validation rules;
- canonicalization treatment;
- cryptographic coverage;
- criticality;
- compatibility expectations.

An extension must not silently redefine Core Evidence Record semantics.

An unknown critical extension must produce an explicit unsupported or indeterminate result for affected claims.

An unknown non-critical extension may be semantically ignored only where Core interpretation remains unchanged.

Cryptographically covered extension bytes must still be processed according to applicable canonicalization rules.

---

# 7.27 Data Minimization

Evidence Records should include or reference only information required for applicable Accountability Claims, Policies, and Trust Profiles.

Evidence quality does not increase automatically with data volume.

Unnecessary data may create:

- privacy exposure;
- security risk;
- regulatory obligations;
- excessive retention cost;
- cross-border transfer complexity;
- broader breach consequences;
- verification dependencies that cannot be preserved safely.

Data minimization must not be used to omit mandatory accountability meaning.

The architecture seeks sufficient evidence, not maximum collection.

---

# 7.28 Confidentiality Classification

An Evidence Record or referenced payload may require confidentiality handling.

Applicable metadata may identify:

- classification;
- disclosure constraints;
- recipient scope;
- encryption requirements;
- retention category;
- redaction policy;
- legal or contractual handling requirements.

Confidentiality metadata is not automatically trusted unless protected by the applicable object or packaging model.

Access control does not replace cryptographic integrity.

Encryption does not replace Authority evaluation.

Classification must not silently change the semantic content of the Evidence Record.

---

# 7.29 Canonical Representation

A finalized Evidence Record participating in digest, Signature, or Commitment operations must have a deterministic canonical representation.

The applicable TAIP definition must establish:

- included logical properties;
- excluded transport and storage metadata;
- representation and encoding rules;
- ordering and normalization;
- treatment of absent, null, and default values;
- number and time representation;
- embedded and referenced payload treatment;
- extension treatment;
- domain separation;
- algorithm identifiers.

Equivalent conforming records must produce equivalent cryptographic input.

Pretty-printed JSON, database serialization, network bytes, and user-interface rendering are not automatically canonical representations.

---

# 7.30 Evidence Record Digest

The Evidence Record digest is a cryptographic digest over the defined canonical input under a defined algorithm and domain.

It may support:

- Object Integrity;
- integrity-bound references;
- Signature input;
- Chain Commitment;
- inclusion proofs;
- Dispute Pack integrity;
- deduplication under constrained rules.

The digest must bind substitution-sensitive semantics, including type and version, where omission would permit reinterpretation.

A matching digest does not establish:

- producer Authority;
- correctness of the event claim;
- historical Commitment;
- Completeness;
- independent observation;
- Legal Validity.

---

# 7.31 Evidence Record Signature

An Evidence Record may be protected by one or more Signatures according to its class, Trust Profile, and TAIP rules.

Signature semantics may bind:

- canonical Evidence Record input or its domain-separated digest;
- signer Protocol Identity;
- Key Identifier;
- Key Purpose;
- algorithm;
- signature scope;
- applicable version context;
- claimed Signature Time.

The signer may be the Evidence Producer, issuer, Actor, approver, Organization, or another authorized role.

The role must be explicit.

A valid Signature proves only that the applicable verification operation succeeded under the evaluated key and input.

Authority, identity control, Key Purpose, historical key state, and business meaning require separate evaluation.

---

# 7.32 Multiple Signatures and Approvals

An Evidence Record may carry or reference multiple cryptographic statements.

Examples include:

- Human approval plus Agent execution attribution;
- maker-checker approval;
- organizational countersignature;
- threshold authorization;
- external-system acknowledgment;
- migration authorization.

Multiple Signatures must not be interpreted as a simple count.

Verification must evaluate:

- signer roles;
- signature scopes;
- ordering;
- independence;
- Key Purpose;
- applicable Authority and Policy;
- Historical Key State;
- threshold rules.

Witness Observations are separate Protocol Objects and must not be collapsed into ordinary Evidence Record co-signatures.

---

# 7.33 Finalization

Finalization establishes the Evidence Record content intended for identifier assignment, canonicalization, digesting, signing, submission, or Commitment.

Before finalization, a local draft may be mutable.

After finalization, content inside the protected boundary must not be modified in place.

```text
Mutable Draft
      │ validate and finalize
      ▼
Finalized Evidence Record
      │ sign and submit
      ▼
Historically Committed Evidence
```

If protected content must change after finalization, the implementation must create a new record or an explicitly governed replacement relationship.

Operational metadata outside the canonical boundary may change without changing the Evidence Record, provided it is not represented as protected record content.

---

# 7.34 Submission, Acceptance, and Commitment

Evidence Record lifecycle states must remain distinct.

## Submission

The record was sent to a Registry, Commitment Service, or another receiver.

## Acceptance

The receiver accepted the record for defined processing according to applicable rules.

## Commitment

The record or its governed commitment entered protected historical state.

```text
Submitted
≠
Accepted
≠
Committed
≠
Witnessed
≠
Checkpointed
```

A transport success response is not automatically Commitment evidence.

A Commitment Receipt or equivalent TAIP-defined artifact is required to support an interoperable Commitment claim.

---

# 7.35 Chain Binding

An Evidence Record is distinct from the Chain Entry that commits it to protected ordered history.

The binding must make it possible to determine:

- which Evidence Record or digest was committed;
- which Chain received the Commitment;
- which Chain Entry represents the Commitment;
- the relevant predecessor state;
- the resulting historical state;
- the Commitment semantics and time evidence.

```text
Evidence Record
      │ canonical digest
      ▼
Chain Entry
      │ protected linkage
      ▼
Chain Head
```

Object Integrity does not imply Historical Integrity.

A valid Evidence Record that was never committed must not be reported as historically committed.

---

# 7.36 Correction and Annotation

An Evidence Record may be incomplete, inaccurate, disputed, or later superseded.

Committed content must not be rewritten.

A correction or annotation must create additional accountable evidence that identifies:

- the affected Evidence Record;
- the relationship type;
- the corrected or disputed proposition;
- the new statement;
- the correcting producer or issuer;
- Authority and Policy context where required;
- relevant time values;
- supporting evidence.

```text
Original Evidence Record
          │
          ├── corrected-by ──► Correction Evidence Record
          ├── disputed-by ───► Dispute Evidence Record
          └── annotated-by ──► Annotation Evidence Record
```

The original record remains part of history.

A correction does not retroactively make the original assertion disappear.

---

# 7.37 Supersession, Revocation, and Reversal

Supersession, revocation, and reversal are distinct relationships.

## Supersession

A later record replaces an earlier instruction, Policy-relevant statement, or current operational meaning from a defined point forward.

## Revocation

A later record withdraws Authority, validity, or permission according to governed rules.

## Reversal

A later operational action attempts to undo or counteract the effects of an earlier action.

None of these relationships erases historical occurrence.

TAIP must define how each relationship affects current and historical Verification.

A reversal of a payment does not mean the original payment was never requested or executed.

---

# 7.38 Retry and Idempotency

Distributed systems retry operations.

Evidence Records must distinguish:

- retransmission of the same finalized record;
- a new attempt of the same business action;
- a duplicate external result;
- a replay by an attacker;
- an intentionally repeated action.

An idempotency identifier may help correlate processing attempts.

It does not replace the ERID or causal relationship semantics.

If a retry represents a new accountability-relevant attempt, it should receive a distinct Evidence Record with an explicit retry relationship.

Deduplication must not suppress a conflicting record or erase repeated events that matter to accountability.

---

# 7.39 Concurrency and Ordering

Evidence Records may be created concurrently.

ERID ordering, Record Time, or arrival order must not be assumed to establish causality unless TAIP explicitly defines such semantics.

Ordering evidence may derive from:

- causal references;
- Chain order;
- external transaction order;
- sequence values under a governed namespace;
- Witness Observations;
- Checkpoints;
- domain-specific state transitions.

Conflicting concurrent records must remain visible.

An implementation must not choose one silently merely because it arrived last.

---

# 7.40 Batching

Multiple Evidence Records may be submitted or committed in a batch.

Batching must preserve:

- individual ERIDs;
- individual canonical content;
- event types;
- Signatures and signer roles;
- acceptance or rejection status;
- inclusion semantics;
- ordering where relevant;
- failure isolation;
- Commitment evidence;
- independent Verification.

A valid batch wrapper does not make every member valid.

A batch-level success must not conceal rejection, omission, or failure of an individual Evidence Record.

---

# 7.41 Partial and Multi-Stage Outcomes

Some actions have multiple stages or partial outcomes.

Examples include:

- partial payment execution;
- multi-leg settlement;
- approval followed by later rejection;
- tool execution with partial result;
- asynchronous acceptance and completion;
- compensation after failure.

The evidence model should represent each accountability-relevant transition explicitly.

One record must not compress materially different stages into a final success label that hides partial failure.

Where one composite event is permitted, the status of each required component must remain available for Verification.

---

# 7.42 Unknown, Missing, and Conflicting Values

Evidence Records must represent uncertainty honestly.

The following conditions are distinct:

- value is known and present;
- value is intentionally omitted under permitted minimization;
- value is redacted from a disclosed representation;
- value is unknown to the producer;
- value was not applicable;
- value is unavailable because a dependency cannot be resolved;
- value conflicts with another statement;
- value is malformed or unsupported.

Null, empty, absent, unknown, and redacted must not be treated as interchangeable where the distinction affects meaning.

A verifier must not invent a favorable value to complete an incomplete record.

---

# 7.43 Structural Validation

Structural Validation determines whether the Evidence Record conforms to the applicable object and event schema.

Checks may include:

- required properties;
- permitted types;
- cardinality;
- value syntax;
- version syntax;
- identifier syntax;
- extension placement;
- size and nesting bounds;
- prohibited combinations.

Structural validity does not establish semantic, cryptographic, historical, or business validity.

A structurally malformed record must not proceed as though it were a valid Evidence Record.

An unsupported version must be reported as unsupported rather than malformed when the distinction is known.

---

# 7.44 Semantic Validation

Semantic Validation determines whether the record's values and relationships are permitted under the applicable Evidence Record class and version.

Checks may include:

- event type and status compatibility;
- required Actor and subject roles;
- Authority and Policy reference rules;
- time relationship constraints;
- causal relationship compatibility;
- input and output requirements;
- correction relationship semantics;
- Trust Profile requirements;
- extension criticality.

Semantic Validation must use the applicable historical schema and registry state.

Current rules must not silently reinterpret an older record.

---

# 7.45 Identifier and Cryptographic Validation

Identifier and cryptographic validation may evaluate:

- ERID construction where derivable;
- canonical representation;
- Evidence Record digest;
- Signature validity;
- signer key binding;
- Key Purpose;
- Historical Key State;
- algorithm eligibility;
- signature scope;
- protected version and type semantics.

A cryptographically valid record may still fail Authority, Policy, dependency, lifecycle, or Completeness evaluation.

A record signed with a currently valid key may have been invalid under the relevant historical state.

A later key revocation must be evaluated according to governed historical rules rather than applied without context.

---

# 7.46 Reference and Dependency Validation

Reference Validation determines whether required targets:

- can be resolved;
- have the expected identity;
- have the expected type and version;
- satisfy required integrity binding;
- support the stated relationship;
- were historically available or committed as required;
- remain interpretable under preserved dependencies.

Evidence Record dependencies may include:

- Authority Evidence;
- Policy definitions and evaluation evidence;
- prior Evidence Records;
- external payloads;
- identity and key history;
- schemas and registries;
- Chain and Commitment evidence;
- Witness Observations;
- Checkpoints;
- Trust Profiles.

Unresolved mandatory dependencies must remain explicit and affect Verification.

---

# 7.47 Historical and Lifecycle Validation

Historical Validation evaluates the record in relation to protected history.

Lifecycle checks may determine whether the record was:

- finalized;
- signed;
- submitted;
- accepted;
- committed;
- witnessed;
- checkpointed;
- anchored;
- preserved;
- corrected or superseded.

These outcomes must remain separate.

Historical evaluation may also require:

- applicable Chain state;
- Commitment Time;
- Historical Key State;
- historical Policy and schema versions;
- correction and revocation history;
- relevant Checkpoints and external anchors.

The latest operational state alone is insufficient for historical interpretation.

---

# 7.48 Evidence Record Verification Result

Evidence Record Verification should produce layered conclusions rather than one undifferentiated boolean.

Representative result dimensions include:

- available or unavailable;
- parseable or malformed;
- supported or unsupported;
- structurally valid or invalid;
- semantically valid or invalid;
- identifier-consistent or conflicting;
- cryptographically valid or invalid;
- signer identity resolved or unresolved;
- Authority supported, unsupported, or indeterminate;
- historically committed or uncommitted;
- dependencies complete or incomplete;
- consistent or conflicting with related evidence;
- Trust Profile achieved, partially achieved, or not achieved.

The exact Verification Outcome vocabulary belongs to the Verification specification and TAIP.

The Evidence Record specification requires that important limitations remain visible.

---

# 7.49 Portability

A conforming Evidence Record should remain interpretable outside the originating application's database and runtime.

Portable interpretation may require preservation of:

- the canonical record representation;
- applicable schema;
- event type registry entry;
- extension definitions;
- canonicalization rules;
- cryptographic algorithm definitions;
- identity and key history;
- Trust Profile;
- referenced evidence;
- Commitment evidence.

Portability does not require every dependency to be embedded in every record.

It requires dependencies to be identifiable, integrity-governed, and obtainable or reportable as unavailable.

---

# 7.50 Preservation

Preserving Evidence Record bytes is necessary but may be insufficient.

Future Verification may also require:

- schemas;
- registries;
- identifier rules;
- canonicalization procedures;
- algorithm definitions;
- historical keys;
- Authority and Policy evidence;
- Chain history;
- correction history;
- decryption capability;
- external payloads.

```text
Stored Bytes
≠
Preserved Evidence
≠
Future Interpretability
```

Preservation planning must consider the Verification Dependency Graph rather than the Evidence Record alone.

---

# 7.51 Redaction and Selective Disclosure

A disclosed Evidence Record representation may redact or withhold permitted information.

The representation must identify, directly or through governed packaging semantics:

- that it is derived or selectively disclosed;
- which portions are unavailable or redacted;
- the relationship to canonical evidence;
- integrity commitments sufficient to detect substitution where applicable;
- how the limitation affects Verification.

Redaction must not be represented as deletion from historical canonical evidence.

A redacted representation must not be labeled complete if mandatory evidence is withheld.

TAIP may define commitment or selective-disclosure mechanisms that permit verification of permitted claims without exposing unnecessary data.

---

# 7.52 Encryption

Evidence Record content may be encrypted at the field, payload, object, transport, storage, or package level.

The encryption model must preserve clarity concerning:

- which representation was signed or digested;
- which metadata remains visible;
- how the canonical plaintext or protected representation is identified;
- how authorized verifiers obtain required material;
- how decryption-key lifecycle is managed;
- how future recovery and migration are supported;
- how inability to decrypt affects Verification.

Encryption must not create an unverifiable black box that is nevertheless reported as complete evidence.

Confidentiality and independent Verification must be designed together.

---

# 7.53 Retention and Deletion

Retention requirements may differ by event type, Trust Profile, Policy, jurisdiction, contract, or Legal Hold.

Deletion of an Evidence Record or mandatory dependency can change what future verifiers are able to establish.

Where deletion is permitted or required, the system should distinguish:

- deletion of operational copies;
- deletion of canonical evidence;
- deletion of encryption keys;
- deletion of external payloads;
- retention of integrity commitments;
- retention under Legal Hold;
- accountable evidence that deletion occurred.

Privacy obligations do not justify silent rewriting of protected history.

The architecture must surface the effect of deletion on Completeness and Achieved Trust Profile.

---

# 7.54 Dispute Pack Inclusion

An Evidence Record included in a Dispute Pack must remain independently identifiable and integrity-verifiable.

The Dispute Pack Manifest should identify:

- the ERID;
- applicable digest;
- Evidence Record class and version;
- inclusion location or reference;
- embedded dependencies;
- external dependencies;
- redactions;
- known omissions;
- relationship to focal Accountability Claims.

Inclusion in a Dispute Pack does not make the Evidence Record valid or complete.

The package makes the available evidence boundary explicit for independent evaluation.

---

# 7.55 Implementation Mapping

Implementations may map the Evidence Record model to different internal structures.

Examples include:

- relational tables;
- document stores;
- immutable object storage;
- event streams;
- content-addressed stores;
- encrypted archives.

The mapping must preserve normative meaning.

Internal implementation fields may be added outside the canonical Evidence Record boundary.

They must not alter the protected record silently.

An exported Evidence Record must not require proprietary application code merely to determine its type, version, identifier, producer, event meaning, protected scope, and dependencies.

---

# 7.56 Illustrative Logical Form

The following form is illustrative only.

It is not a TAIP wire schema and does not prescribe final field names.

```text
EvidenceRecord {
    object_type
    object_version
    protocol_version
    evidence_record_identifier

    producer_or_issuer
    event_type
    event_status
    accountable_action

    actors_and_roles[]
    subjects[]
    authority_context[]
    policy_context[]

    causal_references[]
    workflow_context
    inputs_or_references[]
    outputs_or_references[]

    event_time
    record_time
    intended_trust_profile

    extensions[]
    cryptographic_protection[]
}
```

Concrete schemas may split, rename, constrain, derive, prohibit, or externalize logical properties according to the event class and protocol version.

---

# 7.57 Illustrative Payment Sequence

The following sequence shows why one broad record is insufficient for many financial workflows.

```text
Intent Evidence Record
        │
        ▼
Authorization Evidence Record
        │
        ▼
Policy Evaluation Evidence Record
        │
        ▼
Execution Request Evidence Record
        │
        ▼
Execution Acceptance Evidence Record
        │
        ▼
Settlement Evidence Record
```

Each record may have a different:

- producer;
- issuer;
- Actor;
- event time;
- Signature;
- Authority context;
- external dependency;
- status;
- Commitment state.

The causal graph allows a verifier to evaluate the complete claim without pretending that one system observed every stage.

---

# 7.58 Illustrative Correction Sequence

Assume an Execution Result Evidence Record incorrectly states that a transfer settled.

A later correction should not replace the original bytes.

```text
ER-1: Execution Result = Settled
        │
        ├── committed in Chain state C1
        │
        ▼
ER-2: Correction of ER-1
      Result = Rejected
      Reason = External reconciliation
        │
        └── committed in later Chain state C2
```

Historical Verification may conclude:

- ER-1 existed and was committed at C1;
- ER-2 later corrected ER-1 at C2;
- the current supported outcome is rejection;
- the original incorrect assertion remains part of accountable history.

---

# 7.59 Common Anti-Patterns

The following patterns violate or weaken the Evidence Record model.

## Log-Wrapping

Adding a hash to an unversioned log line and calling it an Evidence Record.

## Signer Equals Authority

Treating a valid Signature as proof of business Authorization.

## One Record for the Entire Workflow

Collapsing intent, approval, execution, and settlement into one producer assertion.

## Mutable Final Record

Updating protected fields in place after signing or Commitment.

## Timestamp Inflation

Treating producer Event Time as independently proven historical time.

## Locator Identity

Using a mutable URL or database row key as the sole durable identity.

## Latest-State Verification

Using current keys, Policies, schemas, or Authority state for every historical record.

## Silent Dependency Omission

Returning success after required Authority, Policy, payload, or Chain evidence cannot be resolved.

## Redaction as Completeness

Presenting a partial disclosed representation as the complete canonical record.

## Over-Collection

Embedding sensitive operational context that is unnecessary for the Accountability Claim.

## Retry Erasure

Deduplicating distinct attempts that matter to causality or outcome.

## Current Truth Rewrite

Replacing an earlier incorrect committed record with a later corrected value.

---

# 7.60 Relationship to Other Specifications

This chapter defines the Evidence Record itself.

Other chapters and TAIP define related concerns, including:

- evidence lifecycle state transitions;
- the Trust and Security Models;
- cryptographic input construction and algorithms;
- Evidence Registry acceptance behavior;
- Hash Chain and Chain Entry semantics;
- Commitment Receipts;
- Witness Observations and independence;
- Checkpoints and External Anchors;
- Key Transparency and Historical Key State;
- Preservation Evidence;
- Verification Engines and Reports;
- Dispute Pack construction;
- Trust Profiles and conformance;
- APIs, SDKs, schemas, registries, and test vectors.

Those specifications may refine Evidence Record class-specific requirements.

They must preserve the Evidence Record invariants defined here.

---

# Evidence Record Invariants

### INV-ER-001 — Bounded Assertion

Every Evidence Record MUST represent a bounded accountability assertion whose scope can be identified independently of the originating application.

### INV-ER-002 — Governed Type

Every Evidence Record MUST identify its governed object type, event type or class, and applicable versions.

### INV-ER-003 — Stable Identifier

Every finalized interoperable Evidence Record MUST possess a stable ERID under a governed namespace.

### INV-ER-004 — Identifier/Digest Separation

The ERID MUST remain distinguishable from the Evidence Record digest unless the applicable version explicitly defines a content-derived identifier.

### INV-ER-005 — Identifier Conflict Visibility

Different protected content associated with the same ERID MUST be surfaced as a conflict and MUST NOT be silently overwritten.

### INV-ER-006 — Producer Attribution

The Evidence Producer or issuer responsible for the record's statement MUST be identifiable according to governed Protocol Identity semantics.

### INV-ER-007 — Role Separation

Producer, issuer, signer, Actor, Principal, approver, executor, subject, and Authority MUST remain distinguishable where their roles differ.

### INV-ER-008 — Signature/Authority Separation

Signature validity MUST NOT automatically be interpreted as Authority or Authorization.

### INV-ER-009 — Explicit Event Semantics

Request, approval, rejection, execution, completion, settlement, reversal, and correction semantics MUST NOT be silently conflated.

### INV-ER-010 — Typed Causality

Accountability-critical causal relationships between Evidence Records MUST use governed relationship semantics.

### INV-ER-011 — Deterministic Protected Input

Every finalized Evidence Record participating in cryptographic operations MUST possess deterministic cryptographic input under the applicable version.

### INV-ER-012 — Protected Scope Visibility

Protected Evidence Record content MUST remain distinguishable from transport, storage, presentation, and other unprotected metadata.

### INV-ER-013 — Time Separation

Event Time, Record Time, Signature Time, Submission Time, Commitment Time, and independent Observation Time MUST NOT be treated as interchangeable.

### INV-ER-014 — Lifecycle Separation

Finalized, signed, submitted, accepted, committed, witnessed, checkpointed, preserved, and verified states MUST remain distinct.

### INV-ER-015 — Committed Immutability

Committed Evidence Record content and its committed historical relationship MUST NOT be silently rewritten.

### INV-ER-016 — Append-Only Correction

Correction, annotation, supersession, revocation, and reversal of committed Evidence Record meaning MUST create additional accountable evidence.

### INV-ER-017 — Explicit Dependencies

Mandatory Evidence Record dependencies MUST remain identifiable, integrity-governed, and either available or explicitly reported as unavailable.

### INV-ER-018 — Historical Interpretation

Evidence Records MUST be interpreted using the applicable historical keys, Policies, schemas, registries, extensions, algorithms, and Trust Profiles.

### INV-ER-019 — Validity/Completeness Separation

Evidence Record validity MUST remain distinguishable from evidence Completeness and Trust Profile achievement.

### INV-ER-020 — Intended/Achieved Separation

An Intended Trust Profile identified by an Evidence Record MUST NOT be treated as proof of the Achieved Trust Profile.

### INV-ER-021 — Explicit Uncertainty

Unknown, missing, redacted, unavailable, malformed, unsupported, and conflicting states MUST remain distinguishable where they affect Verification.

### INV-ER-022 — Privacy-Preserving Accountability

Evidence Records SHOULD minimize unnecessary sensitive data while preserving mandatory accountability meaning.

### INV-ER-023 — Derived Representation Visibility

Redacted, selectively disclosed, reconstructed, or otherwise derived representations MUST remain distinguishable from the complete canonical Evidence Record.

### INV-ER-024 — Representation Independence

Normative Evidence Record meaning MUST NOT depend upon one database, transport, serialization, vendor, or user interface.

### INV-ER-025 — Bounded Verification

Successful Evidence Record Verification MUST NOT be represented as business truth, Legal Validity, Regulatory Compliance, or proof of evidence Completeness beyond the evaluated claims.

### INV-ER-026 — Failure Visibility

Batching, retries, deduplication, and partial processing MUST NOT conceal record-level rejection, conflict, omission, or failure.

---

# Architectural Requirements

### REQ-ER-001

TAIP MUST define or reference a governed Evidence Record object type and versioning model.

### REQ-ER-002

Each interoperable Evidence Record class or event type MUST define its semantic purpose, required properties, permitted statuses, producer roles, and validation rules.

### REQ-ER-003

TAIP MUST define ERID syntax, namespace, generation responsibility, comparison behavior, uniqueness expectations, and collision handling.

### REQ-ER-004

Implementations MUST NOT infer Evidence Record digest or locator semantics from ERID appearance unless the applicable version defines that relationship.

### REQ-ER-005

Finalized Evidence Records MUST identify the Evidence Producer or issuer responsible for the bounded statement.

### REQ-ER-006

Evidence Records MUST distinguish relevant Actor, Principal, subject, approver, executor, signer, and counterparty roles where those distinctions affect an Accountability Claim.

### REQ-ER-007

Evidence Record event status values MUST use governed semantics appropriate to the event type.

### REQ-ER-008

Authority and Authorization claims MUST identify or reference the applicable source, subject, scope, limits, and historical context required by the event type or Trust Profile.

### REQ-ER-009

Policy-dependent records MUST identify or reference the applicable Policy version and evaluation evidence required for historical interpretation.

### REQ-ER-010

Accountability-critical references MUST identify their relationship and satisfy the reference strength required by the applicable event type or Trust Profile.

### REQ-ER-011

External payload references MUST preserve the identity, integrity, type, and availability expectations required for the affected Accountability Claim.

### REQ-ER-012

Time values MUST identify their semantics and MUST NOT imply stronger time assurance than their supporting evidence provides.

### REQ-ER-013

Every cryptographically protected Evidence Record version MUST define or reference deterministic canonicalization and domain-separated cryptographic input construction.

### REQ-ER-014

Substitution-sensitive type, version, identifier, producer, event, status, reference, and extension semantics MUST be cryptographically bound where omission could change interpretation.

### REQ-ER-015

Evidence Record Signature evaluation MUST resolve the applicable signer identity, Key Identifier, Key Purpose, Historical Key State, algorithm policy, and signature scope.

### REQ-ER-016

Signature validity MUST be reported separately from Authority, Authorization, Policy, Commitment, Completeness, and business outcome conclusions.

### REQ-ER-017

Implementations MUST NOT modify protected Evidence Record content in place after finalization.

### REQ-ER-018

Implementations MUST preserve the distinction between Submission, Acceptance, Commitment, Witnessing, Checkpointing, Preservation, and Verification.

### REQ-ER-019

An interoperable Commitment claim MUST be supported by TAIP-defined Commitment evidence rather than transport success alone.

### REQ-ER-020

Correction, annotation, supersession, revocation, reversal, and dispute relationships MUST identify the affected Evidence Record and use governed semantics.

### REQ-ER-021

Committed original Evidence Records MUST remain available or accounted for according to applicable Preservation and retention rules after correction or supersession.

### REQ-ER-022

Duplicate processing MUST compare protected identity and content sufficiently to distinguish retransmission from identifier conflict or a new attempt.

### REQ-ER-023

Batch processing MUST preserve individual record identity, validation, acceptance, rejection, ordering, inclusion, and Commitment outcomes.

### REQ-ER-024

Unknown critical extensions MUST produce an explicit unsupported or indeterminate outcome for affected claims.

### REQ-ER-025

Missing, redacted, unavailable, undecryptable, malformed, conflicting, or unsupported mandatory evidence MUST affect Verification explicitly.

### REQ-ER-026

Evidence Record validation SHOULD distinguish availability, parsing, structural, semantic, identifier, cryptographic, reference, historical, lifecycle, Completeness, and profile results.

### REQ-ER-027

Historical Verification MUST use applicable historical interpretation dependencies rather than silently substituting current state.

### REQ-ER-028

Portable Evidence Records SHOULD remain interpretable without access to the originating database or proprietary application code.

### REQ-ER-029

Preservation planning SHOULD include the schemas, registries, algorithms, historical keys, Policies, Trust Profiles, and referenced evidence required for future Verification.

### REQ-ER-030

Redacted or selectively disclosed representations MUST identify their derived status and MUST NOT be represented as complete canonical Evidence Records.

### REQ-ER-031

Evidence Record designs SHOULD minimize unnecessary personal, confidential, and proprietary information while retaining evidence required by applicable claims and profiles.

### REQ-ER-032

Inability to decrypt mandatory Evidence Record content MUST produce an explicit limitation or indeterminate outcome rather than silent success.

### REQ-ER-033

Implementations MUST bound Evidence Record size, nesting, collection cardinality, parsing effort, and reference-resolution depth.

### REQ-ER-034

Conflicting Evidence Records and competing assertions MUST remain visible to Verification and MUST NOT be silently resolved through last-write-wins behavior.

### REQ-ER-035

Independent conforming implementations SHOULD derive equivalent record-level conclusions from equivalent Evidence Records under equivalent Verification Contexts.

---

# Security Considerations

Evidence Records are a primary security boundary because they determine which event claims are represented, attributed, protected, and later relied upon.

Major threats include:

- forging an Evidence Producer or issuer identity;
- using a valid key for an unauthorized purpose;
- treating signer possession as business Authority;
- substituting the event type, status, subject, amount, target, Policy, or Trust Profile;
- reusing an ERID with different protected content;
- replaying a valid record in a new workflow or context;
- suppressing failed, rejected, or competing records;
- manipulating Event Time to create false contemporaneity;
- presenting Submission or Acceptance as Commitment;
- replacing historical Policy, schema, key, or registry state with current state;
- omitting mandatory Authority, Policy, causal, or result dependencies;
- changing an external payload while retaining its locator;
- exploiting unknown critical extensions;
- using ambiguous null, absent, default, redacted, or unknown values;
- rewriting finalized or committed evidence;
- abusing correction or supersession to conceal the original statement;
- deduplicating distinct retries or repeated actions;
- hiding individual failures inside a successful batch;
- forcing unbounded parsing, decompression, nesting, or reference traversal;
- collecting excessive sensitive data;
- making encrypted evidence permanently undecryptable;
- presenting a redacted or reconstructed representation as canonical and complete.

Implementations should apply controls including:

- strict type, version, and event dispatch;
- deterministic canonicalization;
- domain-separated digests and Signatures;
- cryptographic binding of substitution-sensitive semantics;
- explicit signer role and Key Purpose;
- Historical Key State evaluation;
- Authority and Policy dependency validation;
- integrity-bound typed references;
- bounded parsing and dependency resolution;
- replay and context-binding controls;
- conflict-preserving duplicate handling;
- immutable committed history;
- append-only correction;
- record-level batch outcomes;
- privacy-aware schemas and selective disclosure;
- durable preservation of interpretation dependencies;
- layered, reproducible Verification.

A cryptographically valid Evidence Record may contain a false or incomplete assertion.

TrustAgentAI reduces the ability to alter, substitute, misattribute, or silently reinterpret the statement.

It does not eliminate the need to evaluate provenance, Authority, Policy, causality, independent observation, Completeness, and real-world evidence.

---

# Privacy Considerations

Evidence Records may contain financial, behavioral, organizational, personal, or proprietary information.

Accountability systems can become surveillance systems if evidence collection is not constrained.

Privacy design should consider:

- whether the data is necessary for a defined Accountability Claim;
- whether a digest or commitment can replace direct disclosure;
- whether a sensitive payload can remain external and encrypted;
- which verifiers require which portions;
- whether correlation identifiers create excessive linkability;
- whether retention duration is justified;
- whether decryption and access can be audited;
- whether redaction preserves required integrity relationships;
- whether deletion obligations affect historical Completeness;
- whether exported Dispute Packs expose unnecessary data.

Privacy protection must not depend solely upon hiding the Evidence Record format.

Access control, encryption, minimization, selective disclosure, Preservation, and transparent Verification limitations should operate together.

---

# Design Rationale

TrustAgentAI uses Evidence Records because accountability across Organizations and systems requires a stable unit of evidence meaning.

The Evidence Record separates several concepts that ordinary logging commonly collapses:

- the event from the record describing it;
- the Actor from the Evidence Producer;
- the signer from the Authority;
- the identifier from the digest;
- Event Time from Commitment Time;
- request from execution and settlement;
- Object Integrity from Historical Integrity;
- record validity from evidence Completeness;
- Intended Trust Profile from Achieved Trust Profile;
- current interpretation state from Historical State.

The bounded assertion model prevents one record from being interpreted as proof of claims it never made.

The one-primary-event discipline supports causal graphs in which different Organizations can independently attest to intent, approval, execution, and outcome.

Typed references preserve relationships without forcing all sensitive or large evidence into one object.

Explicit time semantics prevent a producer-supplied timestamp from becoming false proof of historical order.

Finalization and append-only correction preserve the distinction between what was asserted earlier and what became known later.

Layered validation prevents a valid Signature from becoming an unjustified conclusion about Authority, truth, or Completeness.

Portability and dependency preservation allow Evidence Records to outlive the application that produced them.

Data minimization and selective disclosure constrain the privacy cost of durable accountability.

The stable objective is:

> **A verifier should be able to determine who made which bounded statement about which event, under which historical context, with which protected dependencies, and with which limitations.**

---

# Summary

The Evidence Record is the primary TrustAgentAI Protocol Object for representing an Accountable Action or another accountability-relevant event.

A conforming Evidence Record establishes a disciplined evidence boundary:

1. the record makes a bounded assertion;
2. object type, event type, and versions are explicit;
3. the ERID has governed semantics and remains distinct from the digest and locator;
4. producer, issuer, signer, Actor, Principal, subject, approver, executor, and Authority remain distinguishable;
5. action, status, input, output, Authority, Policy, causality, and time semantics are explicit where required;
6. external dependencies are typed and integrity-governed;
7. canonical cryptographic input is deterministic;
8. Signature validity remains separate from Authorization and truth;
9. finalization, Submission, Acceptance, Commitment, Witnessing, Checkpointing, Preservation, and Verification remain distinct;
10. committed content is not silently rewritten;
11. correction and supersession create additional accountable history;
12. retries, conflicts, batches, and partial outcomes preserve record-level visibility;
13. unknown, missing, unsupported, redacted, and conflicting evidence remains explicit;
14. historical interpretation uses historical dependencies;
15. record validity remains separate from Completeness and Trust Profile achievement;
16. portability does not depend upon one vendor or storage engine;
17. privacy is protected through minimization, governed disclosure, and explicit limitations;
18. Verification conclusions remain bounded to evaluated claims.

The foundational Evidence Record rule is:

> **Record the claim precisely, bind its context deterministically, preserve its history, and never hide what the evidence cannot establish.**

The broader architectural principle remains:

> **Proof, not logs.**
