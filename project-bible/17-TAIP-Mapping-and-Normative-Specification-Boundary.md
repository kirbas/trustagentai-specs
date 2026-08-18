# Chapter 17 — TAIP Mapping and Normative Specification Boundary

> **The Project Bible defines the architecture TrustAgentAI must preserve; TAIP converts that architecture into precise, versioned, testable interoperability obligations without allowing profiles, bindings, SDKs, products, or examples to redefine Core meaning.**

## Purpose

This chapter defines how the **TrustAgentAI Project Bible** maps into the **TrustAgentAI Interoperability Protocol (TAIP)** and establishes the boundary between architectural intent, normative protocol specifications, governed supporting artifacts, informative guidance, and implementation behavior.

The mapping exists to ensure that every material architectural invariant and requirement becomes an identifiable protocol obligation, profile rule, Registry entry, schema constraint, conformance assertion, governance responsibility, or explicitly documented out-of-scope concern.

This chapter establishes:

- the TrustAgentAI documentation and specification stack;
- the distinct authority of the Project Bible and TAIP;
- normative, informative, reference, experimental, and implementation-specific boundaries;
- requirement language, stable identifiers, source precedence, and conflict handling;
- TAIP Core, module, profile, Registry, schema, binding, extension, and conformance boundaries;
- relationships among test vectors, conformance suites, reference implementations, APIs, SDKs, and products;
- bidirectional traceability from architecture to protocol, tests, and Verification evidence;
- requirement coverage, lifecycle, versioning, compatibility, deprecation, errata, and migration;
- security, privacy, operational, and governance mapping;
- release-readiness and conformance-claim criteria;
- mapping invariants and architectural requirements.

This chapter maps the architecture established in [01-Philosophy.md](01-Philosophy.md), [02-Executive-Summary.md](02-Executive-Summary.md), [03-Problem-Statement.md](03-Problem-Statement.md), [04-Design-Principles.md](04-Design-Principles.md), [05-System-Overview.md](05-System-Overview.md), and [06-Protocol-Objects.md](06-Protocol-Objects.md).

It incorporates the detailed semantics defined in [07-Evidence-Record-Specification.md](07-Evidence-Record-Specification.md), [08-Hash-Chain-Specification.md](08-Hash-Chain-Specification.md), [09-Witness-Observation-Specification.md](09-Witness-Observation-Specification.md), [10-Checkpoints-and-External-Anchors.md](10-Checkpoints-and-External-Anchors.md), [11-Key-Transparency.md](11-Key-Transparency.md), [12-Preservation.md](12-Preservation.md), [13-Dispute-Packs.md](13-Dispute-Packs.md), [14-Verification.md](14-Verification.md), [15-Trust-Profiles.md](15-Trust-Profiles.md), and [16-Protocol-APIs-and-SDK-Boundaries.md](16-Protocol-APIs-and-SDK-Boundaries.md).

It also applies the canonical terms in [Terminology.md](Terminology.md), abbreviations and identifier families in [Acronyms.md](Acronyms.md), and document roles in [Document-Status.md](Document-Status.md).

This chapter defines architectural mapping and specification-boundary semantics.

It does not itself provide every final TAIP field, encoding, algorithm suite, endpoint, Registry value, Trust Profile, or test vector. It defines where those normative details belong, how they derive from the architecture, and how their coverage and authority must be demonstrated.

---

# 17.1 TAIP Mapping Definition

**TAIP Mapping** is the governed process of translating Project Bible architecture into normative interoperable specifications and traceable supporting artifacts.

A complete mapping identifies:

- architectural source;
- preserved meaning or failure boundary;
- responsible TAIP module or governed artifact;
- normative rule or explicit out-of-scope disposition;
- applicable schema, Registry, profile, or binding;
- conformance and test coverage;
- version and lifecycle state;
- security and privacy consequences.

```text
Architectural Intent
      ▼
Stable Invariant or Requirement
      ▼
Normative TAIP Rule
      ▼
Machine-Testable Assertion
      ▼
Conformance and Verification Evidence
```

Mapping is not a prose summary. It is a traceable transformation from architectural obligation to independently evaluable behavior.

---

# 17.2 Mapping Objectives

The mapping process should ensure that TAIP is:

- faithful to the Project Bible;
- precise enough for independent implementation;
- modular without semantic fragmentation;
- versioned and historically interpretable;
- testable across positive and negative cases;
- safe under unsupported or unknown semantics;
- independent of one product, vendor, or programming language;
- extensible without silent Core redefinition;
- clear about security, privacy, and legal boundaries;
- governed through accountable change.

The goal is not to copy every architectural paragraph into protocol text.

The goal is to preserve every interoperability-relevant meaning and explicitly dispose of every architectural obligation.

---

# 17.3 Scope

This chapter applies to:

- TAIP Core specifications;
- Protocol Object modules;
- Trust Profiles;
- schemas and canonicalization specifications;
- identifier, algorithm, object-type, and extension Registries;
- transport and API bindings;
- Dispute Pack and export formats;
- Verification outcome and report specifications;
- conformance requirements and test suites;
- reference implementations and SDK contracts;
- governance records affecting normative interpretation;
- version, compatibility, migration, and deprecation artifacts.

It does not require every architectural consideration to become a Core wire field.

Some obligations belong in profiles, governance, deployment conformance, operational controls, or external legal and business processes. Their placement must still be explicit.

---

# 17.4 Documentation and Specification Stack

TrustAgentAI uses a layered documentation model:

```text
Philosophy and Manifesto
          ▼
TrustAgentAI Project Bible
          ▼
Architectural Invariants and Requirements
          ▼
TAIP Core and Normative Modules
          ├── Trust Profiles
          ├── Registries
          ├── Schemas
          ├── Binding Specifications
          └── Conformance Suites and Test Vectors
                    ▼
Reference Implementations, APIs, and SDKs
                    ▼
Deployments and Products
```

Lower layers implement or specialize upper-layer intent. They must not silently contradict it.

The stack separates durable architecture from protocol-version detail and protocol meaning from product behavior.

---

# 17.5 Role of the Project Bible

The Project Bible is the architectural foundation of TrustAgentAI.

It defines:

- the accountability problem;
- long-term design principles;
- trust and control boundaries;
- conceptual roles;
- evidence and lifecycle models;
- security and privacy objectives;
- stable invariants;
- architectural requirements;
- distinctions that protocol evolution must preserve.

The Project Bible is intended to be more stable than an individual TAIP version.

It may describe several acceptable mechanisms or leave concrete encodings to later specifications. Architectural statements become interoperability obligations only through an applicable normative mapping, except where a Project Bible requirement directly constrains all conforming TrustAgentAI specifications.

---

# 17.6 Role of TAIP

TAIP is the normative interoperability layer derived from the Project Bible.

TAIP defines or governs:

- Protocol Object types and semantic rules;
- identifiers and namespaces;
- canonicalization and cryptographic inputs;
- Signature and proof interpretation;
- evidence lifecycle and historical state;
- Hash Chains and Commitment evidence;
- Witness, Checkpoint, Anchor, and Key Transparency semantics;
- Preservation and Dispute Pack interoperability;
- Verification Contexts, outcomes, and reports;
- extensions and compatibility behavior;
- binding and conformance obligations.

Where an implementation claims TAIP interoperability, the applicable TAIP version is authoritative for concrete protocol behavior.

TAIP must remain faithful to the Project Bible's architectural invariants.

---

# 17.7 Normative Specification Boundary

Material is **normative** when it defines a requirement, prohibition, permission, semantic rule, algorithm, value, structure, process, or outcome that affects conformance or interoperable interpretation.

Normative material may reside in:

- TAIP Core;
- an incorporated normative module;
- a required Trust Profile;
- a governed Registry entry;
- a schema or canonicalization specification;
- a binding specification;
- a conformance specification.

Normative dependencies must be identified by stable name, version, and applicability.

A document does not become normative merely because an implementation links to it. A blog post, SDK behavior, issue comment, dashboard, or example cannot silently supply mandatory meaning.

---

# 17.8 Informative Material

**Informative** material explains, illustrates, motivates, or recommends without independently creating a conformance obligation.

Informative material may include:

- rationale;
- examples;
- diagrams;
- deployment guidance;
- tutorials;
- implementation notes;
- threat discussion;
- historical background;
- non-binding comparisons.

Informative material must not contradict normative text.

If an example reveals a required rule absent from normative text, the specification is incomplete. The remedy is to add or clarify the normative requirement, not to make implementations reverse-engineer examples.

---

# 17.9 Normative Requirement Language

TrustAgentAI normative prose uses requirement terms such as:

- **MUST** and **MUST NOT** for mandatory obligations and prohibitions;
- **SHOULD** and **SHOULD NOT** for strong expectations with potentially valid exceptions;
- **MAY** for permitted behavior;
- declarative normative statements where semantic rules do not fit an implementation action.

Each specification must define how these terms are interpreted, preferably by reference to a stable requirements-language convention.

Lowercase ordinary uses of `must`, `should`, or `may` should be avoided where they could be confused with normative force.

Requirements must identify their subject, condition, obligation, and observable result precisely enough for independent evaluation.

---

# 17.10 Normative Statements and Semantic Rules

Not every normative rule is best expressed as a single keyword sentence.

Normative semantics may also be defined through:

- formal data models;
- state machines;
- algorithms and pseudocode;
- tables of permitted and prohibited values;
- canonicalization procedures;
- outcome composition rules;
- Registry constraints;
- schema assertions;
- testable invariants.

The normative status of such material must be explicit.

Prose, tables, algorithms, schemas, and test assertions must not disagree. If several representations define the same rule, their precedence and consistency requirements must be specified.

---

# 17.11 Specification Authorities and Roles

The specification ecosystem may involve:

- a **Project Bible Authority** governing architectural text;
- a **TAIP Authority** approving Core and module versions;
- **Profile Authorities** governing Trust Profiles;
- **Registry Authorities** governing assigned values and status;
- **Schema Maintainers**;
- **Binding Maintainers**;
- **Conformance Maintainers**;
- **Reference Implementation Maintainers**;
- reviewers, security experts, implementers, and adopters;
- governance bodies resolving disputes and changes.

One Organization may perform several roles, but each decision must be attributable to the authority responsible for that layer.

Control of a repository, SDK package, service, or domain name does not automatically grant authority to redefine TAIP.

---

# 17.12 Source Precedence

The governing specification set must define source precedence.

A representative order is:

```text
Applicable TAIP Core Version
        ▼
Incorporated Normative Modules
        ▼
Applicable Trust Profile and Registry Entries
        ▼
Schema, Canonicalization, and Binding Specifications
        ▼
Conformance Specifications
        ▼
Reference Code, SDKs, Examples, and Product Documentation
```

The Project Bible constrains the architecture the normative set is permitted to implement. Within one released protocol context, TAIP defines concrete interoperability behavior.

Precedence does not authorize a lower layer to violate an architectural invariant. Such a conflict is a specification defect requiring governance action.

---

# 17.13 Ambiguity and Conflict Resolution

Normative ambiguity exists when conforming implementers can reasonably derive incompatible behavior from the same specification set.

Conflict may occur among:

- prose and schema;
- Core and module text;
- Registry and profile;
- algorithm and test vector;
- binding and object semantics;
- historical and current versions;
- translations;
- errata and original text.

Implementations must not resolve material ambiguity through undocumented local preference while claiming equivalent conformance.

The conflict should produce an issue, erratum, Unsupported outcome, or governed interpretation according to severity. Security-sensitive ambiguity should fail safely.

---

# 17.14 Modular Specification Structure

TAIP may be divided into modules to support review, implementation, and evolution.

Each module should identify:

- stable module ID and version;
- normative or informative status;
- scope and exclusions;
- dependencies;
- defined types, operations, or Registries;
- conformance obligations;
- extension points;
- compatibility and lifecycle status;
- security and privacy considerations.

Modularity must not create hidden semantic dependencies.

An implementation claiming a module must identify required Core version and mandatory companion modules. Circular dependencies that provide no independent semantic foundation must be rejected.

---

# 17.15 TAIP Core Boundary

**TAIP Core** contains semantics that must remain common across interoperable TrustAgentAI implementations.

Core should define or govern:

- common Protocol Object envelope and type dispatch;
- version and extension behavior;
- identifier and typed-reference foundations;
- canonical interpretation boundaries;
- lifecycle distinctions;
- validation-layer and outcome foundations;
- critical semantics handling;
- common security and privacy rules;
- conformance-claim structure.

Core should remain minimal enough to be stable but complete enough to prevent incompatible reinterpretation.

Mechanisms that vary by assurance objective may belong in profiles or modules rather than Core, provided the extension boundary remains explicit.

---

# 17.16 Protocol Object Module Mapping

Every interoperable Protocol Object type should map to a normative module or Core definition that identifies:

- type ID and namespace;
- semantic purpose;
- producer and issuer roles;
- required and optional properties;
- references and dependencies;
- lifecycle and correction rules;
- canonicalization and cryptographic coverage;
- validation and outcome behavior;
- privacy and disclosure semantics;
- version and extension rules.

Object type names must not be reused for incompatible semantics.

A schema alone is insufficient when it cannot express lifecycle, trust, historical, or claim-bounded meaning.

---

# 17.17 Identifier and Canonicalization Mapping

Architectural requirements for stable identity and deterministic cryptographic input map into TAIP rules for:

- identifier namespaces and syntax;
- assignment responsibility;
- normalization and comparison;
- uniqueness and collision handling;
- identifier/digest separation;
- canonical logical properties;
- representation and encoding;
- number, time, Unicode, and ordering rules;
- excluded transport and storage metadata;
- domain separation;
- cross-representation equivalence.

These rules must be precise enough that independent implementations derive identical canonical input for equivalent governed content.

An SDK serialization or database representation must not become the undocumented canonicalization algorithm.

---

# 17.18 Cryptographic Specification and Registry Mapping

Cryptographic architecture maps into:

- algorithm identifiers and Registries;
- permitted parameter sets;
- key-type compatibility;
- Signature and proof encodings;
- canonical cryptographic input;
- Key Purpose;
- Historical Key State evaluation;
- deprecation and security-strength policy;
- agility, renewal, and migration;
- invalid, unsupported, and unsafe behavior.

TAIP Core should define selection and interpretation semantics. Trust Profiles or algorithm profiles may select permitted suites for an assurance objective.

Algorithm names without parameters, encodings, purpose, and historical policy are incomplete normative definitions.

---

# 17.19 Lifecycle and Historical Integrity Mapping

Architectural lifecycle distinctions map into normative definitions for:

- draft and finalization boundaries;
- Signature;
- Submission;
- Acceptance;
- Commitment;
- Witnessing;
- Checkpointing;
- anchoring;
- Preservation;
- Verification;
- correction, supersession, revocation, reversal, and migration.

Each state must identify the evidence required to support it.

Bindings and implementations may expose additional operational states, but they must not conflate them with TAIP protocol states.

---

# 17.20 Hash Chain, Witness, Checkpoint, and Anchor Mapping

Historical-assurance architecture maps into modules defining:

- Chain identity, entries, predecessor relationships, and Chain Heads;
- Commitment Receipts and proof scope;
- fork, gap, rollback, merge, and closure behavior;
- Witness identity, eligibility, Observation Scope, and quorum;
- independence and Control Domain criteria selected by profiles;
- Checkpoint targets, cadence, authority, and consistency;
- External Anchor namespace, publication, finality, and proof semantics.

Each artifact must state what it proves and what it does not prove.

No single mechanism may silently absorb the semantics of the others.

---

# 17.21 Key Transparency Mapping

Historical identity architecture maps into normative Key Transparency rules for:

- Protocol Identity and KID separation;
- key-event and key-state objects;
- Key Purpose;
- issuance, activation, rotation, suspension, revocation, compromise, and retirement;
- inclusion and consistency proofs;
- Registry or log checkpoints;
- gossip and split-view evidence;
- historical resolution boundaries;
- operator and governance Authority;
- privacy-aware query behavior.

TAIP must define which Key Transparency evidence is mandatory for each historical Signature conclusion or delegate that selection explicitly to a Trust Profile.

Current directory state must not replace Historical Key State when the claim requires historical interpretation.

---

# 17.22 Preservation and Dispute Pack Mapping

Long-term verifiability maps into normative rules for:

- Evidence Lifetime;
- retained objects and Verification Dependencies;
- integrity, custody, immutability, availability, and recoverability evidence;
- cryptographic renewal and format migration;
- omission, redaction, encryption, and deletion state;
- Dispute Pack Manifest structure;
- entry identity and integrity;
- included, external, unavailable, and unsupported dependencies;
- package version and portability;
- offline Verification.

Preservation modules must distinguish stored, replicated, immutable, recoverable, and preserved.

Pack specifications must distinguish valid container structure from evidence Validity and Completeness.

---

# 17.23 Verification and Outcome Mapping

Verification architecture maps into normative definitions for:

- Verification Context;
- focal claims and historical boundaries;
- Verification Dependency Graph;
- validation layers;
- structural, semantic, cryptographic, historical, lifecycle, and Completeness checks;
- Intended and Achieved Trust Profiles;
- control and claim result composition;
- Verification Report identity and protection;
- reproducibility and resolver evidence.

TAIP must define stable outcome categories for at least understood invalidity, incompleteness, indeterminacy, unsupported semantics, unavailability, conflict, warning, and not-evaluated state where applicable.

One Boolean cannot preserve these distinctions.

---

# 17.24 Trust Profile Mapping

Assurance composition maps into versioned Trust Profile specifications defining:

- assurance objective and scope;
- mandatory, conditional, optional, recommended, and prohibited controls;
- evidence and dependency requirements;
- algorithms, key custody, Authority, Chains, Witnesses, Checkpoints, anchors, and Preservation;
- thresholds, independence, quorum, and cadence;
- exceptions and degradation;
- Intended/Achieved evaluation;
- conformance and test requirements.

Profiles specialize TAIP without redefining Core semantics.

A profile identifier or numeric level does not create assurance by name. Exact normative requirements and achieved evidence remain necessary.

---

# 17.25 Registry Boundary

Registries provide governed assignments and metadata without replacing the specification that defines their semantics.

TAIP Registries may cover:

- Protocol Object types;
- identifier namespaces;
- algorithms and parameters;
- Key Purposes;
- extensions;
- outcome codes;
- media types and bindings;
- Trust Profile identifiers;
- test and conformance capabilities.

Each Registry must define allocation authority, uniqueness, entry schema, lifecycle state, versioning, deprecation, conflict, and preservation.

A Registry entry cannot introduce a new mandatory semantic class unless the governing specification authorizes that extension point.

---

# 17.26 Schema Boundary

Schemas provide machine-evaluable structural constraints.

They may define:

- required and optional fields;
- data types and value forms;
- enumerations and patterns;
- object nesting;
- extension locations;
- conditional structural constraints;
- representation versions.

Schemas do not automatically define:

- business or protocol Authority;
- lifecycle evidence;
- canonicalization unless expressly incorporated;
- cryptographic validity;
- historical state;
- Completeness;
- Trust Profile achievement.

Schema validation is one normative layer. Semantic prose and algorithms remain necessary where schema languages cannot express the complete rule.

---

# 17.27 Binding Specification Boundary

Bindings map TAIP operations and representations to transports such as HTTP, RPC, messaging, streaming, files, or local interfaces.

A normative binding may define:

- operations and endpoints;
- request and response representations;
- media types;
- authentication mechanisms;
- transport status mappings;
- asynchronous, retry, batching, pagination, and streaming behavior;
- capability negotiation;
- binding-specific security and privacy requirements.

Bindings must preserve TAIP object, lifecycle, outcome, and trust semantics.

Transport success, URLs, headers, queue offsets, and connection identity must not acquire Core meaning unless explicitly bound by TAIP.

---

# 17.28 API, SDK, and Reference Binding Mapping

APIs and SDKs expose normative protocol behavior but do not define it independently.

Their documentation should map:

- each operation to a TAIP rule or binding;
- each type to a normative Protocol Object or derived representation;
- each error to stable outcome semantics;
- each default to a permitted protocol choice;
- each retry or cache behavior to defined safety conditions;
- each unsupported feature to an explicit capability result.

Reference bindings may be normative when formally designated and versioned.

Reference APIs, SDKs, examples, and generated code remain informative or implementation-specific unless an applicable specification explicitly grants normative status.

---

# 17.29 Test Vector Boundary

A **test vector** provides defined inputs and expected outputs for a bounded normative assertion.

Test vectors may cover:

- parsing and structure;
- canonicalization;
- identifiers and digests;
- cryptographic operations;
- Chain, Witness, Checkpoint, Anchor, and key-history proofs;
- Preservation and Dispute Packs;
- Verification Outcomes;
- Trust Profile controls;
- API behavior;
- version and governance cases.

Every vector should identify the requirement or semantic rule it tests, applicable versions, inputs, expected result, and rationale for negative cases.

A vector cannot silently create a rule absent from normative text.

---

# 17.30 Conformance Suite Boundary

A conformance suite evaluates a declared implementation, module, binding, profile, or deployment scope against identified normative requirements.

A suite should define:

- conformance target and role;
- specification versions;
- mandatory and optional feature sets;
- test selection and coverage;
- execution environment and assumptions;
- expected outcomes;
- evidence and report format;
- pass, fail, unsupported, skipped, and not-applicable semantics;
- known limitations.

Passing a subset must not be represented as full conformance.

Conformance tests evaluate specified behavior. They do not automatically certify security, operational reliability, legal compliance, or every deployment configuration.

---

# 17.31 Reference Implementation Boundary

A reference implementation may demonstrate one conforming interpretation and support adoption.

It may provide:

- parsers and serializers;
- canonicalization;
- cryptographic functions;
- object builders;
- Verification logic;
- command-line tools;
- sample services;
- test fixtures.

Reference code is not the specification.

If code and normative text differ, the governed specification controls until an erratum or new version says otherwise. Undocumented quirks, permissive parsing, data structures, exception types, or performance shortcuts must not become conformance requirements.

Independent clean-room implementation must remain possible.

---

# 17.32 Deployment and Product Boundary

Products and deployments may add:

- user workflows;
- business Policies;
- risk scoring;
- operational databases;
- dashboards and analytics;
- proprietary integrations;
- enhanced controls;
- certification and service guarantees.

Those features may be valuable but are not TAIP Core by default.

A product must distinguish TAIP conformance from product capability, deployment assurance, Trust Profile achievement, certification, Regulatory Compliance, and business outcome.

Vendor-specific metadata must not be required for independent interpretation of Core evidence unless represented through a governed extension.

---

# 17.33 Extension Model Mapping

TAIP extension points should define:

- extension namespace and identifier;
- version;
- critical or non-critical status;
- permitted location and scope;
- canonicalization and cryptographic coverage;
- producer and consumer behavior;
- validation and outcome rules;
- Registry and governance authority;
- compatibility and deprecation.

Unknown critical extensions must produce an explicit Unsupported result.

Unknown non-critical extensions may be preserved and ignored only when the Core specification proves that doing so cannot change mandatory interpretation.

Extensions must not redefine existing Core fields or outcomes under new undocumented semantics.

---

# 17.34 Mandatory, Optional, Conditional, and Experimental Features

Feature status must remain explicit.

A specification may classify features as:

- mandatory for all conforming implementations of a role;
- mandatory for a named module or profile;
- conditional under a defined predicate;
- optional but interoperably specified;
- experimental;
- deprecated;
- prohibited.

Optional does not mean undefined.

An implemented optional feature must still follow its normative semantics. An experimental feature must use a distinct namespace or version and must not be represented as stable Core behavior.

Capability and conformance claims must disclose unsupported mandatory and optional features precisely.

---

# 17.35 Namespaces and Assigned Values

Every interoperable identifier family should have a governed namespace.

Namespace rules should define:

- authority and allocation process;
- syntax and normalization;
- uniqueness scope;
- collision handling;
- private-use and experimental ranges;
- reserved values;
- aliases and deprecated names;
- version and compatibility behavior;
- preservation.

Private-use values must not collide with globally assigned semantics when evidence crosses implementation boundaries.

A familiar acronym, media type, URI, or numeric value is not self-defining. Its namespace and governing specification determine meaning.

---

# 17.36 Chapter-Local Requirement Identifiers

Project Bible requirements use stable chapter-local identifiers such as:

```text
REQ-OBJ-001
REQ-VER-038
REQ-API-024
REQ-MAP-001
```

The middle component identifies the architectural domain. The numeric suffix identifies the requirement within that domain.

Published identifiers should remain stable.

Editorial movement or clarification should not renumber an unchanged requirement. A materially replaced requirement should receive governed lineage, supersession, or version treatment rather than silent ID reuse.

TAIP mapping records must reference exact source IDs where available.

---

# 17.37 Chapter-Local Invariant Identifiers

Architectural invariants use stable identifiers such as:

```text
INV-PHIL-003
INV-OBJ-017
INV-VER-023
INV-MAP-001
```

An invariant identifies a property intended to remain true across conforming protocol and implementation evolution.

TAIP may satisfy one invariant through several normative rules and modules.

Every mapped invariant should have a disposition showing how the specification preserves it and which tests or reviews detect violation.

---

# 17.38 Global Requirement and Invariant Identifiers

Cross-chapter consolidation may assign stable global identifiers:

```text
GREQ-*
GINV-*
```

Global identifiers support traceability across:

- Project Bible chapters;
- TAIP modules;
- Trust Profiles;
- Registries and schemas;
- conformance tests;
- Verification evidence;
- governance and compatibility decisions.

A global ID should reference source requirements rather than erase their provenance.

Consolidation must preserve differences among similar but non-equivalent obligations.

---

# 17.39 Traceability Matrix

A **traceability matrix** records the relationship among architecture, protocol, implementation obligations, and evidence.

Representative columns include:

| Field | Purpose |
|---|---|
| Source ID | Identifies Project Bible invariant or requirement |
| Source version | Preserves historical architectural context |
| TAIP disposition | Mapped, delegated, out of scope, deferred, or superseded |
| Normative target | Identifies exact module, section, rule, schema, Registry, or profile |
| Conformance target | Identifies affected implementation role |
| Test IDs | Identifies positive, negative, and boundary tests |
| Security/privacy impact | Identifies required analysis |
| Status | Tracks draft, active, deprecated, or unresolved state |

Traceability should be machine-readable where practical.

---

# 17.40 Coverage and Disposition

Every architectural invariant and requirement should receive one explicit disposition:

- **mapped to Core**;
- **mapped to a normative module**;
- **delegated to a Trust Profile**;
- **mapped to a Registry, schema, binding, or conformance rule**;
- **deployment or governance obligation**;
- **informative architectural guidance**;
- **outside TAIP scope with identified owner**;
- **deferred with tracked issue and risk**;
- **superseded by a governed architectural change**.

Silence is not a disposition.

Unmapped mandatory architecture prevents a complete claim that TAIP faithfully implements the Project Bible.

---

# 17.41 Architecture-to-TAIP Transformation

Mapping an architectural statement into TAIP should follow a disciplined procedure:

1. identify the source invariant, requirement, or semantic distinction;
2. state the failure or interoperability risk it prevents;
3. identify affected roles, objects, states, and claims;
4. decide Core, module, profile, Registry, schema, binding, conformance, governance, or out-of-scope placement;
5. write precise normative rules;
6. define identifiers, versions, dependencies, and extension behavior;
7. define positive, negative, boundary, and unsupported outcomes;
8. create test and traceability entries;
9. review security, privacy, and compatibility;
10. approve through governance.

Mechanism choice may vary, but the protected architectural meaning must remain visible throughout the transformation.

---

# 17.42 TAIP-to-Test Transformation

Every testable normative rule should map to conformance assertions.

Test design should identify:

- requirement and specification version;
- conformance role;
- preconditions;
- exact inputs;
- expected outputs or state transitions;
- permitted alternatives;
- invalid, unsupported, unavailable, and conflicting cases;
- relevant security and resource boundaries;
- deterministic comparison method.

Test families may use stable IDs such as:

```text
TST-STRUCT-*
TST-CANON-*
TST-CRYPTO-*
TST-CHAIN-*
TST-PROFILE-*
TST-API-*
TST-VERSION-*
TST-GOV-*
```

Tests must cover rejection and uncertainty behavior, not only successful examples.

---

# 17.43 Test-to-Evidence Transformation

Conformance execution should produce evidence sufficient to understand and reproduce the result.

A conformance report may identify:

- implementation and role;
- build, configuration, and environment;
- applicable specification, profile, Registry, schema, and suite versions;
- executed test IDs;
- input vector identities;
- observed outputs;
- pass, fail, skipped, unsupported, and not-applicable results;
- deviations, waivers, and limitations;
- report producer and integrity protection;
- execution time.

A badge or summary percentage is not sufficient when it hides failed mandatory tests or unsupported features.

Conformance evidence is separate from per-action Trust Profile achievement.

---

# 17.44 Requirement Lifecycle

Requirements may pass through states such as:

- proposed;
- draft;
- candidate;
- active;
- clarified by erratum;
- deprecated;
- superseded;
- withdrawn;
- archived.

Lifecycle transitions should identify authority, rationale, effective boundary, compatibility impact, and replacement relationship.

A requirement ID must not be silently reassigned to different semantics.

Historical conformance and evidence must remain interpretable against the requirement version applicable at the time.

---

# 17.45 Normative Dependencies

Normative specifications may depend upon other TrustAgentAI or external standards.

Each dependency should identify:

- authoritative title and stable identifier;
- exact version or bounded compatible range;
- normative sections or incorporated semantics;
- availability and preservation expectation;
- security and licensing considerations;
- conflict and errata behavior;
- migration path.

An unversioned web page, mutable `latest` link, or undocumented library behavior is not a durable normative dependency.

External standards should be incorporated narrowly enough that unrelated future changes do not alter historical TAIP meaning.

---

# 17.46 Version Pinning and Historical Context

TAIP evidence may depend upon several exact versions.

Normative artifacts should define how evidence binds to:

- TAIP Core;
- object and schema versions;
- canonicalization;
- algorithms;
- Trust Profiles;
- Registries;
- extensions;
- bindings and packages;
- Verification rules.

Version binding may be explicit in the object or resolvable through an immutable governed context.

Current specification text must not silently replace historical rules. Verification Dependencies required for historical interpretation must remain preservable.

---

# 17.47 Compatibility Mapping

Compatibility claims may be:

- backward compatible;
- forward compatible under defined unknown-field rules;
- wire compatible but semantically changed;
- compatible for one role or feature subset;
- compatible only through an adapter;
- incompatible.

Every compatibility claim should identify compared versions, roles, features, representations, and assumptions.

```text
Parses Successfully
≠
Semantically Compatible
≠
Conformant
```

Compatibility cannot depend upon ignoring unknown mandatory semantics or applying current defaults to historical evidence.

---

# 17.48 Deprecation and Migration Mapping

Deprecation communicates that new use is discouraged or prohibited under defined conditions while preserving historical interpretation.

Migration specifications should identify:

- source and target versions;
- affected types, algorithms, profiles, Registries, or bindings;
- preserved original evidence;
- transformation and renewal rules;
- Migration Records;
- validation and failure behavior;
- compatibility period;
- authority and effective boundary;
- rollback or recovery.

Deprecation does not erase historical evidence or make every prior use invalid automatically.

Migration must create accountable new state rather than rewrite the original protected meaning.

---

# 17.49 Errata and Clarifications

An erratum corrects a defect in a published specification without concealing the original publication.

Errata should identify:

- affected document, version, section, and requirement IDs;
- original text;
- corrected interpretation;
- classification as editorial or semantic;
- effective date;
- conformance and security impact;
- required implementation or test changes;
- approval authority.

A semantic correction that changes conforming behavior may require a new version rather than an in-place erratum.

Historical Verification should record whether an erratum was applied and why it governs the evaluated context.

---

# 17.50 Security Requirement Mapping

Every material security objective or threat should map to one or more of:

- normative prevention or detection controls;
- Trust Profile selection;
- validation and failure behavior;
- conformance tests;
- operational deployment requirements;
- governance and incident response;
- explicit residual risk.

Security considerations cannot remain solely informative when failure behavior affects interoperability.

For example, unknown critical semantics, algorithm downgrade, identifier collision, conflicting history, and unsafe parsing require normative outcomes and negative tests.

Threat-to-control mappings should identify assumptions and affected trust boundaries.

---

# 17.51 Privacy Requirement Mapping

Privacy objectives should map into normative or governed rules for:

- data minimization;
- purpose and claim scope;
- canonical versus disclosed representations;
- redaction and selective disclosure;
- encryption and access control;
- resolver and query privacy;
- retention and deletion;
- linkability and correlation;
- reporting and error disclosure;
- effect of withheld evidence on Verification.

Privacy guidance is insufficient when two implementations would disclose materially different mandatory information while claiming equivalent behavior.

The protocol should specify necessary disclosure semantics while allowing privacy-preserving mechanisms where their proof and interoperability are defined.

---

# 17.52 Operational Requirement Boundary

Some requirements affect deployment operation rather than wire interoperability.

Examples include:

- availability and recovery objectives;
- HSM operations;
- operator staffing and separation of duties;
- monitoring and incident response;
- backup schedules;
- service-level objectives;
- physical and administrative controls.

These may belong in Trust Profiles, deployment conformance, certification criteria, or operator Policy.

TAIP should define the evidence and interface semantics needed to claim or verify such controls when they affect assurance.

Operational guidance must not be presented as a Core protocol guarantee without verifiable requirements.

---

# 17.53 Governance Boundary

Governance determines who may change specifications, Registries, profiles, schemas, tests, and compatibility status.

The normative mapping should identify governance responsibility for:

- proposal and approval;
- version issuance;
- Registry allocation;
- security advisories;
- errata;
- deprecation and withdrawal;
- appeals and disputes;
- emergency action;
- preservation of historical artifacts.

Governance procedures may live outside TAIP Core, but their outputs affect normative interpretation and should be attributable, versioned, integrity-protected, and discoverable.

Repository write access alone is not sufficient governance authority.

---

# 17.54 RFCs and Architecture Decision Records

A TrustAgentAI **Request for Comments (RFC)** may propose significant architectural, protocol, profile, Registry, or governance change.

An **Architecture Decision Record (ADR)** preserves the context, alternatives, rationale, decision, and consequences of an important architectural choice.

RFCs and ADRs are governance records.

They do not become normative protocol text unless the governed process incorporates their requirements into an applicable specification or explicitly designates them as normative.

The resulting TAIP change should trace back to the RFC or ADR, while the normative specification remains the source used by implementers and conformance suites.

---

# 17.55 Normative Review Criteria

Review of a normative proposal should evaluate:

- architectural fidelity;
- semantic precision;
- role and trust boundaries;
- deterministic interpretation;
- negative and unsupported behavior;
- version and extension strategy;
- backward compatibility;
- security and privacy;
- implementation neutrality;
- operational feasibility;
- testability and coverage;
- preservation and historical interpretation;
- legal and business boundary clarity.

Review should include independent implementers where practical.

A rule understood only by its author or reference implementation is not ready for interoperable publication.

---

# 17.56 Unresolved Issues and Deferred Decisions

Draft specifications may contain unresolved decisions, but released normative behavior must not depend upon hidden or ambiguous placeholders.

Deferred items should identify:

- issue ID;
- affected architecture and requirements;
- reason for deferral;
- security, privacy, compatibility, and interoperability risk;
- temporary behavior or prohibition;
- responsible owner;
- target milestone.

Unresolved placeholders, open alternatives, or unspecified mandatory algorithms in a release candidate prevent conformance for the affected feature unless the feature is explicitly excluded or experimental.

---

# 17.57 Release Readiness

A TAIP release or module is ready for normative publication only when:

- scope and authority are explicit;
- normative and informative text are distinguishable;
- dependencies and versions are fixed;
- mandatory semantics are complete;
- architecture mapping and disposition are reviewed;
- schemas, Registries, and examples are consistent;
- positive, negative, boundary, and unsupported tests exist for critical rules;
- security and privacy review is complete;
- compatibility and migration are documented;
- unresolved material is bounded;
- artifacts are preservable and identifiable.

Publication pressure must not convert ambiguity into implicit implementation choice.

---

# 17.58 Conformance Claims

A conformance claim should identify:

- claimant and implementation role;
- TAIP Core and module versions;
- supported Trust Profiles, object types, bindings, and extensions;
- mandatory and optional feature coverage;
- conformance-suite version;
- execution result and report;
- environment and configuration;
- exclusions, deviations, and unsupported features;
- assessment time and validity conditions.

Conformance is scoped.

```text
TAIP Conformant Parser
≠
Conformant Verification Engine
≠
Conformant Deployment
≠
Trust Profile Achieved for Evidence
```

Marketing shorthand must not erase the declared scope.

---

# 17.59 Illustrative Mapping Example

Consider the architectural requirement:

```text
Submitted ≠ Accepted ≠ Committed
```

A complete mapping may produce:

| Layer | Mapped artifact |
|---|---|
| Project Bible | Lifecycle-separation invariant and requirement |
| TAIP Core | Stable state definitions and transition rules |
| Evidence module | Submission, Acceptance, and Commitment evidence semantics |
| Chain module | Commitment Receipt and Chain Entry requirements |
| API binding | Transport-status mapping and asynchronous operation behavior |
| Schema | Distinct typed state and receipt representations |
| Test suite | Success, rejection, timeout, duplicate, and false-Commitment cases |
| Verification | Separate checks and outcomes for each state |

No single HTTP status, database field, or SDK Boolean satisfies the mapping by itself.

---

# 17.60 Anti-Patterns and Relationship to Other Specifications

The following are architectural anti-patterns:

## Project Bible as Wire Specification

Expecting implementers to infer final fields and encodings from high-level architecture without TAIP rules.

## TAIP Without Architectural Traceability

Publishing protocol details without showing which accountability objectives and invariants they preserve.

## SDK as Normative Source

Treating one library's behavior as authoritative because prose is incomplete.

## Schema as Complete Semantics

Assuming structural validation defines Authority, history, cryptography, Completeness, and claim meaning.

## Test Vector as Hidden Requirement

Requiring behavior present only in expected test output and absent from normative text.

## Registry as Specification

Allowing assigned values to introduce semantics beyond the Registry's authorized extension point.

## Example-Driven Conformance

Testing only successful examples while omitting malformed, unsupported, conflicting, and missing-dependency cases.

## Requirement-ID Reuse

Assigning materially different meaning to an existing published identifier.

## Unversioned Normative Dependency

Depending upon mutable web content, `latest` artifacts, or undocumented software behavior.

## Silent Architecture Omission

Leaving a mandatory Project Bible requirement unmapped without disposition.

## Optional Means Undefined

Allowing optional implementations to invent incompatible semantics.

## Extension Redefines Core

Using a proprietary extension to reinterpret existing fields, outcomes, or lifecycle states.

## Parsing Equals Compatibility

Claiming compatibility because new bytes can be parsed while semantic meaning changed.

## Conformance Badge Without Scope

Publishing a generic badge that hides roles, versions, modules, failed tests, and exclusions.

## Normative Compliance Claim

Treating TAIP conformance as automatic Legal Validity, Regulatory Compliance, certification, business correctness, or security assurance.

The specification boundary can be summarized as:

```text
Project Bible       defines stable architectural intent
TAIP Core           defines common normative interoperability
TAIP Modules        define bounded protocol mechanisms
Trust Profiles      select assurance requirements
Registries          govern assigned values
Schemas             constrain representations
Bindings            map protocol operations to transports
Test Suites         evaluate normative behavior
Reference Code      demonstrates one implementation
Products            deliver deployment-specific capabilities
```

Every layer is necessary. No lower layer may silently redefine the meaning of a higher-authority normative or architectural source.

---

# TAIP Mapping Invariants

### INV-MAP-001 — Architectural Fidelity

TAIP and its governed artifacts MUST preserve applicable Project Bible invariants and architectural requirements.

### INV-MAP-002 — Boundary Explicitness

Normative protocol, informative guidance, reference behavior, experimental features, and implementation-specific behavior MUST remain distinguishable.

### INV-MAP-003 — Normative/Informative Separation

Informative examples, rationale, tutorials, diagrams, and implementation notes MUST NOT silently create or override conformance obligations.

### INV-MAP-004 — Source Precedence

Applicable normative-source precedence MUST be explicit and MUST NOT depend upon undocumented implementation preference.

### INV-MAP-005 — Architecture/Protocol Separation

The Project Bible's architectural role and TAIP's concrete normative interoperability role MUST remain distinct.

### INV-MAP-006 — Core/Profile Separation

Trust Profiles MAY specialize or select TAIP controls but MUST NOT silently redefine TAIP Core semantics.

### INV-MAP-007 — Schema/Semantics Separation

Schema validity MUST remain distinguishable from semantic, cryptographic, historical, lifecycle, Completeness, and Trust Profile validity.

### INV-MAP-008 — Registry Authority Boundary

A Registry entry MUST NOT introduce semantics outside the extension or assignment authority granted by its governing specification.

### INV-MAP-009 — Binding/Core Separation

Transport bindings MUST preserve Core object, lifecycle, trust, and outcome meaning and MUST NOT create contradictory semantics.

### INV-MAP-010 — SDK/Specification Separation

SDK behavior MUST NOT become a substitute for normative specification text.

### INV-MAP-011 — Reference/Normative Separation

Reference implementations and examples MUST remain non-authoritative unless an applicable governed specification explicitly designates specific material as normative.

### INV-MAP-012 — Implementation Neutrality

Normative meaning MUST NOT depend upon one vendor, repository, service, programming language, library, or deployment topology.

### INV-MAP-013 — Requirement Traceability

Every mapped normative obligation MUST retain traceability to its architectural source or an explicit independent protocol rationale.

### INV-MAP-014 — Invariant Coverage

Every applicable architectural invariant MUST have a documented protocol, profile, governance, deployment, or out-of-scope disposition.

### INV-MAP-015 — Identifier Stability

Published requirement, invariant, module, Registry, and test identifiers MUST NOT be silently reassigned to incompatible meaning.

### INV-MAP-016 — No Silent Omission

An applicable mandatory architectural requirement MUST NOT disappear from TAIP mapping without explicit disposition and authority.

### INV-MAP-017 — Test/Rule Separation

Test vectors and conformance suites MUST evaluate normative rules and MUST NOT silently invent them.

### INV-MAP-018 — Conformance Scope

Conformance claims MUST remain bounded to identified roles, versions, modules, profiles, bindings, features, suites, and environments.

### INV-MAP-019 — Version Explicitness

Every normative artifact and dependency whose version affects interpretation MUST remain explicitly versioned or immutably bound.

### INV-MAP-020 — Historical Interpretation

Protocol evolution MUST preserve access to the normative context required to interpret historical evidence and conformance results.

### INV-MAP-021 — Parse/Compatibility Separation

Syntactic parseability or wire compatibility MUST NOT automatically be represented as semantic compatibility or conformance.

### INV-MAP-022 — Extension Safety

Unknown mandatory or critical extension semantics MUST NOT be ignored, guessed, or converted into successful conformance or Verification.

### INV-MAP-023 — Unsupported Fail-Safe

Unsupported mandatory specification, schema, algorithm, Registry, profile, binding, or test semantics MUST produce an explicit non-success result.

### INV-MAP-024 — Lifecycle Distinction Preservation

TAIP mapping MUST preserve architectural distinctions among Submission, Acceptance, Commitment, Witnessing, Checkpointing, anchoring, Preservation, and Verification.

### INV-MAP-025 — Security Mapping

Security-critical failure behavior MUST be expressed through normative rules and testable outcomes rather than informative warning alone.

### INV-MAP-026 — Privacy Mapping

Privacy mechanisms and disclosure rules MUST NOT silently alter canonical meaning, mandatory Completeness, or Verification outcomes.

### INV-MAP-027 — Governance Authority

Normative change, Registry allocation, errata, deprecation, and withdrawal MUST remain attributable to the authority responsible for that artifact.

### INV-MAP-028 — Errata Non-Rewrite

Errata and clarifications MUST NOT conceal or silently rewrite the historical content and interpretation boundary of a published specification.

### INV-MAP-029 — Dependency Preservation

Normative dependencies required for future interpretation MUST remain identifiable, integrity-protected, and preservable.

### INV-MAP-030 — Protocol/External Conclusion Separation

TAIP conformance and protocol Verification MUST NOT automatically establish business truth, Legal Validity, Regulatory Compliance, certification, or complete security assurance.

### INV-MAP-031 — Bidirectional Traceability

Architecture-to-rule, rule-to-test, test-to-report, and report-to-applicable-version relationships MUST remain reconstructable.

### INV-MAP-032 — Reproducible Interpretation

Independent conforming implementations using equivalent normative contexts MUST derive equivalent canonical protocol conclusions.

---

# Architectural Requirements

### REQ-MAP-001

TAIP governance MUST maintain a mapping from applicable Project Bible invariants and requirements to normative artifacts or explicit dispositions.

### REQ-MAP-002

Every applicable architectural source MUST be classified as mapped, delegated, deployment-governed, informative, outside scope, deferred, or superseded.

### REQ-MAP-003

Every normative specification MUST identify its title, stable ID, version, authority, status, scope, dependencies, and effective boundary.

### REQ-MAP-004

Normative and informative material MUST be distinguishable within every specification whose content includes both.

### REQ-MAP-005

The applicable specification set MUST define source precedence and conflict behavior for Core, modules, profiles, Registries, schemas, bindings, and conformance artifacts.

### REQ-MAP-006

TAIP specifications MUST use canonical TrustAgentAI terms consistently or define explicit governed aliases and deprecations.

### REQ-MAP-007

Normative requirements MUST identify subject, condition, obligation or prohibition, and observable result sufficiently for independent conformance evaluation.

### REQ-MAP-008

Normative algorithms, tables, schemas, and state machines MUST declare their normative status and MUST remain consistent with governing prose.

### REQ-MAP-009

Published chapter-local requirement and invariant identifiers MUST remain stable and MUST be referenced by exact ID in mapping records where available.

### REQ-MAP-010

An existing requirement, invariant, module, Registry, or test ID MUST NOT be reused for materially incompatible semantics.

### REQ-MAP-011

Every TAIP module MUST identify required Core version, normative dependencies, defined features, conformance role, extension points, and compatibility status.

### REQ-MAP-012

TAIP Core MUST define the common semantics necessary to prevent incompatible interpretation across conforming implementations.

### REQ-MAP-013

Every interoperable Protocol Object type MUST have a normative definition identifying type, version, purpose, roles, properties, references, lifecycle, validation, and extension behavior.

### REQ-MAP-014

Every cryptographically protected object or artifact MUST define or normatively reference deterministic canonical input construction.

### REQ-MAP-015

Identifier specifications MUST define namespace, syntax, assignment, normalization, comparison, uniqueness, collision, persistence, and digest relationship.

### REQ-MAP-016

Cryptographic specifications MUST bind algorithm, parameters, key type, encoding, input, Key Purpose, historical policy, and unsupported behavior.

### REQ-MAP-017

TAIP MUST preserve distinct normative semantics for finalization, Submission, Acceptance, Commitment, Witnessing, Checkpointing, anchoring, Preservation, and Verification.

### REQ-MAP-018

Every claimed lifecycle state MUST have defined evidence and validation requirements.

### REQ-MAP-019

TAIP historical Verification rules MUST identify required historical keys, Policies, profiles, Registries, schemas, algorithms, and other dependencies.

### REQ-MAP-020

Verification specifications MUST define layered checks, dependency resolution, Completeness, profile evaluation, outcome composition, and Verification Report semantics.

### REQ-MAP-021

Outcome specifications MUST distinguish invalid, incomplete, indeterminate, unsupported, unavailable, conflicting, warning, and not-evaluated states where applicable.

### REQ-MAP-022

Trust Profiles MUST identify exact Core and module dependencies and MUST NOT redefine existing Core semantics incompatibly.

### REQ-MAP-023

Profile achievement MUST map to verified controls and evidence rather than implementation labels, configuration, or product tiers.

### REQ-MAP-024

Every Registry MUST define governing specification, authority, entry schema, uniqueness, allocation, versioning, lifecycle, conflict, and preservation rules.

### REQ-MAP-025

A Registry entry MUST remain within the assigned-value or extension authority granted by its governing specification.

### REQ-MAP-026

Schemas MUST identify the normative structural constraints they express and the semantic rules they cannot establish independently.

### REQ-MAP-027

Schema validity MUST NOT be represented as automatic cryptographic, historical, lifecycle, Completeness, profile, or claim validity.

### REQ-MAP-028

Binding specifications MUST map transport operations, representations, statuses, errors, asynchronous behavior, and security to exact TAIP semantics.

### REQ-MAP-029

Transport, API, and SDK behavior MUST NOT silently redefine Protocol Object identity, canonicalization, lifecycle state, trust boundary, or Verification Outcome.

### REQ-MAP-030

SDK documentation SHOULD map public protocol-relevant types, operations, defaults, errors, and capabilities to governing TAIP requirements.

### REQ-MAP-031

Reference implementations MUST be independently replaceable and MUST NOT supply undocumented mandatory semantics required for conformance.

### REQ-MAP-032

Extension specifications MUST define namespace, version, criticality, location, canonicalization, cryptographic coverage, processing, outcomes, and governance.

### REQ-MAP-033

Unknown mandatory or critical extension semantics MUST produce an explicit Unsupported result and MUST NOT be ignored.

### REQ-MAP-034

Optional features MUST have defined interoperable semantics when implemented and MUST NOT be treated as implementation-defined by default.

### REQ-MAP-035

Experimental features MUST use explicit status and isolation sufficient to prevent representation as stable Core conformance.

### REQ-MAP-036

Assigned-value namespaces MUST define authority, uniqueness scope, reserved and private-use ranges, collision behavior, aliases, and deprecation.

### REQ-MAP-037

Every normative dependency MUST identify a stable source, exact version or governed compatible range, incorporated scope, and conflict behavior.

### REQ-MAP-038

Normative dependencies required for historical interpretation MUST remain identifiable, integrity-verifiable, available through preservation, or explicitly reported as unavailable.

### REQ-MAP-039

A machine-readable traceability record SHOULD map source IDs, source versions, TAIP dispositions, normative targets, roles, test IDs, impacts, and status.

### REQ-MAP-040

Global requirement and invariant identifiers MUST retain links to their chapter-local sources and MUST NOT erase distinct non-equivalent obligations.

### REQ-MAP-041

Every conformance test MUST identify the normative requirement, semantic rule, or governed outcome it evaluates.

### REQ-MAP-042

Test vectors MUST identify applicable versions, exact inputs, expected outputs, comparison method, and rationale for negative or boundary cases.

### REQ-MAP-043

Critical normative behavior MUST have positive, negative, boundary, malformed, unsupported, conflicting, and resource-limited coverage where applicable.

### REQ-MAP-044

A conformance suite MUST identify target role, specification versions, feature set, test selection, environment, assumptions, outcomes, and known limitations.

### REQ-MAP-045

Skipped, unsupported, not-applicable, failed, and passed conformance tests MUST remain distinguishable.

### REQ-MAP-046

A conformance report MUST identify implementation, build, configuration, environment, suite and specification versions, executed tests, results, deviations, and report integrity.

### REQ-MAP-047

A badge, percentage, or summary MUST NOT hide failed mandatory tests, unsupported required features, or scope exclusions.

### REQ-MAP-048

Security-critical threats and failure modes MUST map to normative controls, explicit outcomes, conformance tests, governed operational requirements, or documented residual risk.

### REQ-MAP-049

Privacy-critical collection, disclosure, redaction, selective disclosure, retention, deletion, and resolver behavior MUST have explicit normative or governed disposition.

### REQ-MAP-050

Privacy filtering or withheld evidence MUST affect Completeness and Verification according to explicit rules and MUST NOT silently preserve full success.

### REQ-MAP-051

TAIP specifications and conformance claims MUST distinguish protocol results from business truth, Legal Validity, Regulatory Compliance, certification, and complete security assurance.

### REQ-MAP-052

Evidence and conformance artifacts MUST bind to the exact TAIP, module, schema, Registry, algorithm, profile, extension, binding, and suite versions required for interpretation.

### REQ-MAP-053

Compatibility claims MUST identify compared versions, roles, features, representations, direction, assumptions, and known semantic differences.

### REQ-MAP-054

Successful parsing or wire exchange MUST NOT be represented as semantic compatibility when mandatory meaning differs or is unsupported.

### REQ-MAP-055

Deprecation and migration specifications MUST preserve original evidence and identify source, target, effective boundary, transformation, validation, failure, and Migration Record requirements.

### REQ-MAP-056

Errata MUST identify affected artifact, version, section, IDs, original and corrected text, classification, effective boundary, impact, and approval authority.

### REQ-MAP-057

A semantic correction that changes conforming behavior MUST create a new version when an erratum cannot preserve unambiguous historical interpretation.

### REQ-MAP-058

Material normative ambiguity or conflict MUST be resolved through a governed erratum, version, interpretation, or explicit Unsupported behavior rather than undocumented implementation preference.

### REQ-MAP-059

RFCs and ADRs affecting normative behavior MUST trace to incorporated specification changes, while the applicable normative specification remains the implementation source of truth.

### REQ-MAP-060

Normative releases MUST identify unresolved items, bound experimental features, security and privacy review, compatibility, test coverage, and preservation status before publication.

### REQ-MAP-061

Conformance claims MUST identify exact role, versions, modules, profiles, bindings, extensions, suites, environment, exclusions, deviations, and validity conditions.

### REQ-MAP-062

Independent conforming implementations evaluating equivalent inputs under equivalent normative contexts SHOULD produce equivalent canonical protocol conclusions.

---

# Security Considerations

The normative specification boundary is itself a security boundary. Ambiguous authority, hidden dependencies, mutable definitions, permissive examples, and incomplete test coverage can weaken every implementation simultaneously.

## Specification Substitution

An attacker may replace a normative document, schema, Registry, profile, or test artifact with another version. Stable identity, integrity protection, authenticated publication, exact version binding, and preserved historical copies are required.

## Source-Precedence Confusion

Conflicting Core, module, profile, schema, or binding rules may let an implementation choose the weakest interpretation. Precedence and safe conflict outcomes must be explicit.

## Informative-to-Normative Smuggling

A requirement may exist only in an example, issue comment, tutorial, or reference implementation. Implementers then depend on an unreviewed source. Mandatory behavior must be promoted into governed normative text.

## Normative-to-Informative Downgrade

A security-critical obligation may be described as advice rather than a requirement, permitting implementations to omit it while claiming conformance. Threat mappings must identify which behavior requires normative force.

## Reference Implementation Capture

If conformance depends on reproducing one codebase's quirks, compromise or defect in that codebase becomes a protocol defect. Independent implementations and text-derived tests reduce capture.

## Registry Capture

A Registry operator may allocate misleading values, reassign semantics, suppress deprecation, or present split views. Allocation authority, append-only change evidence, signed snapshots, governance review, and conflict handling constrain abuse.

## Profile Weakening

A Profile Authority may weaken algorithms, independence, Preservation, or downgrade behavior while retaining a familiar profile name. Exact profile versions and content integrity must remain visible.

## Schema Under-Specification

A permissive schema may accept objects that violate semantic constraints. Conformance must include semantic validation beyond structural acceptance.

## Schema Overreach

A schema may accidentally reject valid future extensions or claim to establish Authority and history it cannot evaluate. The boundary between structural and higher-layer rules must remain explicit.

## Test Oracle Poisoning

Compromised expected outputs can normalize incorrect behavior. Test vectors require provenance, integrity, review, versioning, and linkage to normative rules.

## Positive-Only Testing

Suites containing only successful examples allow permissive parsers, ignored critical extensions, unsafe fallbacks, and false success. Negative, boundary, malformed, conflicting, and unsupported cases are essential.

## Coverage Inflation

A large test count may hide unmapped requirements or untested failure paths. Coverage should be measured against requirement and semantic-rule IDs rather than raw test quantity.

## Conformance Badge Abuse

Generic badges can hide role, version, module, profile, environment, exclusions, and failed tests. Machine-readable scoped reports are required for meaningful reliance.

## Requirement-ID Rebinding

Reusing a familiar ID for weaker semantics can make old mappings and test reports appear current. Published IDs require stable lineage and explicit supersession.

## Mutable Latest Dependency

Unversioned `latest` schemas, profiles, algorithms, or external standards can change historical interpretation. Normative dependencies need exact or governed compatible versions.

## Dependency Disappearance

Historical evidence may become unverifiable if specifications, Registries, schemas, or algorithms disappear. The normative set is part of the Verification Dependency Graph and requires Preservation.

## Critical Extension Bypass

An implementation may ignore an unknown extension to maximize successful results. Criticality and Unsupported outcomes must be normative and tested.

## Private Extension Collision

Private-use identifiers may escape their original domain and collide with assigned values. Namespaces and export behavior must preserve issuer and scope.

## Version Downgrade

Negotiation, adapters, or migration may select an older vulnerable rule set. Deprecation, algorithm policy, explicit negotiation, and no-silent-fallback requirements constrain downgrade.

## Semantic Compatibility Fraud

An implementation may parse new objects while ignoring meaning and claim compatibility. Compatibility must cover mandatory semantics, not only bytes.

## Errata Rewrite

A purported editorial erratum may materially change behavior without a new version, obscuring which rule governed historical evidence. Errata need classification, impact, authority, and preserved original text.

## Governance Capture

Control of approval, repository, Registry, release, and test infrastructure may allow coordinated weakening. Role separation, transparent records, review, appeals, and independent mirrors reduce single-party control.

## Unauthorized Publication

A plausible document or package may be published by an actor lacking specification Authority. Consumers must validate artifact provenance and governance status rather than trust location alone.

## Translation Divergence

Translated normative text may differ materially from the authoritative language. Specifications should identify authoritative versions and treat translations according to governed status.

## Circular Normative Dependency

Two modules may define each other without a stable foundation, or a Registry may rely upon the profile it authorizes. Dependency analysis must detect circular semantics and authority.

## Ambiguity Exploitation

Attackers benefit when validators choose different interpretations. Security-sensitive ambiguity should fail safely until governed clarification exists.

## Undefined Failure Behavior

Specifications that define success but not malformed, unavailable, conflicting, or unsupported behavior enable optimistic defaults. Failure semantics require equal normative care.

## Algorithm Name Ambiguity

Names without parameters, encodings, input construction, or Key Purpose may allow algorithm confusion. Cryptographic mappings must be complete.

## Hidden Operational Assumption

A protocol may appear portable while requiring one live service, clock, database, or credential not listed as a dependency. Traceability and offline test cases should expose hidden assumptions.

## Unsafe Example Code

Users often copy examples into production. Informative code should still avoid disabling validation, hardcoding secrets, accepting all algorithms, or hiding error states, and should clearly label simplifications.

## Release Pressure

Deadlines may encourage unresolved mandatory semantics, incomplete negative tests, or missing security review. Release readiness must treat such gaps as blockers or explicitly excluded experimental scope.

## Supply-Chain Compromise

Specification sites, repositories, package registries, generators, test bundles, and reference implementations may be compromised. Signed artifacts, reproducible release processes, protected branches, review, and independent verification reduce risk.

## Historical Context Confusion

Current normative text may be used to judge old evidence despite incompatible historical rules. Verification must bind the applicable historical specification set and errata policy.

## Residual-Risk Suppression

Mapping a threat to a control does not prove the risk is eliminated. Specifications and profiles should state assumptions, uncovered failure modes, and bounded conclusions.

---

# Privacy Considerations

Normative mapping determines which information every conforming implementation collects, exposes, retains, resolves, and packages. A poorly placed requirement can institutionalize unnecessary surveillance across the ecosystem.

## Mandatory Data Minimization

Core schemas should require only information necessary for interoperable accountability. Domain-specific or high-assurance evidence should be delegated to profiles or modules when universal collection is unnecessary.

## Schema Privacy

Required fields, identifier stability, cardinality, and external references can create linkability. Schema review must consider correlation and disclosure, not only structural completeness.

## Registry Privacy

Public Registries may expose identity, key, role, Organization, profile, or incident information. Registry schemas should distinguish public interoperability data from protected operational detail.

## Resolver Privacy

Normative resolution requirements may reveal which identities, keys, actions, or historical periods are under review. Offline snapshots, local mirrors, batching, privacy-preserving lookup, and minimum queries should remain possible where compatible.

## Test Vector Privacy

Conformance vectors and failure cases must use synthetic or properly authorized data. Production evidence, credentials, personal information, or dispute material must not be embedded casually in public suites.

## Conformance Report Privacy

Reports can expose system versions, topology, weaknesses, keys, operators, and unsupported features. Public summaries and protected detailed reports may have different disclosure scopes while preserving result integrity.

## Reference Telemetry

Reference implementations and SDKs should not add undisclosed telemetry merely because it aids development. Telemetry behavior belongs outside protocol semantics and requires explicit privacy governance.

## Identifier Linkability

Global IDs and traceability records improve accountability but may correlate actors across systems. Namespace and identifier design should use the narrowest scope compatible with Verification.

## Historical Preservation

Preserving normative dependencies is generally low-risk, but examples, issue discussions, RFCs, and reports may contain personal or confidential information. Preservation policy should separate necessary specification history from incidental sensitive content.

## Redaction Mapping

Specifications must define how redacted or selectively disclosed representations relate to canonical evidence and how missing meaning affects Verification. Redaction cannot be left to presentation convention alone.

## Optional Feature Disclosure

Capability and conformance documents may reveal security posture or business use. Claims should disclose what relying parties need without forcing unnecessary internal detail into public Core artifacts.

## Profile Privacy

Higher assurance profiles may require additional Witness, Preservation, or Authority evidence. They should not equate stronger assurance with indiscriminate disclosure.

## Error Semantics

Normative error precision can conflict with existence privacy. Bindings may define authorization-aware external responses while preserving detailed protected diagnostics and canonical outcome semantics.

## Deletion and Immutable History

Normative mappings should clarify which artifacts can be deleted, transformed, cryptographically erased, or retained as commitments. Protocol preservation must not create misleading promises of complete erasure.

## Legal and Jurisdictional Variation

Privacy obligations vary. Core should preserve interoperable evidence and disclosure state while allowing profiles and governance to impose stricter jurisdiction-specific rules without silently changing object meaning.

## Documentation Analytics

Specification websites and repositories may collect reader and contributor metadata. Such analytics are operational choices, not protocol requirements, and should remain proportionate and transparent.

## Contribution Privacy

RFCs, issues, and review histories can expose contributor identities and affiliations. Governance should balance accountable decision records with contributor safety and data minimization.

## Derived Machine-Readable Indexes

Traceability and global indexes may aggregate information from many documents. They should avoid copying unnecessary sensitive content and should preserve references rather than expanding confidential material.

---

# Design Rationale

TrustAgentAI needs an explicit normative mapping because architecture and protocol serve different purposes.

The Project Bible asks what properties must remain true for durable accountability: evidence over assertion, independent verifiability, historical interpretation, no silent downgrade, explicit uncertainty, portability, privacy proportionality, and separation of protocol results from external truth claims.

TAIP must convert those durable properties into exact, versioned behavior that independent implementations can execute and test. That requires concrete object types, identifiers, canonicalization, algorithms, lifecycle transitions, proof rules, outcomes, schemas, Registries, profiles, and bindings.

Without a mapping layer, two opposite failures become likely.

First, the Project Bible could be treated as a wire specification even though architectural prose intentionally leaves implementation choices open. Implementers would fill the gaps differently and still believe they were conforming.

Second, TAIP could accumulate detailed mechanisms without showing which architectural objectives they protect. A protocol revision might then preserve syntax while weakening accountability meaning.

The documentation stack prevents both failures:

```text
Architecture defines what must remain true.
TAIP defines interoperable behavior.
Profiles select assurance controls.
Registries assign governed values.
Schemas constrain representations.
Bindings expose operations.
Tests evaluate requirements.
Implementations realize the specification.
```

Traceability makes this stack auditable. A reviewer can move from an architectural requirement to its normative rule, from that rule to tests, and from test execution to conformance evidence. The reverse path explains why a field, failure code, or algorithm rule exists.

Stable identifiers and historical versions matter because evidence may outlive the current protocol release. An intact object cannot be verified if the specification, profile, Registry, canonicalization rule, or outcome semantics used to interpret it have disappeared or changed in place.

The boundary also protects implementation neutrality. Reference code, APIs, and SDKs accelerate adoption, but they remain replaceable. A protocol is genuinely interoperable only when independent teams can derive equivalent conclusions from the normative set without copying one vendor's hidden behavior.

Finally, explicit disposition prevents architectural requirements from vanishing between documents. Some obligations belong in Core, some in profiles, some in deployment governance, and some outside TAIP. Every placement can be legitimate. Silence is not.

---

# Summary

The Project Bible is TrustAgentAI's stable architectural foundation. TAIP is the normative interoperability protocol derived from it.

The mapping between them must:

- preserve architectural invariants and requirements;
- distinguish normative from informative and implementation-specific material;
- define source authority and precedence;
- place semantics explicitly in Core, modules, profiles, Registries, schemas, bindings, conformance, governance, or an identified external layer;
- maintain stable requirement, invariant, module, Registry, and test identifiers;
- provide bidirectional traceability from architecture to rules, tests, and reports;
- preserve exact versions and historical normative dependencies;
- define extension, compatibility, deprecation, migration, and errata behavior;
- map security and privacy risks into requirements and tests;
- keep reference code, APIs, SDKs, and products from redefining Core meaning;
- bound conformance claims to exact roles, versions, features, and evidence.

The governing relationship is:

```text
Project Bible
    │ defines stable architectural intent
    ▼
TAIP and Governed Normative Artifacts
    │ define testable interoperable behavior
    ▼
Conformance Suites and Reports
    │ demonstrate bounded implementation behavior
    ▼
Deployments, Products, and Evidence
```

TAIP is not a restatement of the Project Bible.

It is the precise, versioned, independently implementable expression of the architecture's interoperability obligations.

Every normative rule should be traceable to its purpose. Every architectural obligation should have a disposition. Every conformance claim should identify exactly what was tested.

That is how TrustAgentAI preserves **Proof, not logs** from architectural principle through protocol, implementation, and Verification.
