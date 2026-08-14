# Chapter 5 — System Overview

> **TrustAgentAI separates the systems that act from the evidence that makes those actions independently accountable.**

## Purpose

This chapter presents the TrustAgentAI system as a composed architectural whole.

It explains:

- the boundary between the **Operational World** and the **Evidence World**;
- the logical roles that participate in each world;
- the principal Protocol Objects and accountability mechanisms;
- the end-to-end lifecycle of Accountability Evidence;
- the trust and control boundaries that affect assurance;
- the relationship between Intended Trust Profile and Achieved Trust Profile;
- the conditions required for independent historical Verification.

This chapter builds upon [01-Philosophy.md](01-Philosophy.md), [02-Executive-Summary.md](02-Executive-Summary.md), [03-Problem-Statement.md](03-Problem-Statement.md), and [04-Design-Principles.md](04-Design-Principles.md).

It uses the canonical terms defined in [Terminology.md](Terminology.md) and [Acronyms.md](Acronyms.md).

This chapter defines architectural intent and system relationships.

It does not define concrete Protocol Object fields, wire formats, application programming interfaces (APIs), canonicalization algorithms, cryptographic suites, storage schemas, or deployment-specific topology.

Those details belong to later chapters and to the TrustAgentAI Interoperability Protocol (TAIP).

---

# 5.1 System Definition

TrustAgentAI is an accountability and evidence architecture for consequential actions performed or mediated by autonomous and semi-autonomous systems.

Its initial focus is AI-driven financial activity, including actions such as:

- initiating or approving payments;
- executing treasury instructions;
- processing invoices and reimbursements;
- applying financial policy;
- coordinating procurement workflows;
- making multi-agent financial decisions;
- invoking external financial infrastructure.

TrustAgentAI does not perform these business actions itself.

It provides a protocol-oriented layer for creating, protecting, preserving, transporting, and independently verifying evidence about them.

The system can be summarized as:

```text
Consequential Action
        │
        ▼
Structured Accountability Evidence
        │
        ▼
Protected Historical State
        │
        ▼
Portable Verification Material
        │
        ▼
Independent Verification Outcome
```

The architectural objective is not to prove every real-world assertion automatically.

The objective is to make defined Accountability Claims evaluable from explicit evidence rather than from the originating system's unsupported narrative.

---

# 5.2 System Scope and Boundary

The TrustAgentAI system boundary includes the roles, objects, state transitions, and verification dependencies required to support protocol-level accountability.

The boundary includes interactions concerning:

- evidence creation;
- canonical representation;
- cryptographic protection;
- identity, key, Authority, and Policy binding;
- evidence submission and commitment;
- append-only historical continuity;
- independent observation;
- checkpoint creation;
- external anchoring;
- key transparency;
- preservation;
- dispute packaging;
- Verification;
- Trust Profile evaluation.

The boundary does not absorb every system that participates in the underlying business process.

Payment networks, enterprise resource planning systems, banks, identity providers, key-management systems, policy engines, Agent runtimes, archival systems, and external ledgers may remain external systems connected through defined integration boundaries.

```text
External Operational Systems
             │
             ▼
TrustAgentAI Integration Boundary
             │
             ▼
Evidence Lifecycle and Protected History
             │
             ▼
Independent Verification Boundary
```

A connected system does not become trustworthy merely because it participates in TrustAgentAI.

Its statements become evidence inputs whose integrity, provenance, Authority, historical context, and Completeness must be evaluated according to applicable rules.

---

# 5.3 The Dual-World Model

TrustAgentAI divides the system into two related but distinct architectural worlds.

## Operational World

The Operational World performs business activity.

It creates intentions, applies policy, makes decisions, invokes external services, and produces business outcomes.

## Evidence World

The Evidence World creates and preserves accountability state.

It represents claims, protects evidence, maintains historical continuity, obtains independent observations, preserves dependencies, and supports Verification.

```text
┌──────────────────────────────┐       ┌──────────────────────────────┐
│      Operational World       │       │        Evidence World        │
│                              │       │                              │
│  Intent and Authority        │       │  Evidence Records            │
│  Agents and Workflows        │──────►│  Protected History           │
│  Policy Evaluation           │       │  Independent Observations    │
│  Business Execution          │       │  Preservation                │
│  Execution Results           │──────►│  Verification                │
└──────────────────────────────┘       └──────────────────────────────┘
```

The two worlds interact, but they SHOULD NOT be collapsed into one trust boundary.

If the same control domain can perform an action, rewrite all evidence concerning that action, suppress evidence of failure, and determine the final Verification Outcome, the architecture remains dependent upon producer assertion.

Separation does not require every logical role to run on different physical infrastructure.

It requires that role combination, common control, and correlated failure remain explicit when assurance is evaluated.

---

# 5.4 The Operational World

The Operational World may contain:

- Human Actors;
- Organizations;
- AI Agents;
- Agent orchestrators;
- workflow engines;
- policy and approval systems;
- payment and banking interfaces;
- treasury platforms;
- enterprise resource planning systems;
- accounting systems;
- procurement systems;
- external counterparties;
- financial infrastructure.

Its primary concerns include:

- correct business execution;
- availability;
- latency;
- throughput;
- cost;
- user experience;
- automation;
- policy enforcement;
- operational recovery.

The Operational World is the source of many accountability-relevant statements, but those statements are not automatically sufficient evidence.

For example, an Agent runtime may state that a payment was approved under a given policy.

Verification may still require evidence concerning:

- which Protocol Identity represented the Agent;
- which key protected the record;
- whether the key was authorized for the required Key Purpose;
- which version of the Policy applied;
- which Authority delegated the action;
- whether the evidence entered committed history;
- whether required Witnesses observed the relevant state;
- whether the claimed execution result is supported.

Operational success and accountability success therefore remain separate outcomes.

---

# 5.5 The Evidence World

The Evidence World may contain:

- Evidence Producers;
- Evidence Gateways;
- Evidence Registries;
- Commitment Services;
- Hash Chains;
- Witnesses;
- Checkpoint Authorities;
- External Anchor services;
- Key Transparency services;
- Preservation Services;
- Dispute Pack assemblers;
- Verification Engines;
- profile and registry resolvers.

Its primary concerns include:

- deterministic representation;
- integrity;
- attribution;
- historical ordering;
- append-only continuity;
- independent observation;
- explicit assurance;
- portability;
- dependency preservation;
- long-term interpretability;
- reproducible Verification.

The Evidence World does not determine business truth merely by protecting a producer's statement.

It determines whether defined protocol claims are supported by available evidence under a defined Verification Context.

```text
Producer Statement
       │
       ▼
Integrity and Attribution Evaluation
       │
       ▼
Historical and Dependency Evaluation
       │
       ▼
Profile and Completeness Evaluation
       │
       ▼
Bounded Protocol Conclusion
```

The resulting conclusion may be Valid, Invalid, Incomplete, Unsupported, Indeterminate, or another outcome defined by TAIP.

It MUST NOT silently extend beyond the claims actually evaluated.

---

# 5.6 The Integration Boundary

The Integration Boundary connects operational activity to the evidence lifecycle.

An implementation may place this boundary:

- inside an Agent runtime;
- in an Agent orchestration layer;
- in a workflow gateway;
- beside a policy engine;
- around a payment or enterprise application programming interface;
- in an infrastructure proxy;
- in an event-processing layer;
- in a dedicated TrustAgentAI service.

The placement changes operational trust and failure modes.

An integration close to the Agent may capture rich causal context but share the Agent's control domain.

An integration close to the execution system may capture authoritative execution results but lack complete decision context.

A dedicated Evidence Gateway may provide consistency across systems but becomes an important availability and integrity boundary.

The architecture therefore evaluates what the integration can observe and protect, not merely what it is called.

At minimum, the Integration Boundary should make explicit:

- which events create Accountability Evidence;
- which Actor or system supplies each assertion;
- which identifiers and versions apply;
- when evidence creation occurs relative to execution;
- how failures are surfaced;
- how evidence is submitted for commitment;
- which sensitive data is included, referenced, encrypted, or omitted.

---

# 5.7 Logical Roles and Deployment Components

TrustAgentAI defines logical roles rather than requiring one fixed product decomposition.

A logical role expresses an accountability responsibility.

A deployment component is a concrete service, process, library, device, or Organization that performs one or more roles.

```text
Logical Role
     │
     ├── may be implemented by one component
     ├── may be distributed across components
     └── may share a component with another role
```

Role combination is allowed unless prohibited by an applicable Trust Profile.

Role combination does not preserve independence automatically.

For example:

- one service may act as Evidence Producer and submitter;
- one platform may operate an Evidence Registry and a Hash Chain;
- one Organization may operate several Witness instances;
- one archival provider may preserve both Protocol Objects and dependencies;
- one verifier may also assemble a Dispute Pack.

Where multiple roles share ownership, administration, keys, infrastructure, deployment pipelines, or failure domains, the common control MUST be considered during assurance evaluation.

The number of components is not the number of independent parties.

---

# 5.8 Human and Organizational Authority

Consequential Agent activity ultimately occurs within a human, organizational, contractual, or technical Authority structure.

The relevant authority source may be:

- an Organization;
- a designated Human Actor;
- a role or office;
- a governed policy process;
- a contractual mandate;
- a technical delegation;
- a combination of these sources.

TrustAgentAI represents evidence about Authority.

It does not create Authority merely by recording or signing a claim.

The architecture therefore keeps distinct:

- the Protocol Identity that appears in evidence;
- the key used for cryptographic protection;
- the Key Purpose for which that key is recognized;
- the Authority claimed for the action;
- the Policy limiting or directing that Authority;
- the external legal or organizational basis for the delegation.

```text
Organization or Human Authority
              │
              ▼
        Delegation / Policy
              │
              ▼
        Agent or Workflow
              │
              ▼
        Accountable Action
```

Verification can evaluate the protocol evidence supporting these relationships.

It does not replace legal or organizational judgment concerning whether the delegation was enforceable.

---

# 5.9 Agents and Orchestration

An Agent may plan, recommend, authorize, initiate, coordinate, or execute an action.

An Agent may operate alone or as part of a multi-Agent workflow.

An Agent orchestrator may:

- allocate tasks;
- route messages;
- apply workflow state;
- request human approval;
- invoke tools;
- select policies;
- combine outputs;
- trigger external execution.

The architecture does not assume that an Agent's natural-language explanation is sufficient evidence of what occurred.

Accountability Evidence should instead bind to structured facts such as:

- the accountable event;
- the participating Protocol Identity;
- relevant Authority and Policy references;
- causal predecessors;
- structured inputs and outputs required by the claim;
- execution references or results;
- applicable versions;
- time semantics;
- cryptographic protection.

Hidden model reasoning is not the protocol boundary.

Implementations MAY preserve additional model or workflow telemetry when lawful and useful, but TrustAgentAI does not require preservation of private chain-of-thought.

---

# 5.10 Policy and Authority Services

Policy and Authority services provide or evaluate context that constrains an Accountable Action.

They may determine:

- permitted action types;
- transaction limits;
- approval thresholds;
- eligible counterparties;
- separation-of-duties conditions;
- geographic or temporal restrictions;
- escalation requirements;
- required human participation;
- required Trust Profile;
- permitted Key Purposes.

An evidence record claiming compliance with a Policy should identify or bind to the historically applicable Policy representation.

A reference to "current policy" is insufficient when current state may differ from the state that governed the action.

Policy evaluation and policy validity are different claims.

A system may prove that it evaluated a particular version without proving that the policy was lawful, complete, or correctly authored.

Changes to policies, delegations, and trust-critical configuration may themselves constitute Accountable Actions.

---

# 5.11 Execution Systems and Financial Infrastructure

Execution systems perform or report the real-world effect associated with an Accountable Action.

Examples include:

- payment processors;
- banks;
- treasury systems;
- enterprise resource planning systems;
- procurement platforms;
- accounting platforms;
- settlement networks;
- external counterparties.

An Agent decision and an execution result are separate events.

```text
Decision or Instruction
          │
          ▼
Execution Request
          │
          ▼
External Acceptance
          │
          ▼
Settlement or Business Result
```

Each event may have different time semantics, Actors, identifiers, evidence sources, and failure states.

TrustAgentAI should permit evidence to connect these events without falsely representing them as one atomic occurrence.

Evidence from execution infrastructure may strengthen a claim because it originates from a different control domain.

Its independence and meaning must still be evaluated explicitly.

---

# 5.12 Evidence Producer and Evidence Gateway

An **Evidence Producer** creates or causes creation of a Protocol Object representing an accountability-relevant event.

An **Evidence Gateway** is an integration role that may receive operational assertions, normalize them into governed evidence structures, apply canonicalization, obtain signatures, and submit the result to an Evidence Registry or Commitment Service.

The same component may perform both roles.

Typical responsibilities include:

- identifying the applicable object type and version;
- capturing required historical context;
- binding relevant Authority and Policy references;
- recording causal relationships;
- constructing deterministic cryptographic inputs;
- invoking an authorized signing capability;
- reporting evidence-creation failure;
- submitting evidence for commitment;
- retaining or returning a submission receipt where applicable.

The Evidence Producer is not automatically trusted to establish all facts contained in the record.

Its role establishes provenance for the assertions it makes.

Other evidence layers establish different properties.

For example, a Signature may protect object integrity, a Hash Chain may protect historical continuity, a Witness Observation may establish independent observation, and a Preservation Service may support future availability.

---

# 5.13 Evidence Registry and Commitment Service

An **Evidence Registry** accepts, indexes, resolves, or serves Protocol Objects and related accountability state.

A **Commitment Service** establishes that defined evidence or a cryptographic commitment to that evidence entered protected historical state.

These roles may be combined.

Their responsibilities may include:

- validating submission syntax and supported versions;
- assigning or verifying identifiers;
- enforcing required commitment rules;
- creating Chain Entries;
- advancing a Hash Chain;
- returning commitment evidence;
- publishing current or historical commitments;
- exposing evidence required for Verification;
- preserving append-only correction semantics.

Acceptance by a Registry is not identical to Commitment.

Storage by a Registry is not identical to Preservation.

Availability from a Registry is not identical to independent Verification.

```text
Submitted
    │
    ▼
Accepted
    │
    ▼
Committed
    │
    ▼
Observed / Checkpointed / Anchored
    │
    ▼
Preserved
```

TAIP defines the exact semantics required for interoperable lifecycle states.

---

# 5.14 Hash Chain and Historical Commitment

A **Hash Chain** provides cryptographic linkage between ordered historical states.

It may bind:

- Evidence Records;
- Chain Entries;
- prior Chain Identifiers;
- ordering information;
- timestamps with defined meaning;
- additional governed metadata.

The Hash Chain supports detection of certain forms of insertion, deletion, replacement, or reordering.

It does not by itself prove:

- that every required event was submitted;
- that the producer's assertions were true;
- that no alternative history was presented to another observer;
- that the chain operator was independent;
- that the chain will remain available;
- that Authority or Policy was valid.

Hash Chains therefore contribute one layer of assurance.

They become stronger when combined with Witness Observations, Checkpoints, External Anchors, Key Transparency, Preservation, and portable Verification.

Committed evidence is corrected through additional accountable state.

The original committed object MUST NOT be silently replaced as though the earlier state never existed.

---

# 5.15 Witness Role

A **Witness** independently observes and attests to defined evidence state.

A Witness Observation may concern:

- a Chain Entry;
- a Hash Chain head;
- a Checkpoint candidate;
- a commitment receipt;
- another governed protocol state.

The Witness role reduces exclusive dependence upon the producer or chain operator.

Its assurance depends upon:

- Witness eligibility;
- identity and key evidence;
- observation semantics;
- independence criteria;
- timing requirements;
- quorum rules;
- availability of the Witness Observation;
- absence or visibility of conflicts.

Multiple Witnesses operated by one Organization are not automatically independent.

Multiple instances sharing administration, signing keys, infrastructure, or failure domains may provide resilience without providing independent assurance.

A Witness attests only to the observation defined by its statement.

It does not automatically endorse the business truth, legality, or correctness of the underlying action.

---

# 5.16 Checkpoint Authority

A **Checkpoint Authority** creates a compact, governed commitment to a defined historical state.

A Checkpoint may summarize:

- a Hash Chain head;
- a range of Chain Entries;
- a registry state;
- Witness quorum evidence;
- applicable time and version information;
- references required for later Verification.

Checkpoints provide stable boundaries for historical evaluation.

They can:

- reduce the amount of state required for later continuity checks;
- support comparison across observers;
- create regular historical milestones;
- provide inputs to external anchoring;
- strengthen detection of later history rewrite.

A Checkpoint does not eliminate the need for the evidence and dependencies required to interpret what it commits to.

A Checkpoint Authority may be internal or external.

Its control domain and relationship to other roles affect the assurance it contributes.

---

# 5.17 External Anchor

An **External Anchor** places a commitment to TrustAgentAI state into a system with a different administrative or trust boundary.

Possible anchor environments may include:

- an independent transparency service;
- a timestamp authority;
- a public or permissioned ledger;
- a regulated archival service;
- a counterparty-controlled record;
- another independently governed publication channel.

TrustAgentAI does not require blockchain.

The relevant architectural property is that the anchor provides useful evidence outside the control domain whose history is being protected.

An External Anchor may strengthen claims concerning existence by a boundary, consistency, or resistance to unilateral rewrite.

It does not automatically validate the underlying evidence contents.

An anchor reference that cannot be resolved or interpreted during Verification does not provide its intended assurance merely because an identifier exists.

---

# 5.18 Key Transparency

**Key Transparency** preserves accountable history concerning the keys used by Protocol Identities.

It supports questions such as:

- which key was associated with an identity at a relevant historical time;
- which Key Purpose was permitted;
- whether a key was active, suspended, revoked, expired, or superseded;
- whether a key transition was authorized;
- whether conflicting key histories exist;
- which evidence supports historical interpretation.

Key Transparency separates current configuration from Historical Key State.

```text
Protocol Identity
       │
       ├── Key A: active during interval 1
       ├── Key B: active during interval 2
       └── Key C: revoked with historical evidence
```

A current key directory is not sufficient for historical Verification when keys rotate or status changes.

Key Transparency does not replace Authority evidence.

A key may be historically associated with an identity and still lack authorization for the action or Key Purpose being evaluated.

---

# 5.19 Preservation Service

A **Preservation Service** maintains evidence and verification dependencies across the Evidence Lifetime.

Preservation may include:

- canonical Protocol Objects;
- signatures and cryptographic metadata;
- Chain Entries and Hash Chain material;
- Witness Observations;
- Checkpoints and External Anchor evidence;
- Historical Key State;
- schemas and canonicalization rules;
- Trust Profile definitions;
- registry snapshots;
- algorithm identifiers and specifications;
- extension definitions;
- migration and renewal evidence.

Storage and replication are necessary operational techniques in many deployments, but neither alone establishes Preservation.

Preservation requires future interpretability, not merely retained bytes.

```text
Retained Bytes
      +
Historical Semantics
      +
Cryptographic Dependencies
      +
Resolvable Governance Material
      =
Future Verifiability
```

Preservation MAY be distributed across multiple services.

The resulting Verification Dependency Graph must remain discoverable and sufficiently available for the required assurance period.

---

# 5.20 Dispute Pack Assembler

A **Dispute Pack** is a portable collection of evidence and dependencies assembled for review or Verification outside the originating operational environment.

A Dispute Pack assembler may gather:

- the focal Evidence Record;
- causally related records;
- relevant Chain Entries;
- Witness Observations;
- Checkpoints;
- External Anchor evidence;
- Key Transparency Records;
- Policy and Authority evidence;
- execution evidence;
- Trust Profile definitions;
- schemas and registry material;
- Preservation Evidence;
- a manifest describing included and missing material.

A Dispute Pack is not required to contain every object ever produced by a deployment.

It should contain or resolve to the evidence necessary for the claims and Verification Context in scope.

Missing, redacted, unavailable, or externally resolved material MUST remain explicit.

The assembler does not determine validity merely by including an object.

The Verification Engine evaluates the material under applicable rules.

---

# 5.21 Verification Engine

A **Verification Engine** evaluates Protocol Objects, historical evidence, dependencies, and Trust Profile requirements under a defined Verification Context.

Its responsibilities may include:

- parsing supported object types and versions;
- applying deterministic representation rules;
- verifying identifiers and Signatures;
- resolving Protocol Identity and Historical Key State;
- evaluating Key Purpose, Authority, and Policy evidence;
- validating Hash Chain continuity;
- evaluating Witness eligibility and quorum;
- validating Checkpoints and External Anchors;
- evaluating preservation and dependency availability;
- determining Completeness;
- calculating Achieved Trust Profile;
- producing machine-readable and human-readable Verification Reports.

The Verification Engine must not convert uncertainty into success.

Unsupported mandatory semantics, missing dependencies, conflicts, and incomplete evidence must produce explicit outcomes.

Equivalent evidence evaluated under equivalent Verification Contexts should produce equivalent protocol conclusions across conforming implementations.

A Verification Report is a protocol conclusion.

It is not automatically a legal opinion, accounting conclusion, regulatory determination, or guarantee of business truth.

---

# 5.22 Role Responsibility Map

The following map summarizes principal logical responsibilities.

| Role | Primary responsibility | Does not establish by itself |
|---|---|---|
| Human or Organizational Authority | Supplies the basis for delegation and accountability | Cryptographic integrity or historical commitment |
| Agent or workflow | Plans, decides, coordinates, or initiates action | Independent evidence of its own conduct |
| Policy or Authority service | Supplies or evaluates governed constraints | Legal validity of every policy claim |
| Execution system | Performs or reports business effect | Complete decision provenance |
| Evidence Producer or Gateway | Creates structured Protocol Objects | Truth of every included assertion |
| Evidence Registry or Commitment Service | Accepts, resolves, and commits evidence | Independence, Preservation, or business correctness |
| Hash Chain operator | Maintains linked historical state | Completeness or absence of equivocation by itself |
| Witness | Observes defined state | Correctness of the underlying action |
| Checkpoint Authority | Commits to a historical boundary | Availability of all committed evidence |
| External Anchor service | Places a commitment across a trust boundary | Meaning or validity of underlying content |
| Key Transparency service | Preserves historical key state | Authority for a business action |
| Preservation Service | Maintains evidence and dependencies | Validity of preserved claims |
| Dispute Pack assembler | Creates portable verification material | Successful Verification merely by packaging |
| Verification Engine | Evaluates defined protocol claims | Legal, regulatory, or business truth beyond scope |

This table expresses logical separation.

Deployments MAY combine roles where allowed, but their assurance claims must reflect the resulting control relationships.

---

# 5.23 Protocol Objects at System Level

TrustAgentAI represents accountability state through versioned Protocol Objects.

The principal object families include:

- Evidence Records;
- Chain Entries;
- Witness Observations;
- Checkpoints;
- External Anchor Evidence;
- Key Transparency Records;
- Preservation Evidence;
- Dispute Pack Manifests;
- Verification Reports;
- governed profile, registry, and dependency objects.

At system level, each object family contributes a different accountability property.

```text
Evidence Record          What is claimed about an event
Chain Entry              Where it entered ordered history
Witness Observation      Who independently observed defined state
Checkpoint               Which historical boundary was committed
External Anchor Evidence Where a commitment crossed a trust boundary
Key Transparency Record  Which key state applied historically
Preservation Evidence    How evidence and dependencies remained available
Dispute Pack Manifest    What material was assembled or omitted
Verification Report      What protocol conclusions were reached
```

No single Protocol Object should be interpreted as proving properties assigned to other layers unless the applicable specification explicitly defines that composition.

Object structure and field-level semantics are defined later and in TAIP.

---

# 5.24 End-to-End Accountability Lifecycle

The end-to-end lifecycle begins before evidence reaches a registry and continues beyond the Operational Lifetime of the originating system.

```text
Intent and Authority
        │
        ▼
Policy Evaluation
        │
        ▼
Agent Decision or Workflow Event
        │
        ▼
Accountable Action / Execution
        │
        ▼
Evidence Creation and Signature
        │
        ▼
Submission and Acceptance
        │
        ▼
Commitment and Hash Chain
        │
        ▼
Witnessing and Checkpointing
        │
        ▼
External Anchoring
        │
        ▼
Preservation and Renewal
        │
        ▼
Dispute Pack Assembly
        │
        ▼
Independent Verification
```

Not every Accountable Action requires every optional mechanism.

The applicable Trust Profile defines required controls and evidence.

The lifecycle is not always a single synchronous transaction.

Witnessing, Checkpointing, anchoring, preservation, or execution-result evidence may occur later.

The system must preserve which state was reached, when it was reached, and which required state was not reached.

---

# 5.25 Lifecycle State Separation

TrustAgentAI treats lifecycle states as distinct because each supports different claims.

## Created

The evidence object has been constructed.

## Signed

Cryptographic protection has been applied by a key.

## Submitted

The object has been sent to a receiving role.

## Accepted

The receiver has acknowledged the submission under defined acceptance rules.

## Committed

The object or its commitment has entered protected historical state.

## Witnessed

An eligible Witness has created a valid observation of defined state.

## Checkpointed

The state is covered by a valid Checkpoint.

## Anchored

A relevant commitment has been placed in an external trust domain.

## Preserved

Evidence and required dependencies satisfy defined preservation expectations.

## Verified

A Verification Engine has evaluated the available material under a defined Verification Context.

```text
Created ≠ Signed ≠ Submitted ≠ Accepted ≠ Committed
Committed ≠ Witnessed ≠ Checkpointed ≠ Anchored ≠ Preserved
Valid Object ≠ Complete Evidence ≠ Business Truth
```

Implementations MUST NOT infer later states solely from earlier states.

TAIP defines the exact evidence required to establish each interoperable state.

---

# 5.26 Causality and Multi-Agent Workflows

Accountable financial actions may emerge from multiple decisions and Actors rather than one isolated event.

Examples include:

- one Agent proposes a payment;
- another Agent checks policy;
- a Human Actor approves an exception;
- a workflow service releases the instruction;
- a bank accepts or rejects execution;
- a reconciliation service reports settlement.

TrustAgentAI represents such workflows as an evidence graph built from explicit causal relationships.

```text
Authority Evidence ──────┐
                        ▼
Policy Evaluation ──► Decision Record ──► Execution Request
                              │                  │
Human Approval ───────────────┘                  ▼
                                         Execution Result
```

Causal references do not necessarily imply blame, legal responsibility, or sole control.

They establish machine-verifiable relationships among accountability-relevant events.

The graph should permit a verifier to distinguish:

- predecessor from successor;
- input from supporting evidence;
- delegation from execution;
- recommendation from authorization;
- request from acceptance;
- execution from settlement;
- original state from correction.

Missing causal evidence must remain visible when required by the applicable Trust Profile or claim.

---

# 5.27 Trust and Control Domains

A **Control Domain** is a set of roles, systems, keys, Operators, or Organizations subject to sufficiently common control that their failures or misconduct may be correlated.

Relevant common-control factors may include:

- legal ownership;
- administrative authority;
- shared personnel;
- shared signing keys;
- shared key-management infrastructure;
- shared cloud accounts;
- shared deployment pipelines;
- shared databases;
- shared security boundaries;
- shared funding or contractual incentives;
- shared incident response authority.

TrustAgentAI does not require that every role be mutually distrustful.

It requires trust assumptions to be explicit and bounded.

```text
Control Domain A                 Control Domain B

Agent                           Independent Witness
Evidence Producer              External Anchor
Registry                       Preservation Copy
Hash Chain Operator            Independent Verifier
```

The example is illustrative rather than mandatory.

A real deployment may have more or fewer domains.

Trust Profile evaluation must use actual control relationships rather than labels or instance counts.

---

# 5.28 Independence and Correlated Failure

Independence is an assurance property, not a deployment slogan.

Two services are not independent merely because they have:

- different hostnames;
- different process identifiers;
- different regions within one administrative account;
- different keys controlled by the same Operator;
- separate replicas of one database;
- separate brand names under common control.

Independence may be strengthened by:

- separate Organizations;
- separate administrative authority;
- separate key custody;
- separate infrastructure and deployment control;
- distinct economic incentives;
- transparent eligibility rules;
- auditable conflict disclosure;
- verifiable quorum composition.

No single criterion is universally sufficient.

Applicable Trust Profiles define the criteria required for a particular assurance claim.

Replication improves availability and resilience.

Independence reduces correlated manipulation or failure.

The two properties are valuable but MUST remain distinct.

---

# 5.29 Trust Profiles as System Configurations

A **Trust Profile** defines a versioned set of assurance requirements applied across the system.

A profile may specify requirements for:

- cryptographic algorithms and key custody;
- object types and mandatory evidence;
- identity and Historical Key State;
- Authority and Policy binding;
- Hash Chain behavior;
- Witness eligibility and quorum;
- independence criteria;
- Checkpoint cadence;
- External Anchoring;
- Preservation;
- verification dependencies;
- evidence lifetime;
- acceptable degradation behavior.

Trust Profiles do not create assurance by name.

They define measurable conditions whose satisfaction must be supported by evidence.

```text
Trust Profile Requirements
           │
           ▼
Observed Controls and Evidence
           │
           ▼
Achieved Trust Profile
```

Different actions in one deployment MAY require different profiles.

For example, a low-value internal workflow may require fewer independent controls than a high-value cross-organizational transfer.

The profile identifier and version applicable to an Accountable Action must remain historically interpretable.

---

# 5.30 Intended and Achieved Trust Profile

The **Intended Trust Profile** expresses the assurance level selected or required for an action.

The **Achieved Trust Profile** expresses the assurance level actually supported by available evidence and satisfied controls.

```text
Intended Profile: TP3
        │
        ├── Signature valid
        ├── Hash Chain valid
        ├── Witness quorum incomplete
        ├── Checkpoint valid
        └── Preservation evidence unavailable
        │
        ▼
Achieved Profile: lower than TP3 or incomplete
```

The exact result depends upon the profile rules defined by TAIP.

A deployment may continue operating when an assurance service is unavailable, if its governing policy permits that behavior.

It MUST NOT represent the intended level as achieved when required evidence is missing.

Graceful operational degradation is compatible with TrustAgentAI only when the accountability downgrade remains explicit.

---

# 5.31 Verification Dependency Graph

A Verification result may depend upon more than the focal Evidence Record.

The **Verification Dependency Graph** identifies the material required to interpret and evaluate the relevant claims.

Dependencies may include:

- schemas;
- canonicalization rules;
- algorithm definitions;
- registry entries;
- Trust Profile versions;
- extension definitions;
- Protocol Identity material;
- Historical Key State;
- Authority and Policy evidence;
- Chain Entries;
- Witness Observations;
- Checkpoints;
- External Anchor evidence;
- Preservation Evidence;
- related causal records.

```text
Focal Evidence Record
       │
       ├── Schema and canonicalization
       ├── Identity and Historical Key State
       ├── Authority and Policy
       ├── Chain and Witness evidence
       ├── Trust Profile
       └── Preservation and registry material
```

Self-describing evidence does not require every dependency to be embedded.

It requires dependencies to be explicit, versioned, and resolvable or intentionally included.

Loss of a mandatory dependency may change a Verification Outcome from Valid to Incomplete, Unsupported, or Indeterminate even when retained bytes remain unchanged.

---

# 5.32 Privacy, Disclosure, and Data Minimization

Accountability Evidence may contain sensitive financial, personal, commercial, operational, or security information.

TrustAgentAI therefore favors evidence that is minimal but sufficient for the applicable Accountability Claims.

Relevant architectural controls may include:

- data minimization;
- references instead of unnecessary duplication;
- encryption at rest and in transit;
- field-level access control;
- selective evidence packaging;
- explicit redaction;
- governed retention;
- separation of canonical evidence from disclosed views;
- privacy-preserving commitments where appropriate.

Minimization does not mean omitting evidence required by the applicable Trust Profile and then reporting full assurance.

Disclosure does not require unrestricted access to every operational record.

A disclosed view, redacted object, or selective package MUST remain distinguishable from the complete canonical evidence.

The system should expose how redaction or omission affects Completeness and Verification.

---

# 5.33 Failure, Degradation, and Recovery

TrustAgentAI assumes components may become unavailable, compromised, inconsistent, or obsolete.

Relevant failures include:

- evidence creation failure;
- signing failure;
- registry rejection;
- commitment delay;
- chain inconsistency;
- Witness unavailability;
- quorum failure;
- conflicting observations;
- Checkpoint delay;
- anchor failure;
- key-resolution failure;
- Preservation failure;
- dependency loss;
- unsupported object or extension version;
- verifier disagreement.

The architecture distinguishes operational recovery from historical correction.

Operational systems may retry, reroute, or continue under policy.

Committed accountability history must not be silently rewritten during recovery.

```text
Failure Detected
      │
      ├── Operational response: retry, stop, escalate, continue
      │
      └── Evidence response: record status, preserve failure, reassess profile
```

Recovery actions that materially affect trust-critical state may themselves require Accountability Evidence.

The system should fail explicitly and degrade visibly rather than manufacture false certainty.

---

# 5.34 Deployment Patterns

TrustAgentAI permits multiple deployment patterns.

## Embedded Pattern

Evidence creation occurs inside an Agent runtime or operational application.

This pattern may provide low latency and rich context but may share the producer's trust domain.

## Gateway Pattern

A dedicated Evidence Gateway receives events from multiple operational systems.

This pattern may provide consistent policy and implementation behavior but creates a concentrated integration boundary.

## Platform Pattern

A common platform provides Registry, Commitment, Chain, Checkpoint, Preservation, and Verification capabilities.

This pattern may simplify operations, but combined control must remain visible.

## Federated Pattern

Multiple Organizations operate registries, Witnesses, anchors, Preservation Services, or verifiers under governed interoperability rules.

This pattern may strengthen independent assurance while increasing coordination complexity.

## Hybrid Pattern

Operational and evidence roles are distributed across embedded, organizational, and external services.

No pattern is universally correct.

Conformance depends upon protocol behavior and evidence.

Assurance depends upon the actual control domains and satisfied Trust Profile requirements.

---

# 5.35 Integration Modes

Implementations may integrate synchronously, asynchronously, or through a hybrid model.

## Synchronous Integration

The operational workflow waits for defined evidence lifecycle results before proceeding.

This mode can enforce strong accountability gates but may add latency or availability dependencies.

## Asynchronous Integration

The workflow emits accountability-relevant state and commitment occurs later.

This mode can reduce operational coupling but creates a period in which execution may have occurred without committed evidence.

## Hybrid Integration

The workflow requires a minimum synchronous boundary, such as evidence creation or submission, while Witnessing, Checkpointing, anchoring, and Preservation occur asynchronously.

The chosen mode must not blur lifecycle semantics.

An asynchronous submission must not be reported as committed merely because it is queued.

The applicable Policy and Trust Profile should define which lifecycle boundary is required before an Accountable Action may proceed.

---

# 5.36 Administrative and Control-Plane Actions

The Evidence World contains administrative actions capable of changing future Verification.

Examples include:

- registering or disabling a Protocol Identity;
- rotating or revoking a key;
- changing a Key Purpose;
- modifying Witness eligibility;
- changing quorum rules;
- publishing a new Trust Profile version;
- changing a schema or registry entry;
- modifying Checkpoint cadence;
- changing Preservation policy;
- authorizing a cryptographic migration;
- changing verifier trust configuration.

These actions may be as consequential as operational business actions.

Trust-critical administration SHOULD therefore create Accountability Evidence under an appropriate Authority and Policy.

Control-plane evidence must preserve historical meaning.

A later administrator must not be able to make an earlier action appear valid merely by changing current configuration without accountable history.

---

# 5.37 Interoperability Boundary

The Project Bible defines architectural intent.

TAIP defines normative interoperable behavior.

The interoperability boundary includes the semantics that independent implementations must share, including:

- Protocol Object types and versions;
- canonicalization;
- identifiers;
- cryptographic inputs and Signatures;
- lifecycle states;
- Hash Chain semantics;
- Witness Observations and quorum;
- Checkpoints and External Anchors;
- Key Transparency;
- Preservation Evidence;
- Dispute Pack behavior;
- Verification Outcomes;
- Trust Profile evaluation;
- extension and compatibility rules.

Implementations may differ in:

- programming language;
- database technology;
- service topology;
- user interface;
- internal queueing;
- cloud provider;
- operational monitoring;
- performance optimization.

Implementation differences must not change the protocol meaning represented to another conforming implementation.

The ultimate interoperability test is whether independently developed systems can exchange evidence and reproduce bounded protocol conclusions.

---

# 5.38 System Exclusions

TrustAgentAI is not:

- an AI model;
- an Agent orchestration framework;
- a hidden-reasoning archive;
- a payment network;
- a bank;
- an enterprise resource planning system;
- an accounting platform;
- a workflow engine;
- a generic log-management product;
- a security information and event management system;
- a universal identity provider;
- a universal policy language;
- a universal compliance engine;
- a compliance certification;
- a legal decision-maker;
- a guarantee that all producer assertions are true;
- a mandatory blockchain architecture.

TrustAgentAI may integrate with such systems.

It does not replace them or inherit their claims automatically.

Its scope is the accountability evidence and Verification layer connecting consequential action to durable, independently evaluable history.

---

# 5.39 Relationship to Later Chapters

This chapter establishes the system map used by the remainder of the Project Bible.

Later chapters define in greater detail:

- terminology and conceptual models;
- Protocol Identity, Authority, Policy, and keys;
- Accountable Actions and Evidence Records;
- canonicalization and identifiers;
- cryptographic protection;
- Hash Chains and commitment semantics;
- Witnesses and quorum;
- Checkpoints and External Anchors;
- Key Transparency;
- Preservation and cryptographic renewal;
- Dispute Packs;
- Verification Engines, Contexts, Outcomes, and Reports;
- Trust Profiles;
- security, privacy, and threat models;
- protocol evolution and compatibility;
- conformance, test vectors, and governance;
- global invariants and requirements.

This chapter does not pre-empt their normative detail.

It defines how their subjects compose into one accountability system.

---

# System Invariants

### INV-SYS-001 — Operational/Evidence Separation

Operational execution and accountability evidence preservation MUST remain distinguishable system responsibilities.

### INV-SYS-002 — Evidence Over Producer Assertion

No producer-controlled statement alone MUST be represented as independently verified accountability evidence.

### INV-SYS-003 — Logical Role Clarity

Accountability responsibilities MUST remain attributable to explicit logical roles even when one component performs multiple roles.

### INV-SYS-004 — Control-Domain Visibility

Common control and correlated failure among assurance roles MUST remain visible during Trust Profile evaluation.

### INV-SYS-005 — Identity/Key/Authority Separation

Protocol Identity, Key Identifier, Key Purpose, Authority, and Policy MUST remain semantically distinct.

### INV-SYS-006 — Lifecycle Separation

Creation, Signature, Submission, Acceptance, Commitment, Witnessing, Checkpointing, Anchoring, Preservation, and Verification MUST NOT be conflated.

### INV-SYS-007 — Object/History Separation

Object Integrity MUST remain distinguishable from Historical Integrity.

### INV-SYS-008 — Append-Only Correction

Correction of committed evidence MUST create additional accountable state rather than silently replace the prior state.

### INV-SYS-009 — Replication/Independence Separation

Replication and operational redundancy MUST NOT automatically be interpreted as independent assurance.

### INV-SYS-010 — Intended/Achieved Separation

Intended Trust Profile MUST remain distinguishable from Achieved Trust Profile.

### INV-SYS-011 — Explicit Uncertainty

Missing, incomplete, conflicting, unavailable, redacted, or unsupported mandatory evidence MUST remain visible.

### INV-SYS-012 — Historical Interpretation

Historical evidence MUST be evaluated using the applicable historical keys, policies, profiles, schemas, registries, and protocol semantics.

### INV-SYS-013 — Dependency Visibility

Dependencies required for Verification MUST remain explicit and resolvable or be reported as unavailable.

### INV-SYS-014 — Causal Explicitness

Causal relationships required to support an Accountability Claim MUST be represented explicitly rather than inferred solely from event proximity.

### INV-SYS-015 — Evidence Portability

Core evidence required for independent Verification SHOULD remain exportable outside the originating operational environment.

### INV-SYS-016 — Preservation/Storage Separation

Stored or replicated evidence MUST NOT be represented as preserved unless required semantics and verification dependencies remain available.

### INV-SYS-017 — Verification Reproducibility

Equivalent evidence evaluated under equivalent Verification Contexts SHOULD produce equivalent protocol conclusions.

### INV-SYS-018 — Protocol/Truth Separation

Protocol Verification MUST remain distinguishable from business truth, Legal Validity, Regulatory Compliance, and accounting judgment.

### INV-SYS-019 — Privacy Proportionality

Accountability architecture MUST NOT be interpreted as requiring unnecessary preservation or disclosure of sensitive information.

### INV-SYS-020 — Implementation Neutrality

Normative system meaning MUST NOT depend upon one vendor, product, infrastructure provider, ledger, or deployment topology.

---

# Architectural Requirements

### REQ-SYS-001

An implementation MUST identify the boundary at which operational events become Accountability Evidence.

### REQ-SYS-002

An implementation MUST preserve the source and role responsible for each accountability-critical assertion.

### REQ-SYS-003

Evidence Producers MUST create or bind to the versions and semantics required to interpret their Protocol Objects.

### REQ-SYS-004

Evidence creation failures affecting an applicable Trust Profile MUST be surfaced and MUST NOT be silently converted into success.

### REQ-SYS-005

Implementations MUST distinguish operational execution results from evidence lifecycle results.

### REQ-SYS-006

A Registry or Commitment Service MUST expose interoperable evidence of the lifecycle state it claims to establish.

### REQ-SYS-007

Hash Chain implementations MUST preserve governed ordering and continuity semantics without silently rewriting committed entries.

### REQ-SYS-008

Witness and quorum evaluation MUST apply the eligibility, independence, timing, and threshold rules of the applicable Trust Profile.

### REQ-SYS-009

Checkpoint and External Anchor claims MUST identify or bind to the historical state they commit to.

### REQ-SYS-010

Historical Signature Verification MUST use applicable Historical Key State and Key Purpose evidence.

### REQ-SYS-011

Authority and Policy evidence required by an Accountability Claim MUST identify or bind to the historically applicable state.

### REQ-SYS-012

Preservation planning SHOULD cover the Protocol Objects and Verification Dependency Graph required for the applicable Evidence Lifetime.

### REQ-SYS-013

Dispute Packs MUST identify included, omitted, redacted, unavailable, and externally resolved mandatory material.

### REQ-SYS-014

Verification Engines MUST expose failed, incomplete, conflicting, unavailable, and unsupported mandatory checks in their Verification Outcomes.

### REQ-SYS-015

Trust Profile achievement MUST be derived from satisfied requirements and supporting evidence rather than from the requested profile identifier alone.

### REQ-SYS-016

When required controls are unavailable, an implementation MUST NOT silently represent the Intended Trust Profile as achieved.

### REQ-SYS-017

Role combination and common control affecting independence MUST be represented in deployment and assurance evaluation.

### REQ-SYS-018

Accountability-critical causal relationships SHOULD be represented through stable references suitable for independent evaluation.

### REQ-SYS-019

Redacted, selectively disclosed, or derived evidence MUST remain distinguishable from complete canonical evidence.

### REQ-SYS-020

Trust-critical administrative changes SHOULD create Accountability Evidence sufficient to preserve historical interpretation.

### REQ-SYS-021

Independent implementations SHOULD be able to exchange portable evidence and reproduce protocol conclusions under equivalent Verification Contexts.

### REQ-SYS-022

Deployment optimization MUST NOT silently weaken the assurance represented by Protocol Objects, lifecycle states, or Trust Profile results.

---

# Security Considerations

The system architecture reduces exclusive dependence upon the originating operational environment, but its security depends upon correct separation, explicit trust, and complete evidence.

Major system-level risks include:

- an Agent or Evidence Producer fabricating structured but false assertions;
- an integration boundary omitting required events;
- a signing service using an unauthorized or compromised key;
- current identity or key state being substituted for Historical State;
- a Registry accepting evidence without establishing the claimed Commitment;
- a Hash Chain operator presenting inconsistent histories to different observers;
- multiple Witnesses sharing hidden common control;
- Witness quorum being claimed from ineligible or unavailable observations;
- Checkpoints committing to state whose dependencies cannot be recovered;
- an External Anchor being controlled by the same party it is intended to constrain;
- a Preservation Service retaining bytes while losing schemas, profiles, or cryptographic semantics;
- a Dispute Pack omitting adverse or conflicting evidence;
- a verifier ignoring unsupported mandatory extensions;
- Intended Trust Profile being reported as achieved despite control failure;
- sensitive data being preserved or disclosed beyond what the claim requires;
- administrative changes rewriting the interpretation of prior actions;
- operational recovery silently altering committed history;
- one vendor becoming the only practical interpreter of the evidence.

TrustAgentAI addresses these risks through layered mechanisms rather than one universal trust anchor.

Relevant controls include:

- structured and canonicalizable Protocol Objects;
- explicit identity, key, Authority, and Policy relationships;
- append-only historical commitment;
- independent Witnesses;
- governed Checkpoints;
- External Anchors across meaningful trust boundaries;
- Key Transparency;
- Preservation of the Verification Dependency Graph;
- portable Dispute Packs;
- reproducible Verification;
- explicit Trust Profile achievement and downgrade.

These mechanisms do not eliminate malicious or erroneous source data.

They make the provenance, integrity, history, dependencies, assurance limits, and Verification conclusions more explicit and independently evaluable.

Later security chapters define detailed threats, attacker capabilities, mitigations, and residual risk.

---

# Design Rationale

TrustAgentAI uses a layered system because consequential autonomous actions create several distinct accountability questions.

No single mechanism answers all of them.

- an Evidence Record represents what is claimed;
- a Signature protects defined cryptographic inputs and attributes signing capability;
- identity and Historical Key State support interpretation of the signer;
- Authority and Policy evidence support evaluation of permission and constraint;
- a Hash Chain protects ordering and historical continuity;
- Witness Observations reduce exclusive dependence upon the chain operator;
- Checkpoints establish stable historical boundaries;
- External Anchors extend commitments across trust domains;
- Preservation maintains evidence and dependencies across time;
- a Dispute Pack makes the material portable;
- a Verification Engine evaluates bounded claims reproducibly;
- a Trust Profile states which combination is required and achieved.

The Operational World and Evidence World are separated because their incentives and failure modes differ.

Operational systems optimize for execution.

Evidence systems optimize for durable accountability.

Combining them physically may be practical, but collapsing their meanings would allow operational convenience to erase assurance boundaries.

The architecture therefore remains role-based and technology-neutral.

It permits embedded, gateway, platform, federated, and hybrid deployments while requiring each deployment to make its actual trust assumptions visible.

The system is also intentionally explicit about negative results.

Missing evidence, unsupported semantics, failed quorum, unavailable dependencies, incomplete Preservation, and lower Achieved Trust Profile are meaningful outcomes.

Hiding those outcomes would recreate the accountability problem the architecture exists to solve.

The design standard is not that every action must use the strongest possible control.

The design standard is that the selected assurance be defined, evidenced, measured, and reported honestly.

---

# Summary

TrustAgentAI is composed of two interacting architectural worlds.

The **Operational World** performs consequential business activity through Humans, Organizations, Agents, policies, workflows, and execution infrastructure.

The **Evidence World** creates and preserves structured accountability state through Evidence Records, Signatures, Hash Chains, Witness Observations, Checkpoints, External Anchors, Key Transparency, Preservation, Dispute Packs, and Verification Reports.

The system follows an end-to-end lifecycle:

1. Authority and Policy constrain an intended action.
2. An Agent or workflow decides, coordinates, or initiates the action.
3. Execution infrastructure performs or reports the business effect.
4. An Evidence Producer or Gateway creates structured Accountability Evidence.
5. A Registry or Commitment Service places the evidence into protected history.
6. Witnesses, Checkpoints, and External Anchors may strengthen historical assurance.
7. Key Transparency preserves Historical Key State.
8. Preservation maintains evidence and verification dependencies.
9. A Dispute Pack transports the relevant material.
10. A Verification Engine evaluates defined claims and determines supported outcomes and Achieved Trust Profile.

Logical roles may share deployment components, but role combination does not erase control relationships.

Replication is not independence.

Submission is not Commitment.

Storage is not Preservation.

A valid object is not necessarily complete evidence.

A valid Signature is not automatically Authorization.

An Intended Trust Profile is not automatically achieved.

Protocol Verification is not business truth, Legal Validity, or Regulatory Compliance.

The complete system objective is:

> **Create evidence during consequential workflows, protect its history through layered and explicit controls, preserve what future interpretation requires, and enable independent systems to verify bounded claims without trusting the producer's narrative alone.**

The architectural rule remains:

> **Proof, not logs.**
