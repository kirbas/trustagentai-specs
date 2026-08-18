# Chapter 13 — Dispute Packs

> **A Dispute Pack makes a bounded accountability claim portable by carrying the evidence, dependencies, context, integrity references, and explicit limitations required for independent evaluation.**

## Purpose

This chapter defines the architectural specification for TrustAgentAI **Dispute Packs (DPs)**, **Dispute Pack Manifests (DPMs)**, and portable evidence assembly.

A Dispute Pack packages Accountability Evidence for review or Verification outside the originating production environment. It allows a verifier to determine what claims are in scope, which evidence is included, which dependencies are embedded or external, what is missing or redacted, and which Verification Context is expected.

This chapter establishes:

- Dispute Pack purpose, scope, roles, and trust boundaries;
- the canonical Dispute Pack Manifest model;
- focal Accountability Claims and Verification Context binding;
- package identity, versioning, integrity, and cryptographic protection;
- evidence selection, causal closure, and Verification Dependency closure;
- embedded, external, unavailable, omitted, redacted, and derived material;
- Completeness states and claim-bounded sufficiency;
- confidentiality, package encryption, compartmentalization, and key delivery;
- offline and independent Verification behavior;
- deterministic assembly, updates, merging, splitting, migration, and Preservation;
- delivery, receipt, custody, validation, and Verification Outcomes;
- Dispute Pack invariants and architectural requirements.

This chapter applies the common Protocol Object rules defined in [06-Protocol-Objects.md](06-Protocol-Objects.md).

It builds upon the Evidence Record model in [07-Evidence-Record-Specification.md](07-Evidence-Record-Specification.md), the Hash Chain model in [08-Hash-Chain-Specification.md](08-Hash-Chain-Specification.md), the Witness model in [09-Witness-Observation-Specification.md](09-Witness-Observation-Specification.md), the Checkpoint model in [10-Checkpoints-and-External-Anchors.md](10-Checkpoints-and-External-Anchors.md), the Historical Key State model in [11-Key-Transparency.md](11-Key-Transparency.md), and the Preservation model in [12-Preservation.md](12-Preservation.md).

It also applies the system model in [05-System-Overview.md](05-System-Overview.md), the principles in [04-Design-Principles.md](04-Design-Principles.md), and the canonical terms in [Terminology.md](Terminology.md) and [Acronyms.md](Acronyms.md).

This chapter defines architectural semantics.

It does not require one archive format, compression method, content-addressing scheme, transport, storage provider, disclosure system, or Verification implementation. It does not define final field names, concrete schemas, media types, wire encodings, court procedure, discovery rules, evidentiary admissibility, privilege, or jurisdiction-specific legal conclusions.

Those details belong to TAIP, Trust Profiles, package bindings, disclosure policies, cryptographic profiles, APIs, and applicable law.

---

# 13.1 Dispute Pack Definition

A **Dispute Pack (DP)** is a portable, governed collection of Accountability Evidence and Verification Dependencies assembled to support evaluation of defined Accountability Claims outside the originating operational environment.

A Dispute Pack contains or resolves to:

- a canonical Dispute Pack Manifest;
- focal Accountability Claims;
- relevant Protocol Objects;
- integrity and relationship references;
- applicable Verification Context;
- embedded or external dependencies;
- explicit omissions, redactions, unavailable material, and limitations;
- handling and confidentiality metadata where required.

```text
Claim
  +
Evidence
  +
Dependencies
  +
Verification Context
  +
Explicit Limitations
  =
Portable Dispute Pack
```

A file archive without governed package semantics is not automatically a Dispute Pack.

---

# 13.2 Purpose and Use Cases

Dispute Packs support independent or separated evaluation in contexts such as:

- internal investigation;
- counterparty dispute;
- external audit;
- regulatory review;
- incident analysis;
- insurance or contractual review;
- long-term archival Verification;
- organizational or provider migration;
- product interoperability testing;
- preservation handoff;
- expert or legal review under applicable governance.

The Pack reduces dependence upon privileged access to the producer's databases, dashboards, credentials, or proprietary application code.

It does not require unrestricted disclosure of every operational record. Its contents must be proportionate to the claims and Verification Context in scope.

---

# 13.3 Scope

This chapter applies to packages containing or referencing:

- Evidence Records and external payloads;
- Chain Entries, Commitment Receipts, and Chain proofs;
- Witness Observations and quorum evidence;
- Checkpoints and Anchor Evidence;
- Key Transparency Records and Historical Key State;
- Authority and Policy evidence;
- Preservation Evidence and Migration Records;
- schemas, canonicalization rules, registries, Trust Profiles, and algorithm definitions;
- prior Verification Reports;
- redacted or selectively disclosed representations;
- package-specific manifests, indexes, receipts, and custody evidence.

A Pack may address one claim, several related claims, a bounded incident, a transaction, a Chain range, or another governed scope.

---

# 13.4 Security and Assurance Objectives

Dispute Pack architecture has six primary objectives:

1. preserve a clear boundary around the evidence actually presented;
2. make each included item's identity and integrity independently verifiable;
3. carry enough historical context and dependencies for reproducible evaluation;
4. expose omissions, redactions, conflicts, external dependencies, and uncertainty;
5. enable portable Verification without trusting undocumented producer state;
6. protect confidentiality and handling constraints without creating false Completeness.

The architectural objective is:

> A verifier can determine what was packaged, why it was selected, which claims it may support, what was not supplied, and whether the package can be evaluated independently under a defined context.

---

# 13.5 What a Dispute Pack Establishes

Subject to successful Verification, a Dispute Pack may establish that:

- an identifiable assembler created a package with a defined Manifest;
- the Manifest committed to a defined package boundary;
- included entries match their declared identifiers and integrity values;
- defined relationships and dependency classifications were represented;
- omissions, redactions, unavailable material, and external references were declared;
- a particular Verification Context or Trust Profile was requested or expected;
- the package was delivered, received, preserved, or evaluated at supported boundaries;
- a conforming Verification Engine reached a bounded outcome over the available material.

The Pack transports and describes evidence. It does not create the historical facts represented by that evidence.

---

# 13.6 What a Dispute Pack Does Not Establish

A structurally and cryptographically valid Dispute Pack does not by itself prove:

- truth of every included assertion;
- business Authority or Policy compliance;
- historical Commitment of an included object;
- independence of Witnesses or custodians;
- validity of a Checkpoint or External Anchor;
- correct Historical Key State;
- completeness for every possible claim;
- absence of evidence outside the package;
- accuracy of an assembler's relevance classification;
- lawful disclosure or handling;
- Legal Validity, admissibility, Regulatory Compliance, or business truth.

```text
Valid Package
≠
Valid Evidence
≠
Complete Evidence
≠
Successful Claim
```

---

# 13.7 Conceptual Roles

A Dispute Pack workflow may involve:

- a **Requesting Party** defining the review need;
- a **Claim Owner** identifying focal Accountability Claims;
- a **Disclosure Authority** authorizing release or redaction;
- a **Dispute Pack Assembler** selecting and packaging material;
- an **Evidence Producer** supplying source evidence;
- a **Resolver** locating referenced dependencies;
- a **Preservation Service** retaining source and packaged state;
- a **Custodian** controlling transfer and handling;
- a **Recipient** receiving the Pack;
- a **Verification Engine** evaluating it;
- a **Verifier, Auditor, Investigator, or Reviewer** interpreting the result;
- a **Witness or Notary role** observing assembly, transfer, or package commitments where required.

One Organization may perform multiple roles. Role combination does not create independence automatically.

---

# 13.8 Dispute Pack Assembler

A **Dispute Pack Assembler** creates a governed package from source evidence and dependencies.

Its responsibilities may include:

- confirming request and claim scope;
- applying evidence-selection rules;
- resolving typed dependencies;
- preserving source provenance;
- generating canonical package entries;
- classifying embedded, external, omitted, redacted, and unavailable material;
- creating and protecting the Manifest;
- applying confidentiality controls;
- validating package integrity;
- producing assembly and delivery evidence.

The assembler does not determine evidence validity merely by including an item.

Where selection or redaction materially affects the supported claims, the assembler's Authority, Policy, methods, and limitations must remain identifiable.

---

# 13.9 Dispute Pack Manifest Definition

A **Dispute Pack Manifest (DPM)** is the canonical Protocol Object describing the Pack's identity, claims, contents, integrity references, dependencies, omissions, disclosure state, expected Verification Context, and handling semantics.

The DPM is the authoritative package boundary.

It may bind:

- pack identifier and version;
- Manifest type and version;
- assembler Protocol Identity;
- creation and packaging times;
- focal Accountability Claims;
- expected Verification Context;
- content inventory;
- typed relationships;
- integrity commitments;
- embedded and external dependencies;
- omissions, redactions, unavailable material, and conflicts;
- confidentiality and handling rules;
- prior or successor Pack versions;
- cryptographic protection.

---

# 13.10 Pack Identity and Version

Every Dispute Pack must have an unambiguous identifier and version within a governed namespace.

Identity and version rules must define:

- pack namespace;
- identifier generation;
- comparison and normalization;
- relationship to the Manifest digest;
- version succession;
- correction and supersession behavior;
- collision and duplicate handling;
- whether a reassembled identical package retains or changes identity.

```text
Pack Identifier
≠
Manifest Digest
≠
Archive Filename
```

A new package version must not silently replace the earlier package. The relationship and reason for change must remain explicit.

---

# 13.11 Focal Accountability Claims

A Dispute Pack must identify the **focal Accountability Claims** it is intended to support or evaluate.

A claim description should bind or reference:

- claim identifier and type;
- subject and Accountable Action;
- relevant object, event, transaction, or state;
- claimed time or historical boundary;
- claimant or requesting role where applicable;
- required Trust Profile or decision context;
- related claims and exclusions;
- expected verification questions.

Examples include:

```text
"Agent A authorized Transfer T under Policy P."
"Evidence Record E entered Chain C before Checkpoint K."
"Key K was valid for the required Key Purpose at boundary B."
```

An undefined request such as `export all relevant logs` is not a sufficient claim model.

---

# 13.12 Verification Context

The **Verification Context** defines the conditions under which the Pack is expected to be evaluated.

It may include:

- TAIP and object versions;
- Trust Profile and version;
- Policy and registry boundaries;
- Verification Time;
- historical time or Chain boundary;
- algorithm policy;
- resolver state;
- Historical Key State rules;
- disclosure and availability assumptions;
- required claims and Completeness criteria;
- jurisdictional or organizational context where protocol-relevant.

The Pack may include a recommended context without forcing every verifier to accept it.

A verifier using a different context must report the difference and resulting effect on outcomes.

---

# 13.13 Package Boundary

The Manifest defines exactly which entries, dependencies, and representations are inside the Pack and which are outside it.

The boundary must distinguish:

- embedded canonical objects;
- embedded derived or disclosed representations;
- embedded supporting resources;
- external dependencies;
- intentionally omitted material;
- redacted material;
- unavailable material;
- unknown or unresolved references;
- packaging-only metadata.

Filesystem presence alone does not determine membership. An undeclared file must not silently become trusted evidence merely because it appears in the archive.

A declared entry that is missing, altered, duplicated ambiguously, or replaced must produce an explicit validation result.

---

# 13.14 Logical Content Model

A Dispute Pack may logically contain:

```text
Dispute Pack Manifest
├── Accountability Claims
├── Verification Context
├── Canonical Protocol Objects
├── Historical Integrity Evidence
├── Identity, Key, Authority, and Policy Evidence
├── Verification Dependencies
├── Preservation and Custody Evidence
├── Redacted or Derived Representations
├── Prior Verification Reports
└── Omissions, Conflicts, and Handling Metadata
```

The logical model is independent of physical packaging.

One file may contain several logical entries, and one logical object may be chunked across several files, provided the mapping and integrity rules remain deterministic.

---

# 13.15 Manifest Entry Model

Each Manifest entry should identify:

- entry identifier;
- semantic role;
- Protocol Object type and version where applicable;
- object identifier;
- canonical digest or integrity value;
- package path or storage binding;
- media type and representation;
- size or bounded size metadata;
- encryption and compression information;
- relationship to claims and other entries;
- dependency classification;
- disclosure state;
- required or optional status;
- validation expectations.

Packaging locators must be normalized and safe. Entry semantics must not be inferred solely from filename extensions or directory names.

---

# 13.16 Identifiers and Integrity References

The Manifest must bind each included or referenced item to an identity and integrity expectation appropriate to its role.

Integrity references may include:

- canonical Protocol Object digests;
- package-representation digests;
- Merkle inclusion proofs;
- Chain positions and Checkpoints;
- signed external-resource commitments;
- chunk or segment digests;
- aggregate package commitments;
- another TAIP-defined method.

The digest algorithm and input construction must be explicit.

A package-representation digest verifies packaged bytes. It does not automatically prove that those bytes are the canonical Protocol Object or that the object entered historical Commitment.

---

# 13.17 Canonical and Packaged Representations

The canonical Protocol Object and its packaged representation may differ because of encryption, compression, chunking, container framing, or disclosure transformation.

```text
Canonical Protocol Object
        │ packaged as
        ▼
Compressed / Encrypted / Chunked Representation
```

The Manifest must state how the canonical object is recovered or related to the packaged representation.

Where exact canonical bytes are not disclosed, the Pack must identify whether the included material is a redacted view, derived representation, proof, commitment, or unavailable placeholder.

A normalized export must not be presented as the original signed bytes unless the applicable specification defines them as equivalent.

---

# 13.18 Physical Packaging Bindings

TAIP may define bindings for ZIP, TAR, directory trees, content-addressed bundles, multipart streams, object-store collections, or other containers.

A binding must define:

- entry-path normalization;
- ordering where relevant;
- duplicate-name behavior;
- symbolic link and special-file handling;
- compression and decompression limits;
- chunking and reassembly;
- encryption placement;
- Manifest discovery;
- deterministic metadata treatment;
- media type and version;
- safe extraction behavior.

The architectural model does not depend upon one container format.

A container's built-in checksum or password protection is not a substitute for governed object integrity and Manifest semantics.

---

# 13.19 Package Integrity

Package integrity binds the Manifest and declared entries into a verifiable package state.

It may use:

- a signed canonical Manifest containing entry digests;
- an aggregate Merkle root;
- a package-level digest referenced by external evidence;
- a Checkpoint or External Anchor;
- another deterministic TAIP-defined commitment.

Integrity verification must detect:

- missing declared entries;
- altered bytes;
- undeclared substitution;
- ambiguous duplicates;
- Manifest-entry mismatch;
- unsupported algorithms;
- unsafe or noncanonical packaging metadata where protected.

Package integrity does not prove semantic validity or Completeness.

---

# 13.20 Manifest Cryptographic Protection

The DPM must be cryptographically protected according to its type and Trust Profile.

Protection must bind properties whose alteration could change:

- Pack identity or version;
- assembler identity;
- focal claims;
- Verification Context;
- entry inventory;
- integrity references;
- relationship and dependency semantics;
- omission, redaction, or availability status;
- confidentiality and handling instructions;
- predecessor or successor relationship.

Multiple Signatures may represent assembler approval, disclosure approval, custody acknowledgment, or another typed role.

Signature count alone does not establish role, independence, or authorization.

---

# 13.21 Assembly Authority and Policy

Assembly may involve sensitive selection, disclosure, and transformation decisions.

The Pack should identify applicable Authority and Policy for:

- initiating assembly;
- accessing source evidence;
- selecting claim scope;
- disclosing or withholding material;
- creating redacted representations;
- including third-party evidence;
- encrypting and delivering the package;
- correcting or superseding a Pack;
- releasing custody or retention controls.

A cryptographically valid assembler Signature does not prove lawful disclosure or business Authority.

Where the assembler lacks permission for a required item, the Pack should represent the resulting omission or restricted dependency honestly rather than inventing Completeness.

---

# 13.22 Source Provenance

Each material entry should preserve enough provenance to distinguish:

- source Protocol Object;
- source service or custodian;
- retrieval or export method;
- retrieval time;
- original and package integrity values;
- transformations performed;
- assembler-generated metadata;
- source-authored metadata;
- external attestations;
- uncertainty or contested origin.

Provenance does not prove source truth. It makes origin and handling claims evaluable.

Screenshots, copied text, database exports, or rendered reports may be useful supporting material, but they must not be mislabeled as canonical Protocol Objects without the required identity and integrity relationship.

---

# 13.23 Evidence Selection

Evidence selection determines which material is included for the focal claims and Verification Context.

Selection rules may consider:

- claim relevance;
- causal relationships;
- historical boundary;
- Trust Profile requirements;
- mandatory object types;
- dependency closure;
- disclosure authorization;
- proportionality and minimization;
- package size and delivery constraints;
- known disputes and counterevidence;
- Preservation availability.

The selection method should be documented or identified when its behavior affects Completeness.

Selective packaging must not silently exclude known contradictory evidence while claiming neutrality or comprehensiveness.

---

# 13.24 Minimal and Comprehensive Packs

A **Minimal Pack** contains the smallest governed set expected to evaluate defined claims under a stated context.

A **Comprehensive Pack** contains a broader bounded collection intended to support multiple claims, alternate interpretations, or deeper review.

```text
Minimal
≠
Incomplete by definition

Comprehensive
≠
Universally complete
```

Both forms require a Manifest and explicit scope.

The Pack must not use labels such as `complete`, `full`, or `comprehensive` without identifying the population, claims, boundary, selection rule, and known limitations to which the label applies.

---

# 13.25 Claim-Centered Relevance

An entry is relevant when it supports, contradicts, qualifies, interprets, or is required to verify a focal claim.

Relevance may derive from:

- direct subject or object reference;
- causal linkage;
- Chain membership;
- Authority or Policy relationship;
- Historical Key State;
- Witness, Checkpoint, or Anchor dependency;
- common transaction or workflow scope;
- correction, revocation, or supersession;
- required schema, registry, or Trust Profile;
- known conflicting evidence.

Relevance is not identical to favorable support.

A conforming assembler must not define relevance as only evidence that supports the requesting party's preferred conclusion.

---

# 13.26 Causal and Historical Closure

A claim may depend upon a graph of intentions, authorizations, policy evaluations, decisions, executions, outcomes, corrections, and later historical evidence.

The Pack should identify the bounded causal and historical closure used for selection.

Closure may include:

- predecessor and successor events;
- referenced inputs and outputs;
- approvals and exceptions;
- execution confirmations and failures;
- correction or revocation records;
- Chain Entries and historical boundaries;
- conflicting or alternative branches.

The closure algorithm must handle cycles, missing references, and resource limits safely.

An unresolvable causal reference must remain explicit rather than being silently treated as irrelevant.

---

# 13.27 Verification Dependency Closure

Verification Dependency closure identifies the material required to interpret and verify the selected evidence.

It may include:

- schemas and canonicalization rules;
- algorithm definitions;
- Protocol Identity and Historical Key State;
- Authority and Policy evidence;
- registries and Trust Profiles;
- Chain, Witness, Checkpoint, and Anchor material;
- Preservation Evidence;
- extension definitions;
- migration and renewal evidence.

The Manifest must classify each required dependency as embedded, external, redacted, unavailable, unsupported, or omitted.

Self-contained packaging does not require embedding every public resource, but required external resources must be durably identifiable and integrity-protected.

---

# 13.28 Embedded and External Dependencies

An **embedded dependency** is carried within the Pack. An **external dependency** must be resolved outside it.

External dependency metadata should identify:

- stable dependency identifier;
- exact version;
- integrity value;
- semantic role;
- authoritative or acceptable resolver;
- retrieval requirements;
- confidentiality and access constraints;
- expected availability;
- fallback or preserved copy where applicable.

```text
External locator
≠
Resolved dependency
```

The Pack's portability claim must reflect its external dependencies. A package requiring privileged access to the originating production database is not independently portable for that verification path.

---

# 13.29 Offline Verification Profile

An **offline-verifiable Pack** contains all mandatory material required for defined Verification checks without network access at evaluation time.

It may still depend upon:

- the verifier's conforming software;
- preinstalled algorithm implementations;
- trusted local clocks or supplied time evidence;
- authorized decryption keys delivered separately;
- human or legal interpretation outside protocol scope.

The Manifest must state which checks are expected to work offline and which cannot.

Offline Verification strengthens resilience and privacy, but it does not guarantee Completeness, current revocation state, or absence of later evidence unless the Verification Context intentionally fixes a historical boundary.

---

# 13.30 External Resolution and Retrieval Evidence

When Verification retrieves external material, the result should preserve:

- requested identifier and version;
- resolver identity or trust role;
- returned representation;
- integrity verification;
- retrieval time;
- access or authorization result;
- cache or mirror status;
- conflict with other resolvers;
- failure or unavailability semantics.

Mutable network content must not silently satisfy a versioned dependency merely because a URL responds.

A Pack may include Retrieval Evidence or a later Verification Report may record resolution behavior. The original Manifest should remain unchanged unless a new Pack version is assembled.

---

# 13.31 Completeness Definition

**Completeness** is the degree to which the Pack contains or resolves the evidence and dependencies required for defined claims, scope, population, historical boundary, Verification Context, and Trust Profile.

Completeness is always bounded.

```text
Complete for Claim C under Context V
≠
Complete for every possible question
```

A Completeness conclusion may depend upon:

- known source population;
- claim and dependency closure rules;
- required object types;
- sequence or range coverage;
- omission and redaction state;
- external dependency availability;
- conflict and duplicate handling;
- Trust Profile requirements;
- evidence that the assembler could not suppress relevant material.

Structural validity alone does not establish Completeness.

---

# 13.32 Completeness States

TAIP may define claim-bounded states such as:

- **Complete** — all required material for the defined scope is supported;
- **Complete with declared optional omissions** — mandatory closure is supported and optional exclusions are explicit;
- **Incomplete** — known required material is absent, redacted, unavailable, or invalid;
- **Indeterminate** — the verifier cannot determine whether required material is complete;
- **Conflicting** — incompatible evidence prevents one completeness conclusion;
- **Unsupported** — the applicable completeness method or semantics are not understood;
- **Not Evaluated** — no completeness check was performed.

Labels must be accompanied by the claim, context, population, and method to which they apply.

---

# 13.33 Omissions

An **omission** is material not included in the Pack despite being known, expected, referenced, selected by a rule, or potentially relevant within the declared scope.

The Manifest should identify:

- omitted item or bounded category;
- relationship to claims;
- required or optional status;
- reason category;
- whether the material exists;
- whether it is externally available;
- expected effect on Verification and Completeness;
- responsible disclosure or selection rule;
- whether the omission is disputed.

Unknown evidence cannot always be enumerated. The Pack must distinguish `known omitted` from `not known to exist` and from `proven absent within a governed population`.

---

# 13.34 Unavailable and Unresolved Material

Material is **unavailable** when it is identified but cannot be retrieved or disclosed in the applicable workflow. A reference is **unresolved** when its target cannot be reliably identified, located, authenticated, or interpreted.

Reason categories may include:

- source deletion or corruption;
- access denial;
- missing decryption key;
- provider outage;
- unsupported format or algorithm;
- resolver failure;
- legal or policy restriction;
- disputed identity;
- missing version;
- unknown location.

Unavailable is not equivalent to nonexistent, invalid, or intentionally omitted.

The status and its effect on each claim must remain explicit.

---

# 13.35 Redaction and Selective Disclosure

Redaction or selective disclosure may protect sensitive material while revealing bounded claims or fields.

The Pack must identify:

- canonical source object or commitment;
- disclosed representation;
- redacted fields, sections, entries, or categories where permitted;
- transformation method;
- Disclosure Authority and Policy;
- integrity or proof relationship;
- reason category;
- effect on Verification and Completeness;
- access path for authorized fuller disclosure where applicable.

```text
Redacted Representation
≠
Complete Canonical Object
```

A blank field, removed file, or opaque placeholder is not sufficient redaction evidence unless its semantics are authenticated.

---

# 13.36 Derived Representations

A **derived representation** is material created from source evidence for presentation, search, analysis, translation, rendering, summarization, or interoperability.

Examples include:

- human-readable reports;
- tables or timelines;
- rendered JSON or XML;
- screenshots;
- translated text;
- normalized exports;
- graph visualizations;
- extracted fields;
- summaries produced by software or an Agent.

Derived material may improve usability but must remain distinguishable from canonical evidence.

The Manifest should identify source entries, derivation method and version, assembler or generator, generation time, integrity value, and known loss or interpretation limits.

Verification should rely upon canonical evidence or a governed proof relationship where the derived view is insufficient.

---

# 13.37 Confidentiality and Handling Metadata

A Pack may carry handling semantics such as:

- public, confidential, restricted, or privileged classification;
- recipient or role restrictions;
- purpose limitation;
- geographic or jurisdictional restrictions;
- export-control or contractual conditions;
- retention and disposition expectations;
- redistribution rules;
- notification requirements;
- secure-environment requirements;
- incident-reporting obligations.

Handling metadata must be attributable and integrity-protected where alteration would affect disclosure or custody.

TrustAgentAI represents handling requirements but does not determine their legal enforceability.

An authorized recipient still must apply applicable law, contract, and organizational governance.

---

# 13.38 Package Encryption

Dispute Pack contents may be encrypted at entry, compartment, or whole-package level.

The encryption binding must define:

- protected entries and metadata;
- encryption suite and parameters;
- key identifiers and versions;
- authenticated associated data;
- relationship between plaintext and ciphertext integrity;
- recipient or role binding;
- decryption and recovery behavior;
- key rotation or rewrapping;
- failure and unsupported semantics.

The minimum visible envelope should permit safe routing and interpretation without leaking unnecessary claim or identity information.

Encryption does not replace Manifest integrity, Signatures, access Authority, data minimization, or explicit missing-material outcomes.

---

# 13.39 Key Distribution and Recipient Binding

Decryption capability may be delivered through:

- recipient public-key encryption;
- a governed Key Management Service;
- threshold or escrow release;
- out-of-band key exchange;
- attribute- or role-based encryption;
- another TAIP-defined profile.

The Pack should preserve which recipient, role, or authorization context each key grant targets.

```text
Possession of Pack Ciphertext
≠
Authority to Decrypt
```

Key delivery must avoid placing the Pack and unrestricted decryption capability under an unintended single transfer channel where the Trust Profile requires separation.

Lost or unavailable decryption keys must produce explicit Verification limitations.

---

# 13.40 Compartmentalization

A Pack may be divided into compartments with different claims, sensitivity, recipients, or decryption keys.

Compartmentalization may support:

- least-privilege disclosure;
- multi-party review;
- protected identity material;
- regulator-only or auditor-only evidence;
- staged disclosure;
- jurisdiction-specific handling;
- separation of canonical and presentation layers.

The top-level Manifest or governed manifest graph must identify compartment relationships, required status, integrity commitments, disclosure state, and cross-compartment dependencies.

A recipient lacking one compartment must be able to determine which checks and Completeness claims are affected without learning protected contents unnecessarily.

---

# 13.41 Size, Chunking, and Streaming

Large Packs may require chunking, streaming, multipart delivery, or tiered retrieval.

The binding must preserve:

- entry identity;
- chunk order and membership;
- chunk and whole-object integrity;
- retry and resume semantics;
- duplicate handling;
- missing-chunk detection;
- encryption boundaries;
- decompression and expansion limits;
- deterministic reassembly;
- partial-validation behavior.

Partial transfer must not be reported as complete delivery.

Verification Engines should support bounded streaming validation so a malicious or very large Pack cannot force unsafe memory, disk, or compute consumption.

---

# 13.42 Deduplication and Content Addressing

Deduplication may reduce package size by storing one representation referenced from multiple claims or entries.

Content addressing may support stable integrity lookup when:

- the digest algorithm and canonical input are governed;
- namespace and encoding are explicit;
- collisions and algorithm migration are handled;
- access and disclosure rules remain enforceable;
- reference cycles and missing targets are detected.

Deduplication must preserve every semantic relationship and must not merge objects merely because selected payload bytes match.

Two entries with the same content may have different provenance, custody, object identity, disclosure status, or role.

---

# 13.43 Ordering and Relationship Graphs

Filesystem order is not evidence order.

The Manifest may represent:

- causal graphs;
- Chain order;
- object-to-history relationships;
- Authority and delegation graphs;
- identity-key history;
- correction and supersession;
- dependency graphs;
- package-version lineage;
- custody and delivery sequences.

Relationships must be typed and versioned.

Where order matters, it must derive from authenticated sequence evidence, timestamps with explicit semantics, Chain positions, Checkpoints, or another governed method.

Presentation timelines are derived views and must not silently replace the underlying ordering evidence.

---

# 13.44 Duplicates and Conflicts

A Pack may contain duplicate identifiers, alternate representations, or incompatible evidence.

The assembler and verifier must distinguish:

- identical duplicate bytes;
- canonical and derived forms;
- multiple valid representations;
- repeated references to one entry;
- conflicting protected content under one identifier;
- competing histories or claims;
- source and corrected versions;
- malicious ambiguity.

Conflicting evidence should be preserved when relevant to the focal claims.

The Pack must not silently select one favorable version, and a Verification Engine must not use container order or filename sorting as an undeclared conflict-resolution rule.

---

# 13.45 Prior Verification Reports

A Pack may include earlier Verification Reports to document prior evaluations.

Each report must remain bound to:

- evidence evaluated;
- Verification Context;
- Verification Time;
- verifier identity and software where applicable;
- checks performed;
- dependencies available at that time;
- outcomes, warnings, and limitations.

A prior valid report is informative evidence. It does not replace fresh Verification when the current reviewer requires it.

Later evidence, different context, algorithm policy, or newly available dependencies may produce a different conclusion. The earlier report must not be rewritten to match the later result.

---

# 13.46 Preservation and Custody Evidence

A Pack may include Preservation Evidence concerning source retention, integrity checks, custody, recovery, migration, Legal Hold, and disposition state.

It may also include package-specific custody evidence for:

- assembly environment;
- source retrieval;
- temporary working copies;
- package creation;
- encryption;
- transfer;
- receipt;
- storage by the recipient;
- access and redistribution;
- deletion or return.

Preservation Evidence does not make invalid source evidence valid.

Custody continuity can strengthen handling claims, but a custody gap does not automatically prove content alteration when cryptographic integrity remains verifiable. The exact conclusion must remain bounded.

---

# 13.47 Historical Integrity and Key Evidence

A Pack intended for historical Verification should include or resolve the relevant:

- Chain Entries and Chain proofs;
- Commitment Receipts;
- Witness Observations;
- Witness Registry and quorum state;
- Checkpoints;
- Anchor Evidence;
- Key Transparency Records;
- Historical Key State;
- algorithm and Signature context;
- correction, revocation, migration, or renewal evidence.

Including an Evidence Record without its required historical-integrity and key dependencies may support object inspection but not the intended historical claim.

The Manifest must identify which layers are present and which remain absent or external.

---

# 13.48 Temporal Snapshot and Cutoff

A Dispute Pack represents material known, selected, or available at defined boundaries.

Relevant times may include:

- focal Event Time;
- historical Verification boundary;
- source retrieval time;
- package creation time;
- Manifest finalization time;
- delivery and receipt times;
- Verification Time;
- cutoff after which later evidence is excluded.

```text
Evidence existing at Event Time
≠
Evidence known at Packaging Time
≠
Evidence available at Verification Time
```

The Pack must not imply that no later evidence exists merely because the package has a fixed cutoff.

---

# 13.49 Deterministic Assembly and Reproducibility

Where a Trust Profile requires reproducible assembly, equivalent source state, selection rules, context, and packaging profile should produce equivalent logical Manifest contents and integrity commitments.

Determinism requires rules for:

- entry selection and sorting;
- identifier and path generation;
- canonicalization;
- timestamp treatment;
- compression metadata;
- encryption randomness and which layer is compared;
- duplicate handling;
- dependency traversal;
- derived representation generation;
- omission classification.

Bit-identical encrypted archives are not always required. The stable requirement is reproducible logical content and verifiable relationships under the defined binding.

---

# 13.50 Versioning, Correction, and Supersession

A Pack may require a new version when:

- evidence becomes available;
- an omission or error is corrected;
- a redaction is changed;
- a dependency is embedded or removed;
- a claim or Verification Context changes;
- encryption or recipient scope changes;
- a conflict is discovered;
- packaging or schema migration occurs.

The successor Manifest must identify its predecessor, reason for change, changed entries or semantics, and relationship to earlier Verification Reports.

```text
Pack v1
   │ append-only succession
   ▼
Pack v2
```

Earlier versions remain distinct historical packages and must not be silently overwritten.

---

# 13.51 Merge and Split Operations

Several Packs may be merged, or one Pack may be split into claim-, recipient-, size-, or jurisdiction-specific Packs.

The operation must preserve:

- source Pack identities and versions;
- source Manifest integrity;
- claim and context mappings;
- entry identity and provenance;
- dependency relationships;
- omission and redaction state;
- compartment and confidentiality rules;
- duplicate and conflict behavior;
- resulting Completeness limitations.

A merged Pack is not automatically more complete if source Packs share the same omissions. A split Pack must not claim the source's Completeness when required cross-part dependencies are unavailable.

---

# 13.52 Format and Protocol Migration

Dispute Packs may be migrated to a new container, Manifest version, canonicalization rule, encryption suite, or TAIP version.

Migration must bind:

- source and target Pack identifiers;
- source and target Manifest versions;
- transformation rules;
- entry and claim mappings;
- preserved, changed, and lost semantics;
- source and target integrity values;
- Authority and Policy;
- validation results;
- retained original package or Preservation reference.

A package conversion must not make unsupported source semantics appear valid in the target version.

The original Pack, Migration Record, and successor Pack remain distinguishable.

---

# 13.53 Validation Layers

Dispute Pack validation should distinguish:

- container safety and binding validity;
- DPM structure and schema;
- Manifest cryptographic protection;
- package and entry integrity;
- identifier and representation consistency;
- entry availability and decryptability;
- reference and dependency resolution;
- claim and Verification Context interpretation;
- omission, redaction, and Completeness evaluation;
- source Protocol Object validity;
- historical-integrity and key validation;
- Trust Profile achievement.

A Pack may be structurally valid while containing invalid evidence. It may have perfect entry integrity while being incomplete for the focal claim.

Each layer must report its own result.

---

# 13.54 Verification Engine Behavior

A conforming Verification Engine conceptually performs the following steps:

1. process the container using safe bounded rules;
2. locate and validate the DPM;
3. establish Pack identity, version, and cryptographic protection;
4. inventory declared, missing, and undeclared entries;
5. verify entry integrity and representation mappings;
6. establish focal claims and Verification Context;
7. resolve embedded and permitted external dependencies;
8. evaluate omissions, redactions, conflicts, and Completeness;
9. validate Protocol Objects, history, keys, Authority, Policy, and Trust Profile controls;
10. produce bounded outcomes and a reproducible Verification Report.

The engine must not treat assembler intent or a prior report as a substitute for performed checks.

---

# 13.55 Verification Outcomes and Reports

Pack evaluation should support distinct outcomes such as:

- `valid`;
- `invalid`;
- `incomplete`;
- `indeterminate`;
- `unsupported`;
- `unavailable`;
- `conflicting`;
- `valid-with-warnings` where defined by TAIP;
- `not-evaluated` for checks outside scope.

The Verification Report should identify:

- Pack and Manifest evaluated;
- Verification Context used;
- claims evaluated;
- entries and dependencies used;
- checks and individual results;
- omissions, redactions, conflicts, and unavailable material;
- Completeness state;
- Intended and Achieved Trust Profiles;
- verifier identity, software, and Verification Time where applicable.

One overall status must not hide material layer failures.

---

# 13.56 Independent Portability

A Pack is independently portable when an authorized party can transfer and evaluate its defined claims without relying upon undocumented producer databases, application code, privileged dashboards, or mutable operational state.

Portability may require:

- open or fully documented package bindings;
- stable identifiers and integrity values;
- embedded or durably resolvable dependencies;
- portable schemas and profiles;
- algorithm specifications;
- key and identity history;
- deterministic validation rules;
- safe reference implementation or test vectors;
- export and decryption procedures;
- Preservation across the Evidence Lifetime.

Portability is bounded by external dependencies and disclosure rights. It does not require public availability.

---

# 13.57 Privacy and Data Minimization

Dispute Packs can concentrate sensitive financial, commercial, personal, security, identity, and incident information into one portable artifact.

Assembly should apply:

- claim-bounded minimization;
- least-privilege disclosure;
- compartmentalization;
- encryption;
- scoped recipient binding;
- controlled derived views;
- explicit redaction;
- retention and disposition rules;
- query and access privacy;
- omission transparency.

Minimization must not be hidden as Completeness. If evidence required by the Trust Profile is excluded for privacy, the resulting Verification limitation must remain explicit.

Package metadata, claim labels, filenames, entry sizes, and relationship graphs may remain sensitive even when payloads are encrypted.

---

# 13.58 Audit, Regulatory, and Legal Boundaries

A Dispute Pack may support audit, regulatory, contractual, insurance, investigation, or legal processes.

TrustAgentAI can provide protocol evidence concerning:

- package identity and integrity;
- source and custody provenance;
- historical Verification;
- omissions and redactions;
- Completeness under a defined method;
- Preservation and delivery state;
- reproducible technical outcomes.

It does not determine:

- admissibility;
- privilege;
- discovery scope;
- burden of proof;
- legal ownership;
- regulatory compliance;
- final factual or legal judgment.

Those conclusions require applicable human, organizational, and legal Authority.

---

# 13.59 Delivery, Receipt, and Custody

Delivery Evidence may bind:

- Pack identifier, version, and digest;
- sender and recipient identities;
- delivery method;
- encryption and key-delivery relationship;
- dispatch and receipt times;
- accepted, rejected, partial, or failed status;
- confidentiality and handling acknowledgment;
- integrity verification at receipt;
- subsequent custody or transfer restrictions.

```text
Sent
≠
Delivered
≠
Received
≠
Accepted
≠
Verified
```

A transfer receipt does not prove package validity or claim success.

Material custody events should be preserved when required by the Trust Profile or applicable process.

---

# 13.60 Anti-Patterns and Relationship to Other Controls

The following are architectural anti-patterns:

## ZIP Equals Dispute Pack

Treating an arbitrary archive as conforming without a canonical Manifest and governed semantics.

## Manifest Equals Truth

Assuming that a signed inventory makes included assertions valid.

## Structural Validity Equals Completeness

Reporting a Pack as complete merely because every declared file is present.

## Completeness Without Scope

Using `complete` without identifying claims, population, boundary, context, and method.

## Favorable Evidence Selection

Including only material supporting one preferred conclusion while suppressing known counterevidence.

## Silent Omission

Removing referenced, required, or known relevant material without declaring the omission and effect.

## Redaction as Canonical Evidence

Presenting a redacted or derived representation as the complete original object.

## URL Equals Dependency

Treating a mutable or inaccessible locator as a resolved, versioned Verification Dependency.

## Screenshot Equals Protocol Object

Presenting a human-readable capture without preserving its relationship to canonical evidence and provenance.

## Filename Semantics

Inferring object type, order, identity, or Authority from an ungoverned path or filename.

## Prior Report Equals Fresh Verification

Treating an earlier conclusion under another context as a substitute for current checks.

## Encryption Equals Authorization

Assuming anyone able to decrypt is authorized for every use or redistribution.

## One Status Hides Failures

Collapsing container, integrity, evidence, dependency, Completeness, and profile results into an ambiguous Boolean.

## Pack Overwrite

Replacing an earlier package silently when evidence, redaction, claims, or context changes.

## Producer-Only Portability

Calling a Pack portable when mandatory verification still requires the producer's proprietary application or privileged database.

Dispute Packs compose with other controls:

```text
Protocol Objects          provide evidence semantics
Hash Chains               provide ordered historical binding
Witnesses                 provide independent observation
Checkpoints and Anchors   provide historical commitments
Key Transparency          provides Historical Key State
Preservation              maintains evidence and dependencies
Dispute Pack              creates a portable bounded package
Verification              evaluates claims under explicit context
```

A Dispute Pack packages evidence. It does not replace or recreate the controls that produced and preserved that evidence.

---

# Dispute Pack Invariants

### INV-DP-001 — Manifest Requirement

A conforming Dispute Pack MUST possess a canonical governed Dispute Pack Manifest.

### INV-DP-002 — Explicit Package Boundary

The Manifest MUST distinguish declared package members, external dependencies, omissions, redactions, unavailable material, and packaging-only metadata.

### INV-DP-003 — Claim-Bounded Scope

A Dispute Pack MUST identify the focal Accountability Claims or governed review scope it is intended to support.

### INV-DP-004 — Context Explicitness

The expected Verification Context MUST remain explicit and MUST NOT be inferred solely from the assembler's environment.

### INV-DP-005 — Package/Evidence Separation

Validity of a Pack or Manifest MUST NOT by itself establish validity of included evidence.

### INV-DP-006 — Package/History Separation

Inclusion in a Dispute Pack MUST NOT by itself establish historical Commitment, Witnessing, Checkpointing, anchoring, or Preservation.

### INV-DP-007 — Validity/Completeness Separation

Structural and cryptographic validity of a Pack MUST remain distinct from claim-bounded Completeness.

### INV-DP-008 — Bounded Completeness

Every Completeness claim MUST identify its claims, population, historical boundary, Verification Context, and method.

### INV-DP-009 — Canonical/Packaged Separation

Canonical Protocol Objects, packaged representations, derived views, and redacted representations MUST remain distinguishable.

### INV-DP-010 — Exact Entry Integrity

Every material included entry MUST possess an unambiguous identity and integrity expectation appropriate to its role.

### INV-DP-011 — Identifier/Locator Separation

Pack identifiers, object identifiers, digests, filenames, and network locators MUST remain distinct concepts.

### INV-DP-012 — Typed Relationships

Accountability-relevant relationships between entries MUST be typed and MUST NOT depend solely upon directory layout or filename convention.

### INV-DP-013 — Explicit External Dependencies

Mandatory external dependencies MUST identify version, integrity, resolver expectations, and their effect on portability.

### INV-DP-014 — Omission Visibility

Known omitted, unavailable, redacted, unsupported, or unresolved required material MUST remain explicit.

### INV-DP-015 — Absence-State Separation

Known omitted, unavailable, unresolved, not known to exist, and proven absent states MUST remain distinguishable.

### INV-DP-016 — Redaction Transparency

A redacted or selectively disclosed representation MUST NOT be represented as the complete canonical object.

### INV-DP-017 — Derivation Transparency

Derived representations MUST identify their source relationship, method, and material limitations.

### INV-DP-018 — Conflict Preservation

Relevant conflicting evidence MUST NOT be silently discarded or resolved through container order, filename, or assembler preference.

### INV-DP-019 — Selection Neutrality

Evidence selection MUST NOT be represented as comprehensive or neutral when known counterevidence within scope was excluded without disclosure.

### INV-DP-020 — Manifest Protection

Cryptographic protection of the DPM MUST cover claims, context, inventory, integrity references, dependencies, omissions, disclosure state, and version relationships.

### INV-DP-021 — Encryption/Authorization Separation

Decryption capability MUST NOT by itself establish Authority to use, redistribute, or interpret every Pack entry.

### INV-DP-022 — Confidentiality/Completeness Separation

Confidentiality restrictions MUST NOT silently convert missing material into a satisfied Completeness control.

### INV-DP-023 — Safe Container Processing

Physical package processing MUST be bounded and MUST NOT trust unsafe paths, special files, duplicate names, or unbounded expansion.

### INV-DP-024 — Temporal Boundary Visibility

Event, retrieval, packaging, cutoff, delivery, receipt, and Verification times MUST remain distinct when they affect interpretation.

### INV-DP-025 — Prior-Report Non-Substitution

A prior Verification Report MUST NOT substitute for current Verification unless the current procedure explicitly and validly relies upon it.

### INV-DP-026 — Version Non-Overwrite

Correction, expansion, redaction change, or context change MUST create a new Pack version rather than silently overwrite an earlier version.

### INV-DP-027 — Merge/Split Provenance

Merge and split operations MUST preserve source Pack identity, claim mapping, provenance, dependencies, disclosure state, and resulting limitations.

### INV-DP-028 — Migration Non-Rewrite

Package migration MUST preserve the source, target, transformation relationship, and lost or changed semantics.

### INV-DP-029 — Delivery/Verification Separation

Sending, delivery, receipt, acceptance, decryption, and Verification MUST remain distinct lifecycle states.

### INV-DP-030 — Portable Meaning

Portability MUST be evaluated against the actual dependencies, formats, keys, and producer access required for the defined verification path.

### INV-DP-031 — Explicit Verification Layers

Container, Manifest, integrity, evidence, dependency, Completeness, claim, and Trust Profile results MUST remain separately reportable.

### INV-DP-032 — Bounded Legal Meaning

Protocol validity of a Dispute Pack MUST NOT be represented automatically as admissibility, legal sufficiency, Regulatory Compliance, or business truth.

---

# Architectural Requirements

### REQ-DP-001

TAIP MUST define versioned semantics for Dispute Packs, Dispute Pack Manifests, entries, relationships, absence states, and Verification Outcomes.

### REQ-DP-002

Every conforming Pack MUST include exactly one authoritative top-level DPM or an unambiguous governed manifest graph defined by its binding.

### REQ-DP-003

The DPM MUST identify Pack identifier, version, Manifest version, assembler Protocol Identity, and applicable creation or packaging time semantics.

### REQ-DP-004

The DPM MUST identify focal Accountability Claims or a bounded governed review scope.

### REQ-DP-005

The DPM MUST identify the expected Verification Context or the governed method by which it is constructed.

### REQ-DP-006

The DPM MUST enumerate or deterministically commit to all declared package entries.

### REQ-DP-007

Each material entry MUST identify its semantic role, object or resource identity, version, representation, integrity value, and package locator where embedded.

### REQ-DP-008

Entry type and meaning MUST NOT be inferred solely from filename, path, media type, database name, or container order.

### REQ-DP-009

The DPM MUST distinguish canonical Protocol Objects, derived representations, redacted views, proofs, supporting resources, and packaging metadata.

### REQ-DP-010

The relationship between canonical content and compressed, encrypted, chunked, normalized, or otherwise packaged representations MUST be deterministic and verifiable.

### REQ-DP-011

Digest and commitment references MUST identify their algorithm, canonical input or representation, namespace, and comparison behavior.

### REQ-DP-012

DPM cryptographic protection MUST cover all properties whose alteration could change Pack identity, claims, context, membership, integrity, dependency, omission, redaction, handling, or version semantics.

### REQ-DP-013

Signer and countersigner roles MUST be typed, and Signature count MUST NOT substitute for role eligibility or Authority evaluation.

### REQ-DP-014

The Pack SHOULD identify the Authority and Policy governing assembly, source access, disclosure, redaction, encryption, and delivery.

### REQ-DP-015

Entry provenance SHOULD identify source, retrieval method and time, original integrity, performed transformations, and assembler-generated metadata.

### REQ-DP-016

Source-derived presentation material MUST NOT be represented as canonical evidence without a governed identity and integrity relationship.

### REQ-DP-017

Evidence-selection rules SHOULD be identifiable when they affect claim relevance, Completeness, or reproducibility.

### REQ-DP-018

Known relevant counterevidence within the declared scope MUST be included or explicitly classified as omitted, unavailable, redacted, or external.

### REQ-DP-019

Labels such as `minimal`, `comprehensive`, `full`, or `complete` MUST identify the bounded claims, population, selection rule, and limitations to which they apply.

### REQ-DP-020

Causal and historical closure MUST preserve typed relationships, detect cycles, handle missing references, and terminate under governed resource bounds.

### REQ-DP-021

The DPM MUST classify each mandatory Verification Dependency as embedded, external, omitted, redacted, unavailable, unresolved, or unsupported.

### REQ-DP-022

Mandatory external dependencies MUST identify stable identity, exact version, integrity expectation, semantic role, resolver requirements, and access limitations.

### REQ-DP-023

A Pack claiming offline Verification MUST state which checks and dependencies are fully supported without network access.

### REQ-DP-024

External resolution evidence SHOULD record requested identifier, resolver, returned version, integrity result, retrieval time, conflict state, and failure semantics.

### REQ-DP-025

Completeness evaluation MUST be bounded to defined claims, Verification Context, population, historical boundary, Trust Profile, and method.

### REQ-DP-026

TAIP MUST distinguish Complete, Incomplete, Indeterminate, Conflicting, Unsupported, and Not Evaluated completeness states where applicable.

### REQ-DP-027

Each known omission SHOULD identify affected item or category, claim relationship, required status, reason, availability, and expected Verification effect.

### REQ-DP-028

The Pack MUST distinguish known omitted, unavailable, unresolved, not known to exist, and proven absent material.

### REQ-DP-029

Unavailable or unresolved mandatory material MUST produce an explicit effect on Completeness and relevant Verification Outcomes.

### REQ-DP-030

Redacted or selectively disclosed entries MUST identify their canonical source or commitment, transformation or proof method, Disclosure Authority, and effect on Verification.

### REQ-DP-031

Derived representations SHOULD identify source entries, derivation method and version, generator, generation time, integrity value, and known information loss.

### REQ-DP-032

Confidentiality and handling metadata affecting access, redistribution, retention, or custody MUST be attributable and integrity-protected.

### REQ-DP-033

Encrypted entries MUST identify encryption suite, key identifiers and versions, protected scope, authenticated metadata, and authorized decryption semantics.

### REQ-DP-034

Loss or unavailability of required decryption capability MUST produce an explicit unavailable, incomplete, or indeterminate result.

### REQ-DP-035

Compartmentalized Packs MUST identify compartment integrity, recipient or role scope, dependencies, required status, and the effect of inaccessible compartments.

### REQ-DP-036

Physical package bindings MUST define safe path handling, duplicate behavior, special-file policy, compression limits, Manifest discovery, and deterministic extraction semantics.

### REQ-DP-037

Chunked or streaming entries MUST preserve chunk identity, order, membership, integrity, encryption boundaries, reassembly, and partial-transfer state.

### REQ-DP-038

Verification implementations MUST enforce governed bounds on archive expansion, entry count, path length, nesting, dependency depth, cryptographic cost, and total resource use.

### REQ-DP-039

Deduplication MUST preserve distinct object identity, provenance, semantic role, disclosure status, and relationship context.

### REQ-DP-040

Content-addressed references MUST define algorithm agility, collision behavior, namespace, and missing-target handling.

### REQ-DP-041

Accountability-relevant entry relationships MUST use governed typed semantics rather than implicit filesystem adjacency.

### REQ-DP-042

Ordering claims MUST derive from authenticated sequence, Chain, time, Checkpoint, or another governed ordering mechanism.

### REQ-DP-043

Duplicate identifiers with conflicting protected content MUST be surfaced as conflicts and MUST NOT be resolved by container order or filename.

### REQ-DP-044

Prior Verification Reports MUST retain the evidence, context, time, verifier, checks, dependencies, outcomes, and limitations to which they applied.

### REQ-DP-045

Inclusion of a prior Verification Report MUST NOT suppress current re-evaluation requirements defined by the applicable Trust Profile.

### REQ-DP-046

Preservation and custody evidence included in a Pack MUST identify its exact target, scope, time, and claim semantics.

### REQ-DP-047

Packs intended for historical Signature Verification MUST include or resolve applicable Historical Key State, Key Purpose, and algorithm context.

### REQ-DP-048

Packs claiming historical Commitment, Witnessing, Checkpointing, or anchoring MUST include or resolve the corresponding governed evidence and proof dependencies.

### REQ-DP-049

The DPM MUST distinguish Event Time, source retrieval time, packaging time, cutoff, delivery time, receipt time, and Verification Time where differences affect interpretation.

### REQ-DP-050

A fixed package cutoff MUST NOT be represented as proof that no later or external evidence exists.

### REQ-DP-051

Profiles requiring reproducible assembly MUST define deterministic logical selection, ordering, identifier, dependency, duplicate, and metadata rules.

### REQ-DP-052

A corrected, expanded, differently redacted, differently encrypted, or contextually changed Pack MUST receive a new version or identity according to governed succession rules.

### REQ-DP-053

Successor Packs MUST identify predecessors, reasons for change, changed entries or semantics, and effects on prior Verification Reports.

### REQ-DP-054

Merge and split operations MUST preserve source Pack identities, Manifest integrity, claim mapping, provenance, dependencies, disclosure state, and resulting Completeness limitations.

### REQ-DP-055

Package migration MUST bind source, target, transformation rules, claim and entry mappings, changed or lost semantics, integrity values, and validation results.

### REQ-DP-056

Validation MUST distinguish container safety, Manifest structure, cryptographic protection, package integrity, entry integrity, dependency resolution, evidence validity, Completeness, claim, and profile results.

### REQ-DP-057

Verification Engines MUST inventory missing declared entries and MUST distinguish them from undeclared extra files.

### REQ-DP-058

Verification Reports MUST identify the Pack, DPM, context, claims, entries, dependencies, checks, omissions, redactions, conflicts, Completeness, and Trust Profile outcomes evaluated.

### REQ-DP-059

One overall Verification status MUST NOT conceal material failures or unevaluated mandatory layers.

### REQ-DP-060

Portability claims MUST disclose mandatory reliance upon proprietary formats, producer services, privileged databases, external resolvers, or separately delivered keys.

### REQ-DP-061

Delivery and receipt evidence MUST distinguish dispatch, transfer, receipt, acceptance, integrity checking, decryption, and Verification states.

### REQ-DP-062

Unsupported mandatory Manifest semantics, entry types, algorithms, bindings, dependencies, or relationships MUST produce an explicit non-success Verification Outcome.

---

# Security Considerations

Dispute Packs cross trust boundaries and concentrate evidence, dependencies, credentials, and sensitive context into portable artifacts.

## Evidence Cherry-Picking

An assembler may include only evidence favorable to one party. Claim-bounded selection rules, known-counterevidence disclosure, causal closure, Completeness evaluation, and independent source comparison reduce this risk.

## Silent Omission

Required or referenced material may be removed while the remaining Pack still appears structurally valid. The DPM must declare required entries and absence states, and verification must report missing members explicitly.

## Manifest Forgery

An attacker may alter claims, entries, omissions, or handling rules. Canonicalization, cryptographic protection, Historical Key State, Authority evaluation, and protected version lineage are required.

## Entry Substitution

An attacker may replace a file while preserving its name or media type. Each material entry requires a governed identity and integrity value independent of its locator.

## Duplicate-Name Attack

Containers may permit duplicate paths interpreted differently by tools. Bindings must define duplicate behavior and verifiers should reject unsafe ambiguity.

## Path Traversal

Malicious paths may escape the extraction directory, overwrite files, or target system locations. Implementations must normalize paths, reject absolute and traversal paths, isolate extraction, and avoid following unsafe links.

## Special-File and Link Attack

Symbolic links, hard links, devices, sockets, named pipes, and filesystem metadata can create unsafe behavior. Package bindings must prohibit or explicitly govern them, and default processing should reject them.

## Decompression Bomb

Small compressed inputs may expand to extreme size. Verifiers must enforce limits on compressed and expanded size, ratios, nesting, entry count, and processing time.

## Parser Differential

Two implementations may interpret paths, numbers, Unicode, duplicate fields, timestamps, or container metadata differently. Strict schemas, canonicalization, test vectors, and rejection of ambiguous encodings reduce differential attacks.

## Dependency Confusion

A mutable URL or resolver may return a different schema, key, or Policy version. External dependencies require stable identity, exact version, integrity, and resolver context.

## Namespace Collision

Matching local IDs, filenames, or digests under different algorithms may be merged incorrectly. Every identifier and digest must retain namespace, type, version, and algorithm context.

## Derived-View Manipulation

Reports, screenshots, timelines, and summaries may omit or reorder material. Their source mapping and derivation method must remain explicit, and canonical evidence should remain available when required.

## Redaction Failure

Redacted content may remain in metadata, previous versions, thumbnails, search indexes, filenames, compression dictionaries, or derived files. Redaction workflows should test the entire package and preserve accountable transformation evidence.

## Commitment Guessing

Digests of low-entropy confidential values can permit dictionary attacks. Salted or blinded commitments, access control, or another privacy-preserving construction may be required.

## Encryption Misbinding

Ciphertext may be moved between entries or recipients if identity and context are not authenticated. Encryption should bind Pack, entry, compartment, version, and recipient context as associated data where appropriate.

## Key Leakage

Including plaintext keys, passwords, recovery secrets, or unprotected key files inside the same Pack defeats confidentiality. Key delivery must follow a governed, separated method.

## Recipient Confusion

A Pack encrypted for one recipient may be forwarded or decrypted in an unintended role. Recipient and purpose binding, handling metadata, access evidence, and compartment keys reduce misuse.

## Signature Wrapping

An attacker may cause a valid Signature to appear to protect a different Manifest or entry. Signature coverage, object type, identifier, version, and canonical input must be explicit and validated.

## Stale Pack Replay

An older valid Pack may be presented after corrections or new evidence exist. Version lineage, cutoff time, status registries where applicable, and comparison with successor Packs help detect staleness.

## Malicious External Resolver

A resolver may omit, fork, or substitute dependencies. Integrity validation, multiple sources, preserved copies, Checkpoints, and explicit resolver trust constrain the attack.

## Resource-Exhaustion Graphs

Circular, deeply nested, or excessively broad dependency graphs can exhaust processing. Verifiers must bound traversal, detect cycles, cache safely, and return partial non-success outcomes rather than hang.

## Verification Engine Exploitation

Packs may contain malicious files targeting parsers, renderers, codecs, or cryptographic libraries. Processing should use isolation, least privilege, content-type validation, sandboxing, patch management, and safe rendering.

## Active Content

HTML, documents, spreadsheets, scripts, or media may contain macros, links, or executable content. A Pack must not require unsafe execution for protocol Verification. Rendered review should occur in controlled environments.

## Conflict Suppression

An assembler may place conflicting evidence in an undeclared directory or use order to favor one version. Verifiers must inventory undeclared entries, enforce Manifest membership, and surface relevant conflicts.

## False Offline Claim

A Pack may appear self-contained while silently fetching current keys, schemas, or revocation state. Offline profiles must enumerate permitted local dependencies and prohibit undeclared network resolution.

## Custody Substitution

A transfer receipt may reference a filename rather than the exact Pack digest. Delivery evidence must bind Pack identity, version, Manifest commitment, sender, and recipient.

## Prior-Report Laundering

An earlier favorable Verification Report may be included to discourage fresh evaluation even though context or evidence changed. Reports must retain their boundaries and must not override current profile requirements.

## Metadata Leakage

Even encrypted payloads can leak claim types, parties, entry sizes, timestamps, graph shape, or incident severity. Envelope and compartment design should minimize visible metadata.

## Temporary Working Copies

Assembly, extraction, rendering, and Verification may leave sensitive copies on disk, in memory, caches, logs, or backups. Workflows require controlled environments and accountable cleanup or retention.

---

# Privacy Considerations

Dispute Packs create exceptional privacy risk because they combine evidence that may be less sensitive when stored separately.

## Claim-Bounded Minimization

The assembler should include only material required to support, contradict, qualify, or interpret the focal claims under the stated context. Convenience is not sufficient reason to export unrelated records.

## Aggregation Risk

A Pack can reveal relationships among identities, keys, transactions, policies, devices, Witnesses, and incidents. Privacy assessment must consider the combined graph, not only each entry individually.

## Sensitive Manifest Data

The DPM may reveal parties, allegations, categories of omitted evidence, privilege assertions, incident timing, and investigation scope. A minimal outer Manifest and encrypted inner compartments may be appropriate.

## Data Subject and Third-Party Material

Evidence about one Accountable Action may contain information about customers, employees, counterparties, or unrelated third parties. Redaction, pseudonymization, selective disclosure, and access restriction should preserve only necessary semantics.

## Stable Identifiers

PIDs, KIDs, object IDs, and digests can enable cross-Pack correlation. Scoped identifiers or protected mappings may reduce linkage when global correlation is not required.

## Timing and Size Leakage

Packaging, delivery, entry counts, and ciphertext sizes can reveal activity or incident scale. Padding, batching, delayed delivery, or compartment separation may reduce unnecessary leakage.

## Redaction Transparency

The Pack must expose that redaction occurred and its Verification effect without necessarily revealing the protected value or full reason.

## Derived Human-Readable Views

Timelines and summaries can expose more information than the minimum machine-verifiable evidence. Presentation layers should be independently classified and access-controlled.

## Query and Resolver Privacy

External dependency retrieval can reveal which identity, claim, or historical period is under review. Offline dependencies, local mirrors, private retrieval, or batching may reduce query leakage.

## Recipient Limitation

Encryption should target the least set of recipients and purposes required. Shared passwords and broad group keys make revocation, accountability, and least privilege difficult.

## Retention After Review

Portable copies can persist beyond the source retention period. Delivery policy should define recipient retention, Legal Hold, return, deletion, erasure, and evidence of disposition.

## Version Proliferation

Corrected, differently redacted, merged, and split Packs create multiple sensitive copies. Version inventories and disposition rules should account for every successor and derivative.

## Verification Telemetry

Verification Reports, error logs, extracted caches, and analyst notes may reveal Pack contents. These outputs require their own minimization, classification, and Preservation policy.

## Confidentiality Versus Completeness

Some required evidence may be lawfully unavailable to a recipient. The architecture should support restricted compartments and explicit indeterminate outcomes rather than forcing overdisclosure or claiming false Completeness.

TrustAgentAI defines evidence semantics. Applicable privacy, privilege, discovery, and disclosure law remains outside protocol determination.

---

# Design Rationale

## Why the Manifest Is Canonical

Without a governed DPM, the verifier cannot reliably distinguish evidence from convenience files, know what should be present, identify omissions, or reproduce the intended context. The Manifest makes the package boundary accountable.

## Why Claims Come Before Contents

Evidence is sufficient or incomplete only relative to a question. Focal Accountability Claims prevent `all relevant evidence` from becoming an undefined and unverifiable promise.

## Why Verification Context Is Packaged

The same bytes may produce different valid conclusions under different profile, policy, algorithm, time, or resolver conditions. Explicit context makes those differences explainable.

## Why Completeness Is Bounded

No finite package can be complete for every future question. Binding Completeness to claims, population, method, boundary, and context supports meaningful evaluation without universal claims.

## Why Omissions Are First-Class

Absence often determines the result of a dispute. Explicit omission semantics distinguish withheld, lost, unavailable, unknown, and proven-absent evidence and prevent silence from becoming success.

## Why Redacted Views Are Separate

Redaction can preserve confidentiality, but a changed representation may no longer support all checks. Keeping canonical and disclosed views distinct protects both privacy and honest Verification.

## Why the Pack Does Not Create History

Packaging occurs after evidence creation and may be controlled by a dispute party. Historical Commitment, Witnessing, Checkpoints, Anchors, and Key Transparency must therefore be proven by their original evidence, not inferred from inclusion.

## Why External Dependencies Are Allowed

Embedding every specification or public registry can be wasteful. Stable, versioned, integrity-protected external references preserve efficiency, while the Manifest makes their portability cost explicit.

## Why Offline Verification Is a Profile

Some reviews require resilience, privacy, or operation after provider failure. Offline packaging supports those needs, but it is a bounded capability rather than an assumption for every Pack.

## Why Derived Views Are Supported

Human reviewers benefit from timelines, tables, translations, and summaries. Treating them as derived entries preserves usability without confusing presentation with canonical evidence.

## Why Updates Create New Versions

A corrected or expanded Pack changes the evidence boundary seen by a verifier. Versioned succession preserves what each recipient received and why earlier conclusions differed.

## Why Safe Packaging Is Normative

Portable archives are untrusted input crossing system boundaries. Path traversal, parser differences, active content, and resource exhaustion can compromise the verifier before evidence is evaluated.

## Why Prior Reports Are Informative Only

A Verification Report records a result under its own evidence and context. Including it preserves history, but reproducibility requires the current verifier to know which checks were actually performed now.

## Why Delivery States Are Separate

Sending a Pack does not prove receipt, receipt does not prove integrity, and acceptance does not prove the claims. Separate states prevent logistics from becoming evidence validation.

## Why Portability Is More Than File Transfer

True portability requires semantics, dependencies, keys, historical context, safe bindings, and independent tools—not merely the ability to copy an archive.

---

# Summary

A Dispute Pack is a portable, governed evidence package designed for independent evaluation outside the originating production environment.

Its canonical Dispute Pack Manifest identifies:

- Pack identity and version;
- focal Accountability Claims;
- expected Verification Context;
- included canonical and derived entries;
- integrity references and typed relationships;
- embedded and external dependencies;
- omissions, redactions, unavailable material, and conflicts;
- confidentiality and handling semantics;
- version, custody, delivery, and Preservation relationships.

The central distinctions are:

```text
Valid Pack
≠
Valid Evidence
≠
Complete Evidence
≠
Successful Accountability Claim
```

and:

```text
Complete for Claim C under Context V
≠
Complete for every possible question
```

Dispute Packs package evidence; they do not recreate historical Commitment, Witnessing, Checkpoints, External Anchors, Historical Key State, or Preservation. Those properties must remain supported by their own Protocol Objects and dependencies.

Conforming Packs make omissions and disclosure limits visible, preserve canonical and derived representations separately, support safe bounded processing, and carry enough historical context for reproducible Verification. Updates create new versions, external dependencies remain explicit, and delivery is distinct from acceptance and Verification.

The stable objective is a package that another authorized party can independently inspect, verify, challenge, preserve, and explain without trusting an undocumented production narrative.
