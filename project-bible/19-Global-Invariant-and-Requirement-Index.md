# Chapter 19 — Global Invariant and Requirement Index

> **Chapter-local rules explain where an obligation comes from; global identifiers explain which durable system property the obligation protects and how that property is traced into TAIP, Trust Profiles, Verification, and conformance.**

## Purpose

This chapter defines the canonical **Global Invariant and Requirement Index** for the TrustAgentAI Project Bible.

It consolidates recurring architectural properties into stable `GINV-*` identifiers and cross-domain implementation obligations into stable `GREQ-*` identifiers. It provides a common traceability layer across:

- Project Bible chapters and service documents;
- chapter-local `INV-*` and `REQ-*` identifiers;
- TAIP Core and normative modules;
- Trust Profiles and control mappings;
- schemas, Registries, canonicalization rules, extensions, and bindings;
- Verification Contexts, checks, outcomes, and reports;
- conformance assertions, test vectors, suites, and reports;
- governance, versioning, compatibility, deprecation, migration, and preservation evidence.

This chapter does not replace the detailed chapters.

A global entry summarizes and connects obligations. The cited chapter remains authoritative for its complete scope, definitions, exceptions, failure semantics, security and privacy considerations, and rationale.

This chapter establishes:

- 36 globally applicable invariants, `GINV-001` through `GINV-036`;
- 115 global requirements, `GREQ-001` through `GREQ-115`;
- bidirectional invariant-to-requirement traceability;
- local-to-global mapping relationship types;
- requirement ownership and lifecycle metadata;
- Trust Profile and TAIP allocation matrices;
- Verification Evidence and conformance coverage models;
- identifier stability and index-governance rules.

The index covers the architecture defined in [01-Philosophy.md](01-Philosophy.md) through [18-Governance-Versioning-and-Compatibility.md](18-Governance-Versioning-and-Compatibility.md), together with the identifier conventions in [Acronyms.md](Acronyms.md), canonical terms in [Terminology.md](Terminology.md), and document roles in [Document-Status.md](Document-Status.md).

The current indexed source set contains **483 chapter- or service-local invariants** and **796 chapter- or service-local requirements**. These source identifiers remain stable and meaningful in their own right. Global identifiers consolidate cross-cutting meaning without renumbering the source material.

---

## How to Read This Index

Each global invariant entry includes:

- a stable identifier and title;
- the property that must remain true;
- global requirements that implement or protect it;
- representative chapter-local invariant sources.

Each global requirement entry includes:

- a stable identifier and title;
- a normative architectural obligation;
- one or more global invariants it protects;
- representative chapter-local requirement mappings.

The mappings are intentionally many-to-many.

```text
Chapter-Local Invariants and Requirements
                 │
                 ▼
        GINV-* and GREQ-* Catalog
                 │
        ┌────────┼────────┐
        ▼        ▼        ▼
      TAIP   Trust Profiles   Governance
        │        │        │
        └────────┼────────┘
                 ▼
      Verification and Conformance Evidence
```

The global catalog is normative at the architectural level. Concrete field, encoding, algorithm, endpoint, and test details belong in the applicable TAIP and conformance artifacts.

---

## Identifier and Mapping Semantics

### Stable Identifiers

Once published, a `GINV-*` or `GREQ-*` identifier must not be reassigned to incompatible meaning.

An entry may be clarified, deprecated, split, or superseded only through governed, versioned change that preserves the earlier entry and its historical mappings.

### Local Identifiers

Chapter-local identifiers retain their source authority.

Examples include:

```text
INV-ER-011
REQ-CHAIN-024
INV-VER-020
REQ-GOV-061
```

A local identifier may map to one or more global identifiers when its detailed rule protects several system-wide properties.

### Mapping Relationship Types

The index uses these relationship types:

- **Equivalent** — the local and global entries express the same obligation for the same applicable scope.
- **Refines** — the local entry adds narrower role, object, lifecycle, profile, binding, or failure semantics to the global requirement.
- **Supports** — the local entry contributes a control or condition needed to satisfy the global requirement without fully expressing it.
- **Partially Overlaps** — the entries share material scope, but each also contains obligations outside the other.
- **Superseded By** — a historical entry is replaced for future use by another identified entry while remaining preserved for historical interpretation.

The relationship direction in a global requirement's **Primary mappings** field is from the cited local requirement to the global requirement.

### Representative and Exhaustive Mappings

The human-readable catalog lists representative primary mappings. A release-quality machine-readable index may expand these into an exhaustive record for every local identifier.

Omission from the representative list does not cancel a local obligation. An exhaustive mapping record must never infer that an unmapped local requirement is optional.

### Bidirectional Traceability

Every `GINV-*` entry maps to one or more `GREQ-*` entries. Every `GREQ-*` entry maps back to one or more `GINV-*` entries.

This relationship supports two review questions:

```text
Invariant → Which requirements keep this property true?
Requirement → Which durable properties justify this obligation?
```

---

# 19.1 Global Invariant Catalog

The following invariants are the durable cross-domain properties of TrustAgentAI.

## Evidence Identity and Integrity

### GINV-001 — Evidence Identity Stability

A Protocol Object's canonical identity MUST remain stable after finalization. Packaging, storage, export, migration, SDK processing, or transport conversion MUST NOT silently alter that identity.

**Implemented by:** GREQ-004–GREQ-008, GREQ-026, GREQ-067–GREQ-069, GREQ-094.

**Primary local invariant sources:** `INV-ER-003`, `INV-OBJ-003`, `INV-DP-009`, `INV-API-011`.

### GINV-002 — Immutable Committed Evidence

Evidence that has entered protected historical state MUST NOT be modified in place. Correction, withdrawal, supersession, reassessment, or migration MUST create new attributable state.

**Implemented by:** GREQ-026, GREQ-028–GREQ-030, GREQ-034–GREQ-035, GREQ-065–GREQ-066, GREQ-089.

**Primary local invariant sources:** `INV-PHIL-002`, `INV-OBJ-012`, `INV-ER-015`, `INV-CHAIN-010`.

### GINV-003 — Canonical Representation Determinism

Equivalent protocol meaning under the same applicable rules MUST produce identical canonical cryptographic input across conforming implementations.

**Implemented by:** GREQ-007, GREQ-032–GREQ-034, GREQ-041, GREQ-093.

**Primary local invariant sources:** `INV-DESIGN-003`, `INV-OBJ-002`, `INV-ER-011`, `INV-CHAIN-003`.

### GINV-004 — Cryptographic Binding

Every Signature, digest, commitment, proof, Witness Observation, Checkpoint, Anchor, and package-integrity claim MUST bind to unambiguous protected input, algorithm context, purpose, and target scope.

**Implemented by:** GREQ-007–GREQ-009, GREQ-023–GREQ-025, GREQ-033, GREQ-038–GREQ-039, GREQ-043–GREQ-044, GREQ-050–GREQ-051, GREQ-053, GREQ-069, GREQ-094.

**Primary local invariant sources:** `INV-OBJ-007`, `INV-ER-012`, `INV-WIT-002`, `INV-CP-002`.

## Historical Accountability

### GINV-005 — Historical Continuity

TrustAgentAI MUST preserve an independently verifiable relationship among evidence, its ordered history, applicable identities and keys, governing profiles, normative dependencies, and later transitions.

**Implemented by:** GREQ-011, GREQ-018, GREQ-021, GREQ-028, GREQ-031–GREQ-033, GREQ-035–GREQ-039, GREQ-042, GREQ-052–GREQ-053, GREQ-055–GREQ-056, GREQ-114.

**Primary local invariant sources:** `INV-PHIL-004`, `INV-SYS-012`, `INV-CHAIN-016`, `INV-PRES-025`.

### GINV-006 — No Silent History Rewrite

Correction, supersession, revocation, renewal, migration, deletion, reassessment, and governance evolution MUST NOT make later state appear to have existed at an earlier historical boundary.

**Implemented by:** GREQ-021, GREQ-029–GREQ-030, GREQ-037, GREQ-042, GREQ-052, GREQ-057, GREQ-063, GREQ-065–GREQ-066, GREQ-071, GREQ-089, GREQ-112–GREQ-114.

**Primary local invariant sources:** `INV-PHIL-002`, `INV-PRES-023`, `INV-DP-028`, `INV-GOV-015`.

### GINV-007 — Historical Key Interpretation

Signature and cryptographic-action evaluation MUST use the identity, key, Key Purpose, Authority, algorithm policy, and status applicable to the relevant historical boundary.

**Implemented by:** GREQ-014, GREQ-018, GREQ-020–GREQ-021, GREQ-024, GREQ-055–GREQ-058, GREQ-075–GREQ-076.

**Primary local invariant sources:** `INV-PHIL-011`, `INV-KT-001`, `INV-KT-006`, `INV-VER-010`.

### GINV-008 — Independent Observation Integrity

A Witness Observation MUST remain attributable, target-bound, historically eligible, and distinct from producer assertion, replication, Signature validity, and proof of underlying truth.

**Implemented by:** GREQ-043–GREQ-045, GREQ-048–GREQ-049, GREQ-075, GREQ-077, GREQ-079.

**Primary local invariant sources:** `INV-WIT-001`, `INV-WIT-003`, `INV-WIT-005`, `INV-WIT-021`.

### GINV-009 — Checkpoint State Fidelity

A Checkpoint or External Anchor MUST preserve the exact scope, authority, target state, time context, lifecycle, finality, and proof boundary it claims without implying unsupported completeness or truth.

**Implemented by:** GREQ-050–GREQ-054, GREQ-075, GREQ-080.

**Primary local invariant sources:** `INV-CP-001`, `INV-CP-005`, `INV-CP-012`, `INV-CP-025`.

### GINV-010 — Preservation Without Mutation

Preservation MUST maintain evidence bytes or integrity-bound transformations together with the dependencies and history needed for future interpretation, without silently replacing original evidence.

**Implemented by:** GREQ-059–GREQ-066, GREQ-072, GREQ-114.

**Primary local invariant sources:** `INV-PRES-003`, `INV-PRES-009`, `INV-PRES-023`, `INV-PRES-031`.

## Verification and Assurance

### GINV-011 — Explicit Evidence Completeness

Included, omitted, missing, unavailable, withheld, redacted, conflicting, unsupported, and not-applicable evidence states MUST remain explicit and must affect conclusions according to governed rules.

**Implemented by:** GREQ-012, GREQ-025, GREQ-035, GREQ-040, GREQ-049, GREQ-057, GREQ-061, GREQ-067–GREQ-068, GREQ-070, GREQ-072, GREQ-078, GREQ-080, GREQ-100.

**Primary local invariant sources:** `INV-PROB-011`, `INV-DP-014`, `INV-VER-018`, `INV-VER-022`.

### GINV-012 — Verification Independence

Verification MUST be performable from portable evidence and governed dependencies without requiring trust in the producer, original operator, one hosted service, or one proprietary implementation.

**Implemented by:** GREQ-059–GREQ-060, GREQ-067–GREQ-069, GREQ-073–GREQ-076, GREQ-080, GREQ-099, GREQ-114.

**Primary local invariant sources:** `INV-PHIL-003`, `INV-SYS-015`, `INV-DP-030`, `INV-API-030`.

### GINV-013 — Deterministic Verification

Equivalent evidence evaluated under equivalent protocol, profile, policy, time, resolver, and dependency conditions MUST produce equivalent protocol conclusions.

**Implemented by:** GREQ-041, GREQ-048, GREQ-056, GREQ-058, GREQ-075–GREQ-080, GREQ-106.

**Primary local invariant sources:** `INV-DESIGN-016`, `INV-SYS-017`, `INV-VER-003`, `INV-TP-031`.

### GINV-014 — Business Truth Separation

Protocol Verification MUST remain distinct from determination of business truth, legal validity, regulatory compliance, policy wisdom, or real-world correctness beyond the evaluated evidence.

**Implemented by:** GREQ-001, GREQ-011, GREQ-016–GREQ-017, GREQ-039–GREQ-040, GREQ-054, GREQ-077, GREQ-080.

**Primary local invariant sources:** `INV-PHIL-006`, `INV-EXEC-011`, `INV-CP-018`, `INV-VER-006`.

### GINV-015 — Validity and Completeness Separation

Evidence validity MUST remain distinguishable from evidence completeness. Cryptographically valid evidence may be incomplete, and complete evidence may contain invalid elements.

**Implemented by:** GREQ-038, GREQ-040, GREQ-067, GREQ-070, GREQ-078, GREQ-090.

**Primary local invariant sources:** `INV-PHIL-012`, `INV-OBJ-017`, `INV-DP-007`, `INV-VER-004`.

### GINV-016 — Assurance Composition

A Trust Profile or assurance claim MAY be achieved only when its mandatory controls, dependencies, independence conditions, exceptions, and evaluation rules are satisfied by evidence.

**Implemented by:** GREQ-048, GREQ-079, GREQ-081–GREQ-086, GREQ-089–GREQ-091.

**Primary local invariant sources:** `INV-DESIGN-012`, `INV-TP-001`, `INV-TP-003`, `INV-TP-017`.

### GINV-017 — Intended and Achieved Separation

An Intended Trust Profile, requested assurance level, configured control, or product capability MUST remain distinct from the assurance actually achieved by particular evidence under a defined Verification Context.

**Implemented by:** GREQ-079, GREQ-081, GREQ-083–GREQ-084, GREQ-087.

**Primary local invariant sources:** `INV-EXEC-007`, `INV-ER-020`, `INV-VER-005`, `INV-TP-002`.

### GINV-018 — Explicit Downgrade

Failure to achieve intended protocol, cryptographic, profile, evidence, privacy, availability, or compatibility strength MUST remain visible and MUST NOT be converted into silent success.

**Implemented by:** GREQ-049, GREQ-054, GREQ-077, GREQ-079, GREQ-087, GREQ-097, GREQ-102, GREQ-111–GREQ-112.

**Primary local invariant sources:** `INV-PHIL-014`, `INV-VER-023`, `INV-TP-012`, `INV-GOV-011`.

## Identity, Authority, and Roles

### GINV-019 — Identity, Key, and Authority Separation

Protocol Identity, cryptographic key control, Key Purpose, delegated Authority, organizational role, and governance mandate MUST remain distinct and independently evaluable.

**Implemented by:** GREQ-013–GREQ-016, GREQ-022, GREQ-043, GREQ-045–GREQ-046, GREQ-051, GREQ-108.

**Primary local invariant sources:** `INV-SYS-005`, `INV-KT-002`, `INV-KT-004`, `INV-GOV-001`.

### GINV-020 — Signature and Authorization Separation

A valid Signature proves a defined cryptographic relationship to protected input; it MUST NOT by itself prove Authority, permission, policy compliance, independence, completeness, or external truth.

**Implemented by:** GREQ-009, GREQ-015–GREQ-017, GREQ-076.

**Primary local invariant sources:** `INV-EXEC-004`, `INV-DESIGN-006`, `INV-ER-008`, `INV-VER-009`.

### GINV-021 — Substantive Independence

Independence MUST be evaluated from control, incentives, failure domains, Authority, infrastructure, and collusion exposure rather than inferred from instance count, replication, signatures, or organizational labels.

**Implemented by:** GREQ-046–GREQ-048, GREQ-062, GREQ-086.

**Primary local invariant sources:** `INV-PHIL-013`, `INV-DESIGN-011`, `INV-WIT-021`, `INV-TP-018`.

## State, Time, and Portability

### GINV-022 — Operational and Evidence State Separation

Operational logs, database state, API responses, cache state, notifications, and observability data MUST NOT be treated as Accountability Evidence unless they satisfy the applicable evidence semantics.

**Implemented by:** GREQ-001, GREQ-003, GREQ-027–GREQ-028, GREQ-092, GREQ-094–GREQ-096, GREQ-100.

**Primary local invariant sources:** `INV-EXEC-002`, `INV-SYS-001`, `INV-API-023`, `INV-API-024`.

### GINV-023 — Lifecycle State Distinction

Draft, finalized, signed, submitted, accepted, committed, witnessed, checkpointed, anchored, preserved, verified, corrected, superseded, revoked, deprecated, and withdrawn states MUST remain semantically distinct.

**Implemented by:** GREQ-019, GREQ-026–GREQ-031, GREQ-037, GREQ-043, GREQ-045, GREQ-050, GREQ-063, GREQ-088, GREQ-095, GREQ-100, GREQ-112.

**Primary local invariant sources:** `INV-DESIGN-010`, `INV-OBJ-011`, `INV-ER-014`, `INV-GOV-014`.

### GINV-024 — Explicit Time Semantics

Event time, creation time, issuance time, observation time, commitment time, Checkpoint time, Anchor time, Verification Time, and policy-effective time MUST remain distinguishable where they affect interpretation.

**Implemented by:** GREQ-010, GREQ-019, GREQ-036, GREQ-043, GREQ-050, GREQ-075.

**Primary local invariant sources:** `INV-ER-013`, `INV-CHAIN-015`, `INV-WIT-012`, `INV-VER-011`.

### GINV-025 — Portable Verifiability

Evidence, dependency manifests, Dispute Packs, and Verification results MUST preserve protocol meaning across storage systems, transports, implementations, operators, and offline boundaries.

**Implemented by:** GREQ-042, GREQ-059–GREQ-060, GREQ-062, GREQ-067–GREQ-069, GREQ-072–GREQ-074, GREQ-080, GREQ-093, GREQ-099, GREQ-101–GREQ-102, GREQ-114.

**Primary local invariant sources:** `INV-PHIL-009`, `INV-PRES-031`, `INV-DP-030`, `INV-API-030`.

### GINV-026 — Dependency Preservation

Every dependency required to reproduce interpretation MUST be identifiable, integrity-verifiable, versioned, resolvable or embedded, and historically preservable.

**Implemented by:** GREQ-012, GREQ-055, GREQ-059–GREQ-060, GREQ-072, GREQ-075, GREQ-083, GREQ-088, GREQ-099, GREQ-109–GREQ-110, GREQ-114.

**Primary local invariant sources:** `INV-SYS-013`, `INV-OBJ-015`, `INV-PRES-004`, `INV-MAP-029`.

## Privacy, Evolution, and Specification Integrity

### GINV-027 — Bounded Disclosure

Redaction, selective disclosure, encryption, access control, and derived representations MUST preserve visible boundaries, integrity relationships, and the effect of unavailable information on conclusions.

**Implemented by:** GREQ-008, GREQ-064, GREQ-070–GREQ-072, GREQ-091, GREQ-102.

**Primary local invariant sources:** `INV-ER-023`, `INV-DP-016`, `INV-VER-031`, `INV-PRES-018`.

### GINV-028 — Privacy-Proportional Evidence

TrustAgentAI MUST minimize unnecessary sensitive content while retaining enough structured evidence, commitments, provenance, and disclosure state for the applicable Accountability Claim and Trust Profile.

**Implemented by:** GREQ-002–GREQ-003, GREQ-059, GREQ-064, GREQ-071, GREQ-091, GREQ-102.

**Primary local invariant sources:** `INV-PROB-016`, `INV-DESIGN-017`, `INV-ER-022`, `INV-TP-029`.

### GINV-029 — Cryptographic Agility

Algorithms, suites, parameters, keys, and cryptographic policies MUST be explicitly identifiable, governable, deprecatable, replaceable, and historically interpretable without rewriting original evidence.

**Implemented by:** GREQ-020, GREQ-023–GREQ-024, GREQ-065–GREQ-066, GREQ-089, GREQ-110, GREQ-112.

**Primary local invariant sources:** `INV-DESIGN-018`, `INV-KT-028`, `INV-PRES-024`, `INV-GOV-016`.

### GINV-030 — Extension and Negotiation Safety

Extensions, version negotiation, capability discovery, and compatibility handling MUST NOT silently redefine Core meaning or select unsupported or weaker mandatory semantics.

**Implemented by:** GREQ-004, GREQ-023, GREQ-097–GREQ-098, GREQ-110–GREQ-111.

**Primary local invariant sources:** `INV-OBJ-016`, `INV-API-018`, `INV-API-027`, `INV-GOV-013`.

### GINV-031 — Implementation Neutrality

Protocol meaning MUST remain derivable from governed normative artifacts and MUST NOT depend upon one product, SDK, hosted service, programming language, storage engine, or reference implementation.

**Implemented by:** GREQ-003, GREQ-007, GREQ-073–GREQ-074, GREQ-092–GREQ-093, GREQ-101, GREQ-103–GREQ-104.

**Primary local invariant sources:** `INV-PHIL-008`, `INV-DESIGN-019`, `INV-API-032`, `INV-MAP-012`.

### GINV-032 — Normative Traceability

Every material architectural obligation MUST have an explicit normative disposition, stable identifier, responsible owner, test or rationale, lifecycle state, and bidirectional traceability.

**Implemented by:** GREQ-103–GREQ-107, GREQ-109, GREQ-115.

**Primary local invariant sources:** `INV-MAP-013`, `INV-MAP-014`, `INV-MAP-016`, `INV-MAP-031`.

### GINV-033 — Versioned Historical Meaning

Protocol, object, schema, canonicalization, algorithm, profile, Registry, extension, binding, package, test, and governance versions MUST remain identifiable and historically available where they affect meaning.

**Implemented by:** GREQ-004, GREQ-018, GREQ-030, GREQ-042, GREQ-055, GREQ-059, GREQ-081, GREQ-083, GREQ-085, GREQ-089, GREQ-109–GREQ-110, GREQ-112, GREQ-114.

**Primary local invariant sources:** `INV-OBJ-006`, `INV-MAP-019`, `INV-GOV-007`, `INV-GOV-031`.

### GINV-034 — Accountable Governance

Material specification, Registry, profile, test, compatibility, security, and governance decisions MUST be attributable to an identified Authority acting through a bounded, reviewable, preserved process.

**Implemented by:** GREQ-016, GREQ-022, GREQ-030, GREQ-088, GREQ-104–GREQ-105, GREQ-108–GREQ-109, GREQ-113, GREQ-115.

**Primary local invariant sources:** `INV-DESIGN-001`, `INV-TP-027`, `INV-MAP-027`, `INV-GOV-019`.

### GINV-035 — Explicit Conformance Scope

Conformance claims MUST identify the exact role, versions, profiles, features, extensions, bindings, suite, environment, exclusions, evidence, and evaluation time covered.

**Implemented by:** GREQ-090, GREQ-098, GREQ-106–GREQ-107, GREQ-115.

**Primary local invariant sources:** `INV-TP-026`, `INV-API-026`, `INV-MAP-018`, `INV-GOV-028`.

### GINV-036 — No Silent Semantic Degradation

Unsupported semantics, partial evidence, degraded operation, migration loss, version mismatch, conflicts, unavailable dependencies, and governance transitions MUST remain visible and MUST NOT be collapsed into an unqualified successful conclusion.

**Implemented by:** GREQ-025, GREQ-027, GREQ-029–GREQ-030, GREQ-034–GREQ-035, GREQ-037, GREQ-049, GREQ-052, GREQ-054, GREQ-057, GREQ-061, GREQ-063–GREQ-064, GREQ-066, GREQ-070, GREQ-077, GREQ-080, GREQ-082, GREQ-087, GREQ-094–GREQ-097, GREQ-100, GREQ-102, GREQ-111–GREQ-113, GREQ-115.

**Primary local invariant sources:** `INV-EXEC-012`, `INV-VER-019`, `INV-TP-030`, `INV-GOV-012`.

---

# 19.2 Global Requirement Catalog

The Global Requirement Catalog consolidates recurring architectural obligations into cross-domain identifiers. Each requirement is normative at the architectural level and must be allocated to one or more concrete artifacts before a TAIP conformance claim can depend upon it.

## Evidence Foundations

### GREQ-001 — Accountable Action Evidence

Accountability-critical actions MUST create or bind to structured Accountability Evidence at a defined boundary and sufficiently near the accountable event for the applicable Trust Profile.

**Protects:** GINV-014, GINV-022.

**Primary mappings:** Equivalent — `REQ-PHIL-001`, `REQ-EXEC-001`; Refines — `REQ-PROB-001`, `REQ-SYS-001`.

### GREQ-002 — Evidence Sufficiency and Minimization

Evidence MUST contain or reference the information necessary for the applicable Accountability Claim while minimizing personal, confidential, proprietary, and operational data not required by that claim or profile.

**Protects:** GINV-028.

**Primary mappings:** Equivalent — `REQ-PHIL-013`, `REQ-EXEC-015`; Refines — `REQ-ER-031`; Supports — `REQ-DESIGN-015`.

### GREQ-003 — Structured and Self-Describing Meaning

Protocol evidence MUST use governed structures and explicit semantics sufficient for independent implementations to identify its purpose, producer, roles, versions, lifecycle, and validation rules without proprietary context.

**Protects:** GINV-022, GINV-028, GINV-031.

**Primary mappings:** Refines — `REQ-OBJ-002`, `REQ-ER-002`; Supports — `REQ-SYS-002`, `REQ-OBJ-022`.

### GREQ-004 — Governed Type and Version

Every interoperable Protocol Object MUST identify or unambiguously bind to a governed type, namespace, object version, and applicable normative context.

**Protects:** GINV-001, GINV-030, GINV-033.

**Primary mappings:** Equivalent — `REQ-OBJ-001`, `REQ-OBJ-002`; Refines — `REQ-ER-001`, `REQ-PHIL-002`.

### GREQ-005 — Stable Object Identity

Every Protocol Object that requires durable reference MUST possess a stable identifier under defined namespace, generation, comparison, normalization, uniqueness, and collision rules.

**Protects:** GINV-001.

**Primary mappings:** Equivalent — `REQ-OBJ-005`, `REQ-ER-003`; Supports — `REQ-CHAIN-001`, `REQ-DP-006`.

### GREQ-006 — Identifier, Digest, and Locator Separation

Implementations MUST distinguish a protocol identifier from a cryptographic digest, storage key, database key, transport address, and network locator unless the applicable type explicitly defines a combined construction.

**Protects:** GINV-001.

**Primary mappings:** Equivalent — `REQ-OBJ-006`, `REQ-ER-004`; Refines — `REQ-DP-010`, `REQ-API-011`.

### GREQ-007 — Deterministic Canonicalization

Every Protocol Object participating in identifier derivation, digesting, signing, Commitment, or proof generation MUST define or reference deterministic canonical cryptographic input and domain separation.

**Protects:** GINV-001, GINV-003, GINV-004, GINV-031.

**Primary mappings:** Equivalent — `REQ-PHIL-003`, `REQ-OBJ-003`; Refines — `REQ-ER-013`, `REQ-CHAIN-007`.

### GREQ-008 — Protected-Scope Explicitness

The properties included in and excluded from every Signature, digest, commitment, proof, or integrity relationship MUST be explicit, and substitution-sensitive semantics MUST be protected where omission could change interpretation.

**Protects:** GINV-001, GINV-004, GINV-027.

**Primary mappings:** Equivalent — `REQ-OBJ-004`, `REQ-ER-014`; Refines — `REQ-WIT-015`, `REQ-CP-010`.

### GREQ-009 — Producer and Signer Attribution

Finalized evidence MUST identify or resolve the Evidence Producer, issuer, signer, and other accountable roles relevant to the bounded statement, together with the protected relationship each role asserts.

**Protects:** GINV-004, GINV-020.

**Primary mappings:** Equivalent — `REQ-ER-005`; Refines — `REQ-ER-006`, `REQ-OBJ-012`; Supports — `REQ-SYS-002`.

### GREQ-010 — Explicit Event and Time Semantics

Event types, event statuses, causal meaning, and time values MUST use governed semantics and MUST NOT imply stronger ordering, occurrence, or time assurance than supporting evidence establishes.

**Protects:** GINV-024.

**Primary mappings:** Refines — `REQ-ER-007`, `REQ-ER-012`; Supports — `REQ-PROB-006`, `REQ-OBJ-010`.

### GREQ-011 — Typed Causal and Accountability References

References connecting actors, actions, policies, evidence, predecessors, results, corrections, and derived artifacts MUST identify their relationship and preserve the identity and integrity strength required by the applicable claim.

**Protects:** GINV-005, GINV-014.

**Primary mappings:** Equivalent — `REQ-OBJ-007`; Refines — `REQ-ER-010`, `REQ-SYS-018`; Supports — `REQ-PROB-003`.

### GREQ-012 — Explicit Verification Dependencies

Every mandatory dependency required for interpretation or Verification MUST be identifiable by role, type, version, integrity expectation, and resolution status; missing or unresolved mandatory dependencies MUST remain explicit.

**Protects:** GINV-011, GINV-026.

**Primary mappings:** Equivalent — `REQ-OBJ-008`, `REQ-OBJ-009`; Refines — `REQ-ER-011`, `REQ-ER-025`; Supports — `REQ-DESIGN-013`.

## Identity, Authority, and Cryptography

### GREQ-013 — Protocol Identity

TAIP MUST define stable Protocol Identity semantics that remain distinct from keys, accounts, network locations, product tenants, and mutable display names.

**Protects:** GINV-019.

**Primary mappings:** Refines — `REQ-EXEC-004`, `REQ-DESIGN-004`; Supports — `REQ-KT-001`, `REQ-API-023`.

### GREQ-014 — Identity and Key Separation

Implementations MUST represent identity-to-key relationships explicitly and MUST support multiple historical keys, rotations, revocations, recoveries, and purpose assignments without equating identity with one key.

**Protects:** GINV-007, GINV-019.

**Primary mappings:** Equivalent — `REQ-PHIL-008`, `REQ-OBJ-011`; Refines — `REQ-KT-001–REQ-KT-006`.

### GREQ-015 — Key Purpose

Cryptographic keys and Signatures MUST identify or resolve the Key Purpose applicable to the protected action, and a valid key for one purpose MUST NOT be treated as authorized for another.

**Protects:** GINV-019, GINV-020.

**Primary mappings:** Refines — `REQ-ER-015`, `REQ-KT-007–REQ-KT-010`; Supports — `REQ-PROB-005`.

### GREQ-016 — Authority Evidence

Claims of Authority, delegation, approval, governance mandate, or permission MUST bind to attributable evidence identifying the subject, grantor, scope, limits, effective boundary, and applicable Policy.

**Protects:** GINV-014, GINV-019, GINV-020, GINV-034.

**Primary mappings:** Equivalent — `REQ-ER-008`, `REQ-SYS-011`; Refines — `REQ-GOV-001–REQ-GOV-005`.

### GREQ-017 — Signature and Authorization Evaluation

Signature validity MUST be evaluated and reported separately from Authority, Authorization, Policy compliance, Commitment, Completeness, independence, and external business outcome.

**Protects:** GINV-014, GINV-020.

**Primary mappings:** Equivalent — `REQ-ER-016`; Refines — `REQ-VER-015–REQ-VER-019`; Supports — `REQ-OBJ-017`.

### GREQ-018 — Historical Key State

Historical Signature Verification MUST resolve the key material, identity binding, Key Purpose, Authority, status, algorithm policy, and governing version applicable to the relevant historical boundary.

**Protects:** GINV-005, GINV-007, GINV-033.

**Primary mappings:** Equivalent — `REQ-EXEC-006`, `REQ-DESIGN-005`; Refines — `REQ-OBJ-011`, `REQ-KT-024–REQ-KT-031`.

### GREQ-019 — Key Lifecycle State

Registration, activation, suspension, rotation, recovery, revocation, expiry, compromise, supersession, and deletion states MUST remain typed, purpose-scoped, attributable, and historically ordered.

**Protects:** GINV-023, GINV-024.

**Primary mappings:** Refines — `REQ-KT-007–REQ-KT-023`; Supports — `REQ-PHIL-012`, `REQ-GOV-055`.

### GREQ-020 — Key Rotation Continuity

Key rotation MUST create an authorized transition that preserves prior key history and the ability to evaluate evidence created before, during, and after the transition.

**Protects:** GINV-007, GINV-029.

**Primary mappings:** Refines — `REQ-KT-012–REQ-KT-018`; Supports — `REQ-EXEC-018`, `REQ-DESIGN-017`.

### GREQ-021 — Revocation and Compromise Semantics

Revocation or compromise evidence MUST identify scope, reason, time, affected purposes, and historical effect, and MUST NOT erase prior state or imply retroactivity beyond governed rules.

**Protects:** GINV-005, GINV-006, GINV-007.

**Primary mappings:** Refines — `REQ-KT-016–REQ-KT-023`; Supports — `REQ-OBJ-016`, `REQ-GOV-055`.

### GREQ-022 — Delegation and Custody Boundaries

Delegation, subdelegation, key custody, signing service operation, and organizational control MUST remain distinguishable and MUST preserve the Authority chain and Control Domains relevant to the action.

**Protects:** GINV-019, GINV-034.

**Primary mappings:** Refines — `REQ-KT-039–REQ-KT-044`; Supports — `REQ-SYS-017`, `REQ-GOV-004`.

### GREQ-023 — Algorithm and Suite Identification

Every cryptographic operation MUST identify the algorithm, suite, parameters, canonicalization, domain, and version needed for deterministic processing and safe unknown-algorithm behavior.

**Protects:** GINV-004, GINV-029, GINV-030.

**Primary mappings:** Refines — `REQ-OBJ-003`, `REQ-ER-013`, `REQ-KT-045–REQ-KT-047`; Supports — `REQ-PHIL-015`.

### GREQ-024 — Cryptographic Agility and Historical Policy

Governance and implementations MUST support deprecation, replacement, renewal, and historical Verification of cryptographic mechanisms without altering original evidence or silently applying current policy retroactively.

**Protects:** GINV-004, GINV-007, GINV-029.

**Primary mappings:** Equivalent — `REQ-PROB-018`, `REQ-DESIGN-017`; Refines — `REQ-PRES-041–REQ-PRES-048`, `REQ-GOV-057–REQ-GOV-058`.

### GREQ-025 — Cryptographic Failure Visibility

Malformed, unsupported, ambiguous, invalid, conflicted, or unavailable cryptographic inputs and proofs MUST produce explicit bounded results and MUST NOT be treated as successful verification.

**Protects:** GINV-004, GINV-011, GINV-036.

**Primary mappings:** Refines — `REQ-OBJ-025`, `REQ-ER-025–REQ-ER-026`; Supports — `REQ-PHIL-007`, `REQ-VER-032–REQ-VER-035`.

## Evidence Lifecycle and Historical State

### GREQ-026 — Finalization and Immutability

Protected Protocol Object content MUST NOT be modified in place after finalization, identifier or digest generation, signing, Commitment, or incorporation into another protected artifact.

**Protects:** GINV-001, GINV-002, GINV-023.

**Primary mappings:** Equivalent — `REQ-OBJ-015`, `REQ-ER-017`; Refines — `REQ-CHAIN-020`, `REQ-CP-026`.

### GREQ-027 — Explicit Lifecycle States

Specifications and implementations MUST use distinct, governed lifecycle states and MUST expose the exact state established by an operation or item of evidence.

**Protects:** GINV-022, GINV-023, GINV-036.

**Primary mappings:** Equivalent — `REQ-DESIGN-007`, `REQ-ER-018`; Refines — `REQ-API-018–REQ-API-023`.

### GREQ-028 — Submission, Acceptance, and Commitment Separation

Submission, transport receipt, validation, acceptance, persistence, Commitment, Witnessing, Checkpointing, Anchoring, and Preservation MUST remain distinct, and each claimed transition MUST have applicable evidence.

**Protects:** GINV-002, GINV-005, GINV-022, GINV-023.

**Primary mappings:** Equivalent — `REQ-PHIL-012`, `REQ-ER-018`; Refines — `REQ-SYS-006`, `REQ-ER-019`.

### GREQ-029 — Append-Only Correction

Correction, annotation, reversal, dispute, withdrawal, and supersession of committed evidence MUST create new attributable relationships while preserving the original object and historical state.

**Protects:** GINV-002, GINV-006, GINV-023, GINV-036.

**Primary mappings:** Equivalent — `REQ-PHIL-004`, `REQ-EXEC-005`; Refines — `REQ-OBJ-016`, `REQ-ER-020–REQ-ER-021`.

### GREQ-030 — Accountable Administrative Transition

Administrative, policy, Authority, profile, Registry, retention, quorum, release, and governance changes that affect future accountability MUST create versioned transition evidence with an explicit effective boundary.

**Protects:** GINV-002, GINV-006, GINV-023, GINV-033, GINV-034, GINV-036.

**Primary mappings:** Equivalent — `REQ-PHIL-014`, `REQ-EXEC-017`; Refines — `REQ-SYS-020`, `REQ-GOV-005`, `REQ-GOV-030`.

---

## Hash Chains and Commitment

### GREQ-031 — Governed Chain Identity and Genesis

TAIP MUST define governed Hash Chain, Chain Entry, Chain Head, Commitment Receipt, Chain Identifier, and genesis semantics sufficient to recognize one historical domain and its valid starting boundary.

**Protects:** GINV-005, GINV-023.

**Primary mappings:** Equivalent — `REQ-CHAIN-001–REQ-CHAIN-003`; Supports — `REQ-OBJ-001`, `REQ-OBJ-005`.

### GREQ-032 — Deterministic Chain Transition

Each Chain version MUST define deterministic canonicalization, domain separation, algorithm context, entry semantics, and transition construction sufficient for independent reproduction of Chain state.

**Protects:** GINV-003, GINV-005.

**Primary mappings:** Refines — `REQ-CHAIN-004`, `REQ-CHAIN-006`; Supports — `REQ-OBJ-003`, `REQ-DESIGN-003`.

### GREQ-033 — Predecessor and Committed-Material Binding

Every non-genesis Chain Entry MUST cryptographically bind the Chain identity, predecessor state, committed material, type, version, and substitution-sensitive ordering context.

**Protects:** GINV-003, GINV-004, GINV-005.

**Primary mappings:** Equivalent — `REQ-CHAIN-005`, `REQ-CHAIN-008`; Refines — `REQ-CHAIN-011`.

### GREQ-034 — Atomic Append and Concurrency

Chain append processing MUST preserve atomicity or explicitly governed branch behavior and MUST expose retries, rejections, conflicts, duplicate attempts, and the resulting order.

**Protects:** GINV-002, GINV-003, GINV-036.

**Primary mappings:** Equivalent — `REQ-CHAIN-009`, `REQ-CHAIN-010`; Refines — `REQ-CHAIN-024–REQ-CHAIN-025`, `REQ-CHAIN-029`.

### GREQ-035 — Conflict, Branch, and Equivocation Visibility

Incompatible successors, duplicate exclusive positions, conflicting Chain Heads, permitted branches, and equivocation evidence MUST remain visible and MUST be evaluated under explicit selection, merge, closure, and Trust Profile rules.

**Protects:** GINV-002, GINV-005, GINV-011, GINV-036.

**Primary mappings:** Equivalent — `REQ-CHAIN-021`, `REQ-CHAIN-022`; Refines — `REQ-CHAIN-023`; Supports — `REQ-ER-034`.

### GREQ-036 — Ordering and Time Scope

Sequence, range, partition, epoch, and Commitment Time semantics MUST be protected and MUST NOT be generalized beyond the exact Chain and time scope established by evidence.

**Protects:** GINV-005, GINV-024.

**Primary mappings:** Equivalent — `REQ-CHAIN-011–REQ-CHAIN-013`; Supports — `REQ-PROB-006`.

### GREQ-037 — Partition, Epoch, Closure, and Rollover

Partitioning, segmentation, epoch transition, closure, rollover, sharding, and Chain migration MUST identify continuity or discontinuity, boundaries, assignment rules, successor relationships, and unresolved gaps.

**Protects:** GINV-005, GINV-006, GINV-023, GINV-036.

**Primary mappings:** Refines — `REQ-CHAIN-026–REQ-CHAIN-028`, `REQ-CHAIN-039`; Supports — `REQ-PRES-048`.

### GREQ-038 — Inclusion, Range, and Consistency Proofs

Chain proof formats MUST bind proof type, algorithm, target Chain state, committed material, membership or range semantics, and the exact conclusion established.

**Protects:** GINV-004, GINV-005, GINV-015.

**Primary mappings:** Equivalent — `REQ-CHAIN-018`; Refines — `REQ-KT-035–REQ-KT-038`; Supports — `REQ-CP-041`.

### GREQ-039 — Commitment Receipts and Operator Claims

Commitment Receipts MUST bind committed material, Chain identity and state, version, semantics, issuer, and protected scope, and their validation MUST remain distinct from independent Chain-continuity Verification.

**Protects:** GINV-004, GINV-005, GINV-014.

**Primary mappings:** Equivalent — `REQ-CHAIN-015–REQ-CHAIN-017`; Supports — `REQ-SYS-006`.

### GREQ-040 — No Implied Completeness or Truth

Chain inclusion, absence, Commitment, Checkpointing, anchoring, storage, or cryptographic validity MUST NOT imply Evidence Completeness, non-occurrence, Authority, or external truth without separately governed evidence.

**Protects:** GINV-011, GINV-014, GINV-015.

**Primary mappings:** Refines — `REQ-CHAIN-019–REQ-CHAIN-020`; Supports — `REQ-PHIL-006`, `REQ-CP-017`, `REQ-KT-037`.

### GREQ-041 — Bounded Reproducible Chain Verification

Chain Verification MUST distinguish availability, structure, semantics, canonicalization, cryptography, linkage, object binding, operator evidence, range, conflict, Completeness, and Trust Profile results under an explicit context.

**Protects:** GINV-003, GINV-013.

**Primary mappings:** Equivalent — `REQ-CHAIN-040–REQ-CHAIN-042`; Supports — `REQ-VER-020–REQ-VER-031`.

### GREQ-042 — Portable Historical Chain Evidence

Chain exports, preservation, recovery, and migration MUST retain entries, boundaries, objects, proofs, conflicts, gaps, algorithms, governing versions, and independently retained state required to detect rollback and reproduce conclusions.

**Protects:** GINV-005, GINV-006, GINV-025, GINV-033.

**Primary mappings:** Refines — `REQ-CHAIN-034`, `REQ-CHAIN-037–REQ-CHAIN-039`; Supports — `REQ-PRES-023`, `REQ-PRES-053`.

## Witnesses, Quorum, Checkpoints, and Anchors

### GREQ-043 — Governed Witness Observation

TAIP MUST define versioned Witness Observation types, Observation Scopes, result semantics, lifecycle, responsible Witness identity, and validation behavior.

**Protects:** GINV-004, GINV-008, GINV-019, GINV-023, GINV-024.

**Primary mappings:** Equivalent — `REQ-WIT-001–REQ-WIT-003`; Refines — `REQ-WIT-011–REQ-WIT-014`.

### GREQ-044 — Witness Target and Protected Context

Every Witness Observation MUST bind the exact target, target type, identifier or namespace, commitment, scope, version, result, time, Witness identity, and replay-sensitive context required by its claim.

**Protects:** GINV-004, GINV-008.

**Primary mappings:** Equivalent — `REQ-WIT-005–REQ-WIT-008`; Refines — `REQ-WIT-009–REQ-WIT-010`, `REQ-WIT-015`.

### GREQ-045 — Historical Witness Eligibility

Witness eligibility MUST be evaluated under the identity, key, scope, status, Registry, profile, time, and conflict conditions applicable to the observation and quorum boundary.

**Protects:** GINV-008, GINV-019, GINV-023.

**Primary mappings:** Refines — `REQ-WIT-019–REQ-WIT-020`, `REQ-WIT-024`, `REQ-WIT-032`; Supports — `REQ-WIT-047`.

### GREQ-046 — Evidence of Witness Independence

Claims of Witness independence MUST identify the required independence dimensions and evidence of organizational, administrative, infrastructure, key-custody, economic, and correlated-failure separation.

**Protects:** GINV-019, GINV-021.

**Primary mappings:** Equivalent — `REQ-WIT-021–REQ-WIT-022`; Refines — `REQ-SYS-017`, `REQ-TP-033–REQ-TP-038`.

### GREQ-047 — Control-Domain and Role-Combination Visibility

Common control, role combination, shared operators, shared custody, conflicts of interest, and overlapping Control Domains MUST remain visible wherever they affect Witness, Preservation, governance, or Trust Profile assurance.

**Protects:** GINV-021.

**Primary mappings:** Refines — `REQ-WIT-023–REQ-WIT-024`, `REQ-WIT-029`; Supports — `REQ-PRES-026`, `REQ-GOV-013`.

### GREQ-048 — Deterministic Quorum Evaluation

Quorum policies MUST define eligible populations, target and scope compatibility, thresholds, denominators, weighting, Control Domains, timing, conflict, dynamic membership, degradation, and deterministic arithmetic.

**Protects:** GINV-008, GINV-013, GINV-016, GINV-021.

**Primary mappings:** Equivalent — `REQ-WIT-025–REQ-WIT-030`; Refines — `REQ-WIT-031–REQ-WIT-034`.

### GREQ-049 — Witness Conflict, Absence, and Degradation

Conflicting observations, dissent, non-response, timeout, unavailability, refusal, late evidence, exclusion, and failed quorum MUST remain distinguishable and MUST affect the Achieved Trust Profile explicitly.

**Protects:** GINV-008, GINV-011, GINV-018, GINV-036.

**Primary mappings:** Equivalent — `REQ-WIT-035–REQ-WIT-037`; Refines — `REQ-WIT-045–REQ-WIT-048`.

### GREQ-050 — Governed Checkpoint Semantics

TAIP MUST define Checkpoint types, scope, target state, stable identity, lifecycle, protected properties, dependencies, and validation rules sufficient to reproduce the exact historical boundary claimed.

**Protects:** GINV-004, GINV-009, GINV-023, GINV-024.

**Primary mappings:** Equivalent — `REQ-CP-001–REQ-CP-003`; Refines — `REQ-CP-013–REQ-CP-020`.

### GREQ-051 — Checkpoint Authority and Target Binding

Every Checkpoint MUST identify the Checkpoint Authority and cryptographically bind the exact target, namespace, position or range, version context, time semantics, policy, profile, and required dependencies.

**Protects:** GINV-004, GINV-009, GINV-019.

**Primary mappings:** Equivalent — `REQ-CP-004–REQ-CP-010`; Refines — `REQ-CP-018–REQ-CP-019`.

### GREQ-052 — Checkpoint Cadence and Succession

Checkpoint policies MUST define cadence, delay, grace, failure, retry, publication, predecessor, succession, closure, migration, missed-checkpoint behavior, and the resulting assurance effect.

**Protects:** GINV-005, GINV-006, GINV-009, GINV-036.

**Primary mappings:** Refines — `REQ-CP-011–REQ-CP-012`, `REQ-CP-020–REQ-CP-025`, `REQ-CP-049`.

### GREQ-053 — External Anchor Evidence

External Anchor Evidence MUST bind a TAIP commitment to the exact external namespace, target, transaction or publication state, account or issuer, binding rule, algorithm, and retrieval or proof material.

**Protects:** GINV-004, GINV-005, GINV-009.

**Primary mappings:** Refines — `REQ-CP-027–REQ-CP-036`; Supports — `REQ-KT-041`.

### GREQ-054 — Anchor Finality and Trust-Boundary Limits

Anchor Verification MUST distinguish submission, acceptance, publication, confirmation, finality, reorganization, availability, and proof scope and MUST NOT imply underlying evidence validity, Completeness, Authority, or truth.

**Protects:** GINV-009, GINV-014, GINV-018, GINV-036.

**Primary mappings:** Refines — `REQ-CP-031–REQ-CP-043`; Supports — `REQ-WIT-039`, `REQ-VER-026`.

## Key Transparency and Preservation

### GREQ-055 — Versioned Key Transparency History

TAIP MUST define versioned Key Transparency Records, Historical Key State, lifecycle transitions, exact key-material binding, applicable namespaces, and integrity protection.

**Protects:** GINV-005, GINV-007, GINV-026, GINV-033.

**Primary mappings:** Equivalent — `REQ-KT-001–REQ-KT-006`; Refines — `REQ-PRES-020–REQ-PRES-021`.

### GREQ-056 — Authenticated Key-History Ordering and Proofs

Key Transparency histories MUST provide authenticated ordering, Checkpoints, inclusion and consistency proofs, predecessor validation, and typed non-inclusion behavior sufficient to detect prohibited transitions and relevant gaps.

**Protects:** GINV-005, GINV-007, GINV-013.

**Primary mappings:** Refines — `REQ-KT-029`, `REQ-KT-032–REQ-KT-038`; Supports — `REQ-KT-040–REQ-KT-041`.

### GREQ-057 — Key-History Conflict and Split-View Evidence

Conflicting key histories, predecessor mismatches, duplicate positions, split views, disputed compromise boundaries, corrections, and revocations MUST remain visible as evidence and MUST produce bounded Verification results.

**Protects:** GINV-006, GINV-007, GINV-011, GINV-036.

**Primary mappings:** Refines — `REQ-KT-019–REQ-KT-020`, `REQ-KT-028–REQ-KT-031`, `REQ-KT-039`.

### GREQ-058 — Deterministic Historical Key Resolution

Historical resolvers MUST accept or derive an explicit target boundary and return lifecycle state, Key Purpose, supporting records, conflicts, uncertainty, and missing dependencies without defaulting silently to current state.

**Protects:** GINV-007, GINV-013.

**Primary mappings:** Equivalent — `REQ-KT-042–REQ-KT-045`; Supports — `REQ-PRES-021`, `REQ-VER-017`.

### GREQ-059 — Preservation Scope and Evidence Lifetime

Preservation plans MUST identify targets, object classes, Accountability Claims, Trust Profiles, Evidence Lifetime, retention rules, responsible services, custody, and the Verification Dependency Graph in scope.

**Protects:** GINV-010, GINV-012, GINV-025, GINV-026, GINV-028, GINV-033.

**Primary mappings:** Equivalent — `REQ-PRES-001–REQ-PRES-007`; Refines — `REQ-PHIL-015`, `REQ-EXEC-014`.

### GREQ-060 — Preservation of Interpretation Dependencies

Schemas, canonicalization, algorithms, historical keys, Authorities, Policies, Registries, Trust Profiles, Chains, Witness evidence, Checkpoints, anchors, extensions, tests, and governance material required for future Verification MUST be embedded, preserved, or durably resolvable.

**Protects:** GINV-010, GINV-012, GINV-025, GINV-026.

**Primary mappings:** Equivalent — `REQ-PRES-016–REQ-PRES-023`; Refines — `REQ-OBJ-024`, `REQ-WIT-044`, `REQ-CP-047`.

---

### GREQ-061 — Fixity, Integrity Monitoring, and Repair

Preservation Services MUST provide attributable fixity and integrity evidence for defined target populations and MUST record corruption, expected and observed values, repair sources, methods, outcomes, and unresolved uncertainty.

**Protects:** GINV-010, GINV-011, GINV-036.

**Primary mappings:** Refines — `REQ-PRES-014–REQ-PRES-015`, `REQ-PRES-035–REQ-PRES-036`; Supports — `REQ-PRES-055`.

### GREQ-062 — Redundancy, Recovery, and Independence

Replication, WORM controls, backups, recovery, disaster recovery, and independent Preservation MUST identify exact scope and Control Domains and MUST remain distinct from durability, availability, integrity, and successful restoration.

**Protects:** GINV-010, GINV-021, GINV-025.

**Primary mappings:** Refines — `REQ-PRES-024–REQ-PRES-027`, `REQ-PRES-037–REQ-PRES-039`; Supports — `REQ-PHIL-005`.

### GREQ-063 — Retention, Legal Hold, and Disposition

Retention, expiry, Legal Hold, custody, deletion eligibility, and disposition MUST use versioned Policies, accountable Authorities, explicit targets and time boundaries, dependency checks, exceptions, and outcome evidence.

**Protects:** GINV-006, GINV-010, GINV-023, GINV-036.

**Primary mappings:** Refines — `REQ-PRES-008–REQ-PRES-013`, `REQ-PRES-041–REQ-PRES-045`; Supports — `REQ-GOV-030`.

### GREQ-064 — Encryption and Decryption Availability

Encrypted evidence MUST identify the encryption suite, keys and versions, protected scope, authenticated metadata, Authority, recovery semantics, and the explicit Verification effect of inaccessible or destroyed decryption capability.

**Protects:** GINV-010, GINV-027, GINV-028, GINV-036.

**Primary mappings:** Refines — `REQ-PRES-030–REQ-PRES-033`, `REQ-DP-032–REQ-DP-035`; Supports — `REQ-ER-032`.

### GREQ-065 — Renewal and Recovery Evidence

Cryptographic renewal, recovery, repair, and restoration MUST bind the original protected state to the new protection or recovered state and MUST identify what property was preserved, added, changed, or lost.

**Protects:** GINV-002, GINV-006, GINV-010, GINV-029.

**Primary mappings:** Refines — `REQ-PRES-036–REQ-PRES-039`, `REQ-PRES-051–REQ-PRES-052`; Supports — `REQ-DESIGN-017`.

### GREQ-066 — Migration, Deletion, and Service-Closure Evidence

Migration, cryptographic erasure, deletion, custody transfer, provider change, and service closure MUST bind source, target, Authority, scope, dependencies, transformation, validation, residual information, failures, and successor resolution.

**Protects:** GINV-002, GINV-006, GINV-010, GINV-029, GINV-036.

**Primary mappings:** Refines — `REQ-PRES-044–REQ-PRES-054`; Supports — `REQ-PROB-017`, `REQ-GOV-057–REQ-GOV-060`.

## Dispute Packs and Portable Review

### GREQ-067 — Authoritative Dispute Pack Manifest

Every conforming Dispute Pack MUST contain an authoritative governed Manifest identifying pack identity, version, assembler, focal claims or bounded review scope, expected Verification Context, and declared membership.

**Protects:** GINV-001, GINV-011, GINV-012, GINV-015, GINV-025.

**Primary mappings:** Equivalent — `REQ-DP-001–REQ-DP-006`; Supports — `REQ-EXEC-013`.

### GREQ-068 — Explicit Package Boundary and Entry Semantics

Every material Pack entry MUST identify its role, identity, version, representation, integrity, location, provenance, and relationship to canonical evidence; entry meaning MUST NOT be inferred from container layout alone.

**Protects:** GINV-001, GINV-011, GINV-012, GINV-025.

**Primary mappings:** Refines — `REQ-DP-007–REQ-DP-010`, `REQ-DP-015–REQ-DP-016`; Supports — `REQ-OBJ-019`.

### GREQ-069 — Package Integrity and Protected Manifest Scope

Pack and entry integrity MUST bind all properties that affect identity, membership, claims, context, dependencies, omissions, disclosure, transformations, handling, and version semantics.

**Protects:** GINV-001, GINV-004, GINV-012, GINV-025.

**Primary mappings:** Refines — `REQ-DP-010–REQ-DP-014`; Supports — `REQ-DP-039–REQ-DP-043`.

### GREQ-070 — Omission, Absence, Conflict, and Completeness States

Packs and Verification Engines MUST distinguish included, omitted, unavailable, redacted, unresolved, unsupported, conflicting, not known to exist, and proven absent material under a bounded Completeness model.

**Protects:** GINV-011, GINV-015, GINV-027, GINV-036.

**Primary mappings:** Equivalent — `REQ-DP-018–REQ-DP-029`; Refines — `REQ-DP-043`, `REQ-DP-057`.

### GREQ-071 — Redaction and Derived-Representation Provenance

Redacted, selectively disclosed, transformed, compressed, encrypted, normalized, or derived material MUST identify or integrity-bind its canonical source, method, Authority, generator, information loss, and Verification effect.

**Protects:** GINV-006, GINV-027, GINV-028.

**Primary mappings:** Equivalent — `REQ-DP-030–REQ-DP-031`; Refines — `REQ-ER-030`, `REQ-PRES-049`; Supports — `REQ-EXEC-016`.

### GREQ-072 — External Dependency Resolution Evidence

Mandatory external Pack dependencies MUST identify stable identity, exact version, integrity, role, resolver requirements, access limits, and resolution evidence including conflicts and failures.

**Protects:** GINV-010, GINV-011, GINV-025, GINV-026, GINV-027.

**Primary mappings:** Refines — `REQ-DP-020–REQ-DP-024`; Supports — `REQ-VER-008–REQ-VER-011`.

### GREQ-073 — Portable and Offline Verification Packages

Dispute Packs and protocol exports MUST state which claims and checks can be evaluated offline and MUST disclose reliance on producer services, proprietary formats, privileged databases, external resolvers, or separately delivered keys.

**Protects:** GINV-012, GINV-025, GINV-031.

**Primary mappings:** Refines — `REQ-DP-023`, `REQ-DP-047–REQ-DP-048`, `REQ-DP-060`; Supports — `REQ-PHIL-010`, `REQ-VER-054`.

### GREQ-074 — Safe Container and Resource Processing

Package bindings and Verification implementations MUST define safe path handling, duplicate and special-file behavior, deterministic extraction, streaming and chunk integrity, decompression limits, nesting, cardinality, cryptographic cost, and dependency bounds.

**Protects:** GINV-012, GINV-025, GINV-031.

**Primary mappings:** Refines — `REQ-DP-036–REQ-DP-038`; Supports — `REQ-ER-033`, `REQ-VER-009`, `REQ-VER-014`.

## Verification

### GREQ-075 — Explicit Verification Context

Every Verification run MUST bind focal claims, evidence, historical boundary, Verification Time, protocol and profile versions, schemas, algorithms, Policies, Registries, trust roots, resolvers, dependencies, disclosure state, and material runtime defaults.

**Protects:** GINV-007, GINV-008, GINV-009, GINV-012, GINV-013, GINV-024, GINV-026.

**Primary mappings:** Equivalent — `REQ-VER-001–REQ-VER-007`; Refines — `REQ-DP-004–REQ-DP-005`.

### GREQ-076 — Layered Verification Checks

Verification MUST evaluate and preserve separate availability, safety, structure, semantics, identifier, canonicalization, cryptographic, Authority, historical, reference, lifecycle, Chain, Witness, Checkpoint, Anchor, Preservation, package, and causal results as applicable.

**Protects:** GINV-007, GINV-012, GINV-013, GINV-020.

**Primary mappings:** Refines — `REQ-VER-014–REQ-VER-035`; Supports — `REQ-OBJ-017`, `REQ-DP-056`.

### GREQ-077 — Typed Verification Outcomes

TAIP MUST define deterministic codes and composition rules for Valid, Invalid, Incomplete, Indeterminate, Unsupported, Unavailable, Conflicting, warning, and Not Evaluated states, with precise use conditions.

**Protects:** GINV-008, GINV-013, GINV-014, GINV-018, GINV-036.

**Primary mappings:** Equivalent — `REQ-VER-041–REQ-VER-049`; Supports — `REQ-PHIL-007`, `REQ-DP-062`.

### GREQ-078 — Validity, Completeness, and Dependency Propagation

Object Validity, Evidence Completeness, dependency state, claim outcome, control result, and aggregate outcome MUST remain separate, and failures MUST propagate only through documented dependencies.

**Protects:** GINV-011, GINV-013, GINV-015.

**Primary mappings:** Equivalent — `REQ-VER-036–REQ-VER-037`, `REQ-VER-049–REQ-VER-050`; Supports — `REQ-PHIL-006`, `REQ-TP-032`.

### GREQ-079 — Intended and Achieved Trust Profile Evaluation

Verification MUST evaluate every mandatory control of the Intended Trust Profile using actual evidence and MUST report the Achieved Trust Profile, unsatisfied controls, downgrade rule, and effect on claims.

**Protects:** GINV-008, GINV-013, GINV-016, GINV-017, GINV-018.

**Primary mappings:** Equivalent — `REQ-VER-038–REQ-VER-040`; Refines — `REQ-TP-025–REQ-TP-043`.

### GREQ-080 — Versioned Verification Reports

Every Verification Report MUST identify its identity, version, verifier, context, time, evidence and claim scope, checks, dependencies, outcomes, Completeness, Intended and Achieved Trust Profiles, warnings, and limitations; later evidence MUST produce a new report.

**Protects:** GINV-009, GINV-011, GINV-012, GINV-013, GINV-014, GINV-025, GINV-036.

**Primary mappings:** Equivalent — `REQ-VER-057–REQ-VER-062`; Refines — `REQ-DP-044–REQ-DP-045`, `REQ-DP-058–REQ-DP-059`.

## Trust Profiles

### GREQ-081 — Stable Trust Profile Identity

Every Trust Profile MUST possess a governed namespace, stable identifier, exact immutable version, Profile Authority, lifecycle state, assurance objective, scope, and integrity-protected definition.

**Protects:** GINV-016, GINV-017, GINV-033.

**Primary mappings:** Equivalent — `REQ-TP-001–REQ-TP-006`; Supports — `REQ-PHIL-009`.

### GREQ-082 — Explicit Profile Controls and Criticality

Every profile control MUST identify stable identity, purpose, applicability, criticality, inputs, deterministic criteria, thresholds, missing-data behavior, prohibited behavior, and effect on outcomes.

**Protects:** GINV-016, GINV-036.

**Primary mappings:** Equivalent — `REQ-TP-009–REQ-TP-015`; Refines — `REQ-TP-027–REQ-TP-028`.

### GREQ-083 — Profile Dependencies and Binding

Profiles and profile bindings MUST identify exact dependencies, versions, integrity, selection inputs, selecting Authority or Policy, scope, effective boundary, and permitted degradation behavior.

**Protects:** GINV-016, GINV-017, GINV-026, GINV-033.

**Primary mappings:** Refines — `REQ-TP-016–REQ-TP-024`; Supports — `REQ-PRES-022`.

### GREQ-084 — Evidence-Based Profile Achievement

A profile MUST be reported as achieved only when every applicable mandatory control and dependency satisfies its governed composition rules for the exact evidence, scope, context, and time evaluated.

**Protects:** GINV-016, GINV-017.

**Primary mappings:** Equivalent — `REQ-TP-025–REQ-TP-035`; Supports — `REQ-EXEC-011`, `REQ-SYS-015`.

### GREQ-085 — Profile Inheritance, Composition, and Mapping

Inheritance, composition, equivalence, implication, ordering, and mapping MUST bind exact versions, components, parameters, precedence, unmatched controls, direction, assumptions, conflicts, and resulting identity.

**Protects:** GINV-016, GINV-033.

**Primary mappings:** Refines — `REQ-TP-047–REQ-TP-052`; Supports — `REQ-GOV-047`.

### GREQ-086 — Profile Independence and Quorum Controls

Profiles claiming independent assurance MUST define historical Control Domain criteria, qualifying evidence, quorum populations, eligibility, diversity, role composition, unknown-state behavior, and conflict handling.

**Protects:** GINV-016, GINV-021.

**Primary mappings:** Refines — `REQ-TP-044–REQ-TP-046`; Supports — `REQ-WIT-020–REQ-WIT-030`.

### GREQ-087 — Profile Downgrade, Fallback, and Degradation

Failure to achieve an Intended Trust Profile MUST identify failed controls and MAY report a fallback only after the fallback's own mandatory requirements are independently satisfied under an authorized degradation rule.

**Protects:** GINV-017, GINV-018, GINV-036.

**Primary mappings:** Equivalent — `REQ-TP-036–REQ-TP-041`; Refines — `REQ-DESIGN-011`, `REQ-WIT-037`.

### GREQ-088 — Profile Registry and Governance

Profile Authorities and Registries MUST preserve identity, version, digest, Authority, lifecycle, dependencies, canonical definition, conflicts, custom namespaces, security process, and historical snapshots.

**Protects:** GINV-023, GINV-026, GINV-034.

**Primary mappings:** Refines — `REQ-TP-005–REQ-TP-008`, `REQ-TP-053–REQ-TP-057`; Supports — `REQ-GOV-008`, `REQ-GOV-060`.

### GREQ-089 — Profile Migration and Reassessment

Profile upgrade, migration, late evidence, changed context, and reassessment MUST identify source and target versions, control mappings, new evidence, effective boundary, loss, and a new Verification result without rewriting earlier achievement.

**Protects:** GINV-002, GINV-006, GINV-016, GINV-029, GINV-033.

**Primary mappings:** Refines — `REQ-TP-042–REQ-TP-043`, `REQ-TP-047–REQ-TP-052`; Supports — `REQ-GOV-057–REQ-GOV-058`.

### GREQ-090 — Profile Conformance, Certification, and Claims

Capability, conformance, certification, and attestation claims MUST identify exact profile and suite versions, subject, assessor, scope, options, environment, period, exclusions, exceptions, evidence, and validity conditions and MUST remain distinct from profile achievement.

**Protects:** GINV-015, GINV-016, GINV-035.

**Primary mappings:** Equivalent — `REQ-TP-058–REQ-TP-060`; Supports — `REQ-TP-035`, `REQ-GOV-062`.

---

### GREQ-091 — Profile Privacy and External-Conclusion Boundaries

Privacy, redaction, selective disclosure, or access restriction MUST preserve full profile achievement only when every affected mandatory control has governed verifiable substitute evidence, and profile results MUST NOT be represented as automatic external truth or legal compliance.

**Protects:** GINV-016, GINV-027, GINV-028.

**Primary mappings:** Equivalent — `REQ-TP-061–REQ-TP-062`; Supports — `REQ-DESIGN-022`, `REQ-MAP-049–REQ-MAP-051`.

## APIs, Bindings, and SDKs

### GREQ-092 — Protocol, API, and SDK Separation

Bindings, APIs, SDKs, reference implementations, and products MUST identify the TAIP semantics and versions they expose and MUST NOT become undocumented sources of canonical protocol meaning.

**Protects:** GINV-022, GINV-031.

**Primary mappings:** Equivalent — `REQ-API-001–REQ-API-005`; Refines — `REQ-MAP-028–REQ-MAP-031`.

### GREQ-093 — Transport and Representation Mapping

Every binding and representation MUST define deterministic mapping among logical protocol meaning, wire bytes, canonical bytes, storage and presentation forms, statuses, errors, security, unknown values, and lossy or derived content.

**Protects:** GINV-003, GINV-025, GINV-031.

**Primary mappings:** Refines — `REQ-API-015–REQ-API-016`, `REQ-API-046–REQ-API-047`; Supports — `REQ-MAP-028–REQ-MAP-029`.

### GREQ-094 — Request Identity, Idempotency, Retry, and Replay

Operation identity, object identity, idempotency, correlation, retry, duplicate, replay, concurrency, and ambiguous-timeout semantics MUST remain distinct and MUST prevent unrelated or conflicting requests from being reported as prior success.

**Protects:** GINV-001, GINV-004, GINV-022, GINV-036.

**Primary mappings:** Refines — `REQ-API-006–REQ-API-008`, `REQ-API-025–REQ-API-034`; Supports — `REQ-ER-022–REQ-ER-023`.

### GREQ-095 — Lifecycle Operation Fidelity

Creation, finalization, signing, submission, acceptance, Commitment, asynchronous status, receipt, cancellation, and batch interfaces MUST expose the exact protocol state and evidence they establish without deriving stronger state from transport success.

**Protects:** GINV-022, GINV-023, GINV-036.

**Primary mappings:** Refines — `REQ-API-012–REQ-API-026`, `REQ-API-033–REQ-API-035`; Supports — `REQ-ER-018–REQ-ER-019`.

### GREQ-096 — Stable Error and Outcome Fidelity

Bindings MUST define stable machine-readable error and outcome categories and MUST preserve member-level, warning, limitation, pending, transport, and protocol distinctions without allowing human messages or generic status codes to replace canonical results.

**Protects:** GINV-022, GINV-036.

**Primary mappings:** Equivalent — `REQ-API-010–REQ-API-011`, `REQ-API-036–REQ-API-037`; Supports — `REQ-VER-041–REQ-VER-050`.

### GREQ-097 — Safe Version Negotiation

Version negotiation MUST produce an explicit effective context and MUST reject unsupported mandatory semantics rather than silently selecting an older unsafe protocol, weaker algorithm, lower Trust Profile, or lossy representation.

**Protects:** GINV-018, GINV-030, GINV-036.

**Primary mappings:** Equivalent — `REQ-API-041–REQ-API-043`; Refines — `REQ-GOV-049–REQ-GOV-051`.

### GREQ-098 — Versioned Capability Declarations

Capability documents MUST scope supported operations, types, versions, representations, algorithms, profiles, extensions, limits, and conformance suites and SHOULD be authenticated, freshness-bounded, and preservable when relied upon.

**Protects:** GINV-030, GINV-035.

**Primary mappings:** Equivalent — `REQ-API-044–REQ-API-045`; Refines — `REQ-GOV-052`; Supports — `REQ-TP-058`.

### GREQ-099 — Accountable Resolution

Resolver responses MUST identify the requested namespace, returned identity, type, version, integrity, source role, historical boundary, retrieval state, and conflicts, and resolved material MUST be validated before use.

**Protects:** GINV-012, GINV-025, GINV-026.

**Primary mappings:** Equivalent — `REQ-API-048–REQ-API-050`; Refines — `REQ-VER-010–REQ-VER-011`, `REQ-VER-053`.

### GREQ-100 — Collection, Pagination, Streaming, and Notification Boundaries

Collection, pagination, streaming, callback, and webhook interfaces MUST define identity, ordering, snapshot or live state, cursor and resume behavior, duplication, gaps, truncation, concurrent change, authenticity, replay, delivery, and Completeness limits.

**Protects:** GINV-011, GINV-022, GINV-023, GINV-036.

**Primary mappings:** Refines — `REQ-API-051–REQ-API-054`; Supports — `REQ-DP-037`.

### GREQ-101 — Safe SDK Defaults and Replaceability

SDKs SHOULD default to strict parsing, safe algorithms, explicit finalization, exact version binding, integrity validation, bounded retry, rich outcomes, and no silent downgrade, while remaining independently replaceable.

**Protects:** GINV-025, GINV-031.

**Primary mappings:** Equivalent — `REQ-API-061`; Refines — `REQ-MAP-030–REQ-MAP-031`; Supports — `REQ-DESIGN-020–REQ-DESIGN-021`.

### GREQ-102 — Tenant, Privacy, Resource, and Offline Boundaries

APIs and SDKs MUST isolate tenants and credentials, preserve confidentiality and disclosure state, bound resources, expose timeout and partial-processing results, and distinguish offline local state from later protocol Commitment or synchronization.

**Protects:** GINV-018, GINV-025, GINV-027, GINV-028, GINV-036.

**Primary mappings:** Refines — `REQ-API-056`, `REQ-API-058–REQ-API-060`; Supports — `REQ-DP-023`, `REQ-PRES-061`.

## Normative Mapping, Governance, and Evolution

### GREQ-103 — Explicit Normative Boundary

Every governed specification MUST identify its authority, status, scope, normative and informative material, effective boundary, dependencies, and relationship to the Project Bible, TAIP Core, modules, profiles, Registries, schemas, bindings, and conformance artifacts.

**Protects:** GINV-031, GINV-032.

**Primary mappings:** Equivalent — `REQ-MAP-003–REQ-MAP-005`; Refines — `REQ-GOV-001–REQ-GOV-002`.

### GREQ-104 — Source Precedence and Semantic Ownership

The normative set MUST define source precedence and artifact ownership so that schemas, Registries, profiles, bindings, SDKs, examples, tests, and implementation behavior cannot silently redefine semantics outside their mandate.

**Protects:** GINV-031, GINV-032, GINV-034.

**Primary mappings:** Equivalent — `REQ-MAP-005`, `REQ-MAP-024–REQ-MAP-031`; Supports — `REQ-GOV-003`, `REQ-GOV-010`.

### GREQ-105 — Bidirectional Architectural Traceability

Governance MUST maintain stable bidirectional mappings from Project Bible sources through global identifiers to normative artifacts, roles, profiles, tests, impacts, lifecycle state, and explicit dispositions.

**Protects:** GINV-032, GINV-034.

**Primary mappings:** Equivalent — `REQ-MAP-001–REQ-MAP-002`, `REQ-MAP-009–REQ-MAP-010`, `REQ-MAP-039–REQ-MAP-040`.

### GREQ-106 — Requirement-to-Test Traceability

Every conformance assertion and test MUST identify the exact normative rule evaluated, applicable versions, inputs, expected results, comparison method, role, feature scope, environment, and known limitations.

**Protects:** GINV-013, GINV-032, GINV-035.

**Primary mappings:** Equivalent — `REQ-MAP-041–REQ-MAP-046`; Refines — `REQ-GOV-009`; Supports — `REQ-API-062`.

### GREQ-107 — Requirement Lifecycle and Coverage Gaps

Requirements, invariants, mappings, and tests MUST preserve stable identity, status, version, ownership, coverage, deferred or superseded disposition, and known gaps without silently dropping obligations.

**Protects:** GINV-032, GINV-035.

**Primary mappings:** Refines — `REQ-MAP-009–REQ-MAP-010`, `REQ-MAP-039–REQ-MAP-047`; Supports — `REQ-GOV-029–REQ-GOV-030`.

### GREQ-108 — Accountable Governance Authority

Normative, Registry, profile, conformance, security, and governance decisions MUST be made by identified Authorities acting within published mandates, conflict rules, review procedures, approval thresholds, and challenge mechanisms.

**Protects:** GINV-019, GINV-034.

**Primary mappings:** Refines — `REQ-GOV-001–REQ-GOV-026`; Supports — `REQ-MAP-059`.

### GREQ-109 — Authorized Release and Dependency Manifest

Every release MUST bind artifact identity, immutable version, lifecycle status, approved content, Authority evidence, exact normative dependencies, compatibility classification, conformance artifacts, effective boundary, and preservation locations.

**Protects:** GINV-026, GINV-032, GINV-033, GINV-034.

**Primary mappings:** Equivalent — `REQ-GOV-007`, `REQ-GOV-027–REQ-GOV-030`, `REQ-GOV-045`; Refines — `REQ-MAP-037–REQ-MAP-038`, `REQ-MAP-060`.

### GREQ-110 — Independent Version Dimensions

TAIP, object, schema, canonicalization, algorithm, profile, Registry, extension, binding, package, conformance-suite, governance, and implementation versions MUST remain distinguishable wherever they affect interpretation.

**Protects:** GINV-026, GINV-029, GINV-030, GINV-033.

**Primary mappings:** Equivalent — `REQ-API-041`, `REQ-GOV-039–REQ-GOV-046`; Supports — `REQ-MAP-052`.

### GREQ-111 — Scoped Compatibility

Compatibility declarations MUST identify source, target, direction, roles, categories, profiles, features, bindings, assumptions, exclusions, semantic differences, negotiation, and supporting conformance evidence.

**Protects:** GINV-018, GINV-030, GINV-036.

**Primary mappings:** Equivalent — `REQ-GOV-047–REQ-GOV-050`; Refines — `REQ-MAP-053–REQ-MAP-054`.

### GREQ-112 — Deprecation, Migration, and Historical Effect

Deprecation, suspension, withdrawal, revocation, supersession, migration, and compatibility transitions MUST identify Authority, scope, source and target, effective boundary, transformation, loss, validation, failure, successor, historical effect, and Preservation evidence.

**Protects:** GINV-006, GINV-018, GINV-023, GINV-029, GINV-033, GINV-036.

**Primary mappings:** Refines — `REQ-GOV-055–REQ-GOV-058`; Supports — `REQ-MAP-055`, `REQ-PRES-048–REQ-PRES-052`.

### GREQ-113 — Emergency and Security Change Accountability

Emergency changes and vulnerability handling MUST use authenticated reporting, bounded confidentiality, explicit triggers, authorized actors, scope, duration, affected-version analysis, compatibility review, advisory history, expiry, post-action review, and appeal.

**Protects:** GINV-006, GINV-034, GINV-036.

**Primary mappings:** Refines — `REQ-GOV-031–REQ-GOV-035`; Supports — `REQ-MAP-048`.

### GREQ-114 — Historical Normative Preservation

Historical specifications, dependencies, Registries, profiles, schemas, canonicalization rules, tests, decisions, interpretations, manifests, release proofs, and Authority evidence MUST remain retrievable through durable identity and independently verifiable integrity.

**Protects:** GINV-005, GINV-006, GINV-010, GINV-012, GINV-025, GINV-026, GINV-033.

**Primary mappings:** Equivalent — `REQ-GOV-061`; Refines — `REQ-MAP-038`, `REQ-PRES-019–REQ-PRES-023`.

### GREQ-115 — Conformance Transition and Reproducible Evolution

Every normative transition MUST identify affected roles, versions, profiles, suites, retesting, coexistence, grace periods, historical support, unresolved risks, and claim limitations so an independent reviewer can reproduce what changed and why.

**Protects:** GINV-032, GINV-034, GINV-035, GINV-036.

**Primary mappings:** Equivalent — `REQ-GOV-062`; Refines — `REQ-MAP-056–REQ-MAP-062`, `REQ-GOV-014`, `REQ-GOV-021–REQ-GOV-022`.

---

# 19.3 Requirement Ownership Matrix

Global identifiers need accountable ownership without erasing the authority of their source chapters.

## Source-Set Inventory

The current Project Bible index includes these published local identifier families:

| Source | Invariant range | Count | Requirement range | Count |
|---|---:|---:|---:|---:|
| [Acronyms.md](Acronyms.md) | `INV-ACR-001–005` | 5 | `REQ-ACR-001–006` | 6 |
| [01-Philosophy.md](01-Philosophy.md) | `INV-PHIL-001–014` | 14 | `REQ-PHIL-001–016` | 16 |
| [02-Executive-Summary.md](02-Executive-Summary.md) | `INV-EXEC-001–014` | 14 | `REQ-EXEC-001–020` | 20 |
| [03-Problem-Statement.md](03-Problem-Statement.md) | `INV-PROB-001–016` | 16 | `REQ-PROB-001–022` | 22 |
| [04-Design-Principles.md](04-Design-Principles.md) | `INV-DESIGN-001–020` | 20 | `REQ-DESIGN-001–022` | 22 |
| [05-System-Overview.md](05-System-Overview.md) | `INV-SYS-001–020` | 20 | `REQ-SYS-001–022` | 22 |
| [06-Protocol-Objects.md](06-Protocol-Objects.md) | `INV-OBJ-001–022` | 22 | `REQ-OBJ-001–026` | 26 |
| [07-Evidence-Record-Specification.md](07-Evidence-Record-Specification.md) | `INV-ER-001–026` | 26 | `REQ-ER-001–035` | 35 |
| [08-Hash-Chain-Specification.md](08-Hash-Chain-Specification.md) | `INV-CHAIN-001–028` | 28 | `REQ-CHAIN-001–042` | 42 |
| [09-Witness-Observation-Specification.md](09-Witness-Observation-Specification.md) | `INV-WIT-001–030` | 30 | `REQ-WIT-001–049` | 49 |
| [10-Checkpoints-and-External-Anchors.md](10-Checkpoints-and-External-Anchors.md) | `INV-CP-001–032` | 32 | `REQ-CP-001–051` | 51 |
| [11-Key-Transparency.md](11-Key-Transparency.md) | `INV-KT-001–032` | 32 | `REQ-KT-001–051` | 51 |
| [12-Preservation.md](12-Preservation.md) | `INV-PRES-001–032` | 32 | `REQ-PRES-001–062` | 62 |
| [13-Dispute-Packs.md](13-Dispute-Packs.md) | `INV-DP-001–032` | 32 | `REQ-DP-001–062` | 62 |
| [14-Verification.md](14-Verification.md) | `INV-VER-001–032` | 32 | `REQ-VER-001–062` | 62 |
| [15-Trust-Profiles.md](15-Trust-Profiles.md) | `INV-TP-001–032` | 32 | `REQ-TP-001–062` | 62 |
| [16-Protocol-APIs-and-SDK-Boundaries.md](16-Protocol-APIs-and-SDK-Boundaries.md) | `INV-API-001–032` | 32 | `REQ-API-001–062` | 62 |
| [17-TAIP-Mapping-and-Normative-Specification-Boundary.md](17-TAIP-Mapping-and-Normative-Specification-Boundary.md) | `INV-MAP-001–032` | 32 | `REQ-MAP-001–062` | 62 |
| [18-Governance-Versioning-and-Compatibility.md](18-Governance-Versioning-and-Compatibility.md) | `INV-GOV-001–032` | 32 | `REQ-GOV-001–062` | 62 |
| **Total indexed local identifiers** |  | **483** |  | **796** |

Service documents without local `INV-*` or `REQ-*` entries remain normative or informative according to [Document-Status.md](Document-Status.md) and may still be cited as terminology, scope, or authority dependencies.

## Ownership Principles

Ownership means responsibility for maintaining a requirement's normative allocation and traceability. It does not permit the owner to contradict source chapters or change a global entry unilaterally.

Every global requirement should have:

- one primary architectural owner;
- zero or more contributing owners;
- one governance Authority;
- one TAIP allocation owner when protocol work exists;
- one conformance owner when tests exist;
- a lifecycle state;
- a review milestone;
- issue, RFC, ADR, or release references where applicable.

## Global Ownership Matrix

| Global range | Primary architectural owner | Principal contributing chapters | Expected normative owner |
|---|---|---|---|
| `GREQ-001–012` | Protocol Objects and Evidence Records | Philosophy, Executive Summary, Problem Statement, Design Principles, System Overview | TAIP Core and object modules |
| `GREQ-013–025` | Identity, Authority, and Key Transparency | System Overview, Protocol Objects, Evidence Records, Verification | TAIP identity, Authority, and cryptography modules |
| `GREQ-026–030` | Protocol Object lifecycle | Evidence Records, Hash Chains, APIs, Governance | TAIP Core lifecycle specification |
| `GREQ-031–042` | Hash Chain specification | Protocol Objects, Preservation, Verification | TAIP Hash Chain module |
| `GREQ-043–049` | Witness specification | Trust Profiles, Verification, System Overview | TAIP Witness and quorum module |
| `GREQ-050–054` | Checkpoint and Anchor specification | Witness, Hash Chain, Verification | TAIP Checkpoint and Anchor module |
| `GREQ-055–058` | Key Transparency specification | Identity, Verification, Preservation | TAIP Key Transparency module |
| `GREQ-059–066` | Preservation specification | Protocol Objects, Key Transparency, Governance | TAIP Preservation module and profiles |
| `GREQ-067–074` | Dispute Pack specification | Preservation, Verification, APIs | TAIP Dispute Pack and package-binding modules |
| `GREQ-075–080` | Verification specification | Every evidence-producing domain | TAIP Verification and report module |
| `GREQ-081–091` | Trust Profile specification | Witness, Preservation, Verification, Governance | TAIP profile model and Profile Authorities |
| `GREQ-092–102` | API and SDK boundary specification | Protocol Objects, lifecycle modules, Verification | TAIP binding specifications |
| `GREQ-103–107` | TAIP mapping and conformance governance | All chapters | TAIP specification and conformance Authorities |
| `GREQ-108–115` | Governance, versioning, and compatibility | All governed artifacts | Governance Authority and delegated artifact Authorities |

## Mapping Record Model

A machine-readable local-to-global mapping record should contain:

```text
mapping_id
mapping_version
source_document
source_version
source_identifier
global_identifier
relationship
applicability
roles
trust_profiles
taip_targets
test_identifiers
status
owner
rationale
introduced_in
deprecated_in
superseded_by
```

The mapping record has its own identity and version. Updating a mapping does not modify either linked requirement.

## Lifecycle States

Global entries may use states such as:

- proposed;
- active;
- partially allocated;
- fully allocated;
- partially covered;
- fully covered;
- deprecated;
- superseded;
- archived.

Allocation and coverage are separate.

```text
Requirement exists
      ≠
Normative TAIP text exists
      ≠
Conformance tests exist
      ≠
Every implementation conforms
```

## Duplicate and Overlap Handling

Repeated chapter-local obligations are expected because each chapter applies a durable property to a different object or trust boundary.

Global consolidation must not discard meaningful scope.

For example, `REQ-PHIL-004`, `REQ-ER-020`, `REQ-CHAIN-030`, `REQ-PRES-048`, and `REQ-GOV-058` all protect non-rewrite semantics but apply to different transitions. Their relationship to `GINV-006` and the relevant `GREQ-*` entries is convergence, not redundancy that permits deletion.

## Conflict Handling

If a local source and global entry appear to conflict:

1. preserve both texts and versions;
2. identify source authority and applicability;
3. classify the relationship and semantic difference;
4. open a governed issue, RFC, erratum, or interpretation;
5. avoid undocumented implementation preference;
6. mark affected mappings and tests as disputed or blocked;
7. publish the governed resolution without rewriting historical releases.

---

# 19.4 Trust Profile Mapping

Global requirements identify what TrustAgentAI must protect. Trust Profiles identify which controls and evidence are mandatory for a particular assurance objective.

## Profile-Mapping Record

A profile mapping should identify:

```text
profile_id
profile_version
global_requirement
control_id
criticality
applicability_predicate
required_evidence
evaluation_rule
failure_effect
degradation_rule
test_identifiers
```

The mapping must bind exact profile and requirement versions.

## Control-Family Matrix

| Control family | Global requirements | Typical profile question | Representative evidence |
|---|---|---|---|
| Evidence creation | `GREQ-001–012` | Was sufficient structured evidence created and protected? | Evidence Records, canonical bytes, Signatures, dependency manifests |
| Identity and Authority | `GREQ-013–025` | Were identity, key, purpose, Authority, and historical state adequately established? | Identity records, KTRs, Authority and Policy evidence |
| Lifecycle and Commitment | `GREQ-026–042` | Did evidence enter governed immutable history with required continuity? | Chain Entries, receipts, proofs, conflict and migration evidence |
| Independent observation | `GREQ-043–049` | Were required Witness classes, independence, timing, and quorum satisfied? | Witness Observations, eligibility and Control Domain evidence |
| Historical boundaries | `GREQ-050–058` | Were required Checkpoints, anchors, key history, and finality controls satisfied? | Checkpoints, Anchor Evidence, KT proofs and snapshots |
| Long-term preservation | `GREQ-059–066` | Can evidence and dependencies remain verifiable for the required lifetime? | Preservation Evidence, manifests, recovery and migration records |
| Portable dispute review | `GREQ-067–074` | Can an independent reviewer inspect the bounded claim and its omissions safely? | Dispute Pack Manifest, entries, omission and disclosure states |
| Verification | `GREQ-075–080` | Were all mandatory checks performed under the correct context with explicit outcomes? | Verification Context, check results, Verification Report |
| Profile semantics | `GREQ-081–091` | Is the profile itself governed and was achievement evidence-based? | Profile definition, Registry snapshot, control results |
| Binding behavior | `GREQ-092–102` | Did interfaces preserve protocol meaning, failures, versions, and isolation? | Requests, receipts, capability documents, exports, API test reports |
| Specification accountability | `GREQ-103–115` | Are rules, tests, releases, transitions, and historical dependencies accountable? | Mapping records, RFCs, ADRs, release manifests, conformance reports |

## Reference Profile Family

The reference profile family in [15-Trust-Profiles.md](15-Trust-Profiles.md) is cumulative in assurance intent while allowing incomparability where domain-specific controls differ.

The following is an architectural allocation guide, not a substitute for the exact profile definitions:

| Reference profile | Global emphasis | Minimum architectural interpretation |
|---|---|---|
| TP0 — Explicit Evidence Boundary | `GREQ-001–012`, `GREQ-026–030`, `GREQ-075–080` | Evidence and uncertainty are structured, bounded, and reportable |
| TP1 — Cryptographically Attributable Evidence | TP0 plus `GREQ-013–025` | Identity, protected input, Signature, key, purpose, and Authority can be evaluated |
| TP2 — Verifiable Historical Continuity | TP1 plus `GREQ-031–042`, `GREQ-055–066` | Ordered Commitment, historical keys, dependencies, and Preservation support later Verification |
| TP3 — Independently Observed Accountability | TP2 plus `GREQ-043–054` | Independent Witness, quorum, Checkpoint, and Anchor controls are evaluated |
| TP4 — Multi-Domain Durable Assurance | TP3 plus strengthened `GREQ-059–074`, `GREQ-081–115` | Multiple control domains, portability, governance continuity, and long-lived assurance are evidenced |

A higher profile label must not be inferred from the number of implemented global requirements. Exact profile controls, criticality, composition, evidence, and exceptions govern achievement.

## Mandatory, Conditional, and Informational Use

A profile may classify a global requirement as:

- mandatory for all evidence in scope;
- mandatory for one object, role, action, jurisdiction, or risk condition;
- conditional under a deterministic predicate;
- satisfied by an identified lower-layer control;
- not applicable with explicit rationale;
- outside the profile's claim boundary.

An informational citation does not satisfy a mandatory control.

## Profile Exception Rules

An exception must identify:

- affected `GREQ-*` and control IDs;
- authorizing Policy and Authority;
- applicable scope and time;
- compensating control;
- evidence;
- effect on `GINV-*` properties;
- Achieved Trust Profile consequence;
- expiry and review behavior.

No exception may silently convert an unsatisfied mandatory global requirement into full achievement.

## Profile Change Impact

When a `GREQ-*` entry changes, every referencing profile must be reviewed for:

- compatibility;
- control wording;
- evidence sufficiency;
- thresholds and criticality;
- migration or reassessment;
- test coverage;
- historical interpretation.

The profile may retain an older global-index version when necessary for historical evidence, but the bound version must remain explicit.

---

# 19.5 TAIP Allocation Matrix

The Project Bible states architectural obligations. TAIP and governed supporting artifacts define exact interoperable behavior.

## Allocation Outcomes

Every `GREQ-*` entry should have one or more explicit dispositions:

- **Core** — mandatory common TAIP semantics;
- **Module** — normative object or mechanism specification;
- **Profile** — assurance selection, threshold, or deployment control;
- **Registry** — governed value, namespace, status, or allocation policy;
- **Schema** — machine-enforceable structural constraint;
- **Canonicalization** — deterministic cryptographic-input rule;
- **Binding** — transport, operation, representation, and error mapping;
- **Conformance** — test assertion, vector, suite, or report requirement;
- **Governance** — Authority, release, compatibility, or lifecycle process;
- **Deployment** — governed operational obligation outside protocol interoperability;
- **Informative** — rationale or guidance without independent conformance force;
- **Outside Scope** — explicit non-TAIP responsibility;
- **Deferred** — identified unresolved allocation with owner and milestone.

Silence is not an allocation.

## TAIP Allocation Record

A normative allocation record should contain:

```text
allocation_id
global_requirement
index_version
disposition
normative_artifact
artifact_version
normative_location
responsible_role
applicable_profiles
schema_or_registry
binding
test_identifiers
security_impact
privacy_impact
compatibility_status
lifecycle_status
owner
rationale
```

## Allocation Matrix

| Global range | Primary TAIP allocation | Supporting artifacts | Principal conformance target |
|---|---|---|---|
| `GREQ-001–012` | TAIP Core object and evidence model | Object Registry, schemas, canonicalization | Object producer, parser, validator |
| `GREQ-013–025` | Identity, Authority, Signature, and cryptography modules | Algorithm and key-purpose Registries | Signer, resolver, Verification Engine |
| `GREQ-026–030` | TAIP Core lifecycle model | Lifecycle Registry and evidence schemas | Producer, Registry, Commitment Service |
| `GREQ-031–042` | Hash Chain module | Chain schemas, proof formats, algorithm Registry | Chain operator and verifier |
| `GREQ-043–049` | Witness and quorum module | Witness Registry, profile controls | Witness, quorum evaluator, verifier |
| `GREQ-050–054` | Checkpoint and External Anchor module | Anchor binding Registry and proof formats | Checkpoint Authority, anchor resolver, verifier |
| `GREQ-055–058` | Key Transparency module | KT Registry, log and proof formats | KT service, monitor, historical resolver |
| `GREQ-059–066` | Preservation module | Preservation profiles, manifests, custody and migration schemas | Preservation Service and verifier |
| `GREQ-067–074` | Dispute Pack module | Package bindings, media types, manifest schema | Assembler, parser, verifier |
| `GREQ-075–080` | Verification module | Outcome Registry, report schema, resolver interface | Verification Engine |
| `GREQ-081–091` | Trust Profile model | Profile Registry, control schemas, mapping records | Profile Authority and evaluator |
| `GREQ-092–102` | API and transport bindings | Capability schema, media types, SDK contracts | Service, client, SDK, gateway |
| `GREQ-103–107` | TAIP specification and conformance process | Traceability index, test Registry | Specification and Conformance Authorities |
| `GREQ-108–115` | Governance and release process | RFCs, ADRs, manifests, advisories, archives | Governance and artifact Authorities |

## Core and Profile Boundary

Core should define the semantic minimum required to prevent incompatible interpretation.

Profiles may:

- require particular optional modules;
- select algorithms and parameters;
- define Witness eligibility and quorum;
- define timing and Preservation controls;
- require stricter privacy or disclosure behavior;
- prohibit otherwise supported mechanisms;
- define degradation and reassessment.

Profiles must not redefine existing Core identifiers, lifecycle states, outcome semantics, canonicalization, or historical meaning incompatibly.

## Schema and Semantic Boundary

Schemas may express:

- required members;
- data types;
- structural choices;
- syntactic formats;
- simple enumerations;
- structural cardinality.

Schemas do not independently establish:

- Authority;
- historical applicability;
- Signature validity;
- Commitment;
- Witness independence;
- Completeness;
- Trust Profile achievement;
- business truth.

Every schema allocation should identify which part of a `GREQ-*` entry it enforces and which semantic checks remain elsewhere.

## Registry Allocation

Registry allocations should specify:

- governing `GREQ-*` and TAIP rule;
- namespace and Authority;
- entry identity and version;
- permitted semantic scope;
- criticality and unknown-value behavior;
- lifecycle and deprecation;
- compatibility impact;
- integrity and historical snapshots;
- associated tests.

A Registry entry cannot broaden its authority beyond the governing specification.

## Binding Allocation

Binding specifications should map:

- logical operation;
- request and response semantics;
- transport statuses;
- asynchronous and batch behavior;
- errors and warnings;
- media types and representations;
- authentication and authorization boundary;
- version negotiation;
- capability discovery;
- privacy and security behavior;
- offline and failure behavior.

The mapping must cite `GREQ-092–GREQ-102` and every domain requirement whose behavior the binding exposes.

## Allocation Completeness

A TAIP release is not architecturally complete merely because every global requirement has a row.

Each applicable allocation must be:

- normative or explicitly non-normative;
- sufficiently precise for independent implementation;
- linked to exact versions;
- reviewed for security and privacy;
- tested or accompanied by a justified coverage plan;
- compatible or accompanied by migration behavior;
- preserved for historical interpretation.

## Unresolved and Deferred Allocations

A deferred allocation should identify:

- affected `GINV-*` and `GREQ-*` identifiers;
- reason for deferral;
- prohibited assumptions while unresolved;
- interoperability, security, privacy, and conformance risk;
- interim failure behavior;
- owner and target milestone.

An unresolved mandatory semantic prevents an unqualified conformance claim for the affected feature.

---

# 19.6 Verification Evidence Mapping

Global requirements become useful in Verification only when evaluators can identify the evidence, dependencies, checks, and outcomes that demonstrate them.

## Verification-Evidence Mapping Record

A mapping should contain:

```text
verification_mapping_id
global_requirement
global_invariants
applicable_claims
applicable_roles
verification_layer
required_evidence_types
required_dependencies
check_identifiers
success_condition
failure_conditions
completeness_effect
profile_effect
report_fields
versions
```

## Evidence-Family Matrix

| Evidence family | Principal global requirements | Verification focus | Representative output |
|---|---|---|---|
| Evidence Record | `GREQ-001–012`, `GREQ-026–030` | type, identity, semantics, producer, protected scope, references, lifecycle | object and claim check results |
| Identity and Authority | `GREQ-013–025` | identity, key, purpose, Authority, delegation, historical policy | identity, Signature, Authority outcomes |
| Hash Chain | `GREQ-031–042` | genesis, transition, linkage, order, conflict, proof, range | Chain and Commitment outcomes |
| Witness | `GREQ-043–049` | target, scope, result, eligibility, independence, timing, quorum | observation and quorum outcomes |
| Checkpoint and Anchor | `GREQ-050–054` | target state, Authority, cadence, publication, finality, proof scope | Checkpoint and anchor outcomes |
| Key Transparency | `GREQ-055–058` | lifecycle, ordering, proof, conflict, split view, historical resolution | Historical Key State result |
| Preservation | `GREQ-059–066` | target, lifetime, dependencies, fixity, custody, recovery, migration, disposition | Preservation and availability outcomes |
| Dispute Pack | `GREQ-067–074` | Manifest, membership, integrity, omissions, disclosure, package safety | package and Completeness outcomes |
| Verification | `GREQ-075–080` | context, layered checks, outcome composition, reporting | Verification Report |
| Trust Profile | `GREQ-081–091` | profile Authority, controls, dependencies, evidence, composition, downgrade | Intended and Achieved Trust Profiles |
| API and binding evidence | `GREQ-092–102` | operation, state, receipt, error, negotiation, capabilities, isolation | binding and service conformance evidence |
| Governance evidence | `GREQ-103–115` | mapping, Authority, release, version, compatibility, transition, preservation | traceability, release, and conformance records |

## Verification Layers

Global requirement checks should be assigned to explicit layers:

1. availability and safe processing;
2. parsing and structure;
3. semantic validity;
4. identifier and canonicalization;
5. cryptographic integrity;
6. identity, Key Purpose, and historical key state;
7. Authority and Policy;
8. lifecycle and historical Commitment;
9. Witness, quorum, Checkpoint, and Anchor assurance;
10. Preservation and dependency availability;
11. Completeness and conflict evaluation;
12. Trust Profile control evaluation;
13. aggregate bounded protocol conclusion;
14. human-readable explanation consistent with canonical results.

One successful layer must not hide an unevaluated or failed mandatory layer.

## Evidence Sufficiency

A mapping should state whether the required evidence is:

- focal;
- supporting;
- dependency;
- contextual;
- negative or conflict evidence;
- governance evidence;
- external but resolvable;
- optional corroboration.

Possession of an object type commonly associated with a requirement does not prove the requirement is satisfied. The object's validity, scope, historical applicability, and relationship to the claim must be evaluated.

## Outcome Mapping

Every check should define how relevant states map into canonical outcomes.

| Condition | Typical bounded outcome |
|---|---|
| Applicable evidence understood and satisfies the rule | Valid or satisfied for the check |
| Understood available evidence contradicts the rule | Invalid or failed |
| Required evidence is known to be missing or insufficient | Incomplete |
| Evidence supports competing unresolved conclusions | Conflicting or Indeterminate |
| Mandatory semantic or algorithm is not implemented | Unsupported |
| Required evidence or service cannot be accessed | Unavailable |
| Rule does not apply to the evaluated claim | Not Applicable |
| Check was intentionally outside evaluated scope | Not Evaluated |

The exact outcome remains governed by the applicable TAIP Verification specification and profile.

## Example Mapping

For `GREQ-048 — Deterministic Quorum Evaluation`:

```text
Required evidence:
  Witness Observations
  Witness Registry snapshot
  historical eligibility state
  Control Domain evidence
  quorum policy and version
  timing evidence

Checks:
  observation validity
  target and scope compatibility
  eligibility
  independence
  duplicate and conflict handling
  threshold calculation

Outputs:
  per-observation results
  contributing and excluded sets
  quorum calculation
  completeness state
  profile-control result
```

The mapping does not turn raw Signature count into quorum evidence.

## Reverification Triggers

Mappings should identify events that require or recommend reverification, including:

- new or corrected evidence;
- changed Verification Context;
- restored or lost dependency;
- new conflict or revocation;
- updated historical key evidence;
- profile or Policy change;
- algorithm or security advisory;
- migration or cryptographic renewal;
- new normative interpretation;
- Verification Engine or test-suite correction.

Reverification creates a new report. It does not silently rewrite the prior report.

## Verification Report Traceability

A Verification Report should permit navigation from:

```text
Report outcome
    → check identifier
    → GREQ identifier
    → GINV property
    → chapter-local source
    → TAIP rule and version
    → evaluated evidence and dependency
```

This path explains both what the verifier concluded and why the rule exists.

---

# 19.7 Conformance Coverage

Conformance coverage shows whether a normative obligation can be evaluated reproducibly. It does not prove that every implementation conforms or that every deployment achieves a Trust Profile.

## Test Identifier Families

TrustAgentAI conformance artifacts may use stable test families such as:

```text
TST-STRUCT-*
TST-CANON-*
TST-CRYPTO-*
TST-CHAIN-*
TST-WITNESS-*
TST-CHECKPOINT-*
TST-KEYS-*
TST-PRESERVE-*
TST-PACK-*
TST-VERIFY-*
TST-PROFILE-*
TST-API-*
TST-GOV-*
```

Test identifiers must remain stable within their governed namespace and version history.

## Coverage Record

A coverage record should identify:

```text
coverage_id
global_requirement
normative_rule
specification_version
target_role
feature_or_profile
test_id
test_version
coverage_type
required_or_optional
environment
expected_outcome
implementation_result
status
known_gap
owner
```

## Coverage Types

Applicable coverage should include:

- positive success;
- negative understood failure;
- malformed input;
- missing required input;
- unsupported mandatory semantics;
- unavailable dependency;
- conflicting evidence;
- boundary and threshold behavior;
- historical-version behavior;
- cross-version compatibility;
- extension and unknown-value behavior;
- migration and deprecation;
- downgrade resistance;
- replay, duplicate, concurrency, and retry behavior;
- resource exhaustion and safe termination;
- privacy and redaction effects;
- cross-implementation equivalence.

One happy-path test is not meaningful coverage of a security- or compatibility-critical rule.

## Global Coverage Matrix

| Global range | Principal test families | Critical negative coverage |
|---|---|---|
| `GREQ-001–012` | `TST-STRUCT-*`, `TST-CANON-*` | unknown types, collisions, ambiguous canonicalization, missing dependencies |
| `GREQ-013–025` | `TST-CRYPTO-*`, `TST-KEYS-*` | wrong purpose, unauthorized signer, revoked key, historical mismatch, unsupported algorithm |
| `GREQ-026–030` | `TST-STRUCT-*`, lifecycle suites | in-place mutation, state conflation, missing transition evidence |
| `GREQ-031–042` | `TST-CHAIN-*` | fork, rollback, gap, conflict, invalid proof, partition, historical mismatch |
| `GREQ-043–049` | `TST-WITNESS-*` | ineligible Witness, shared control, incompatible scope, failed quorum, silence/conflict |
| `GREQ-050–054` | `TST-CHECKPOINT-*` | wrong target, stale cadence, invalid Authority, reorganization, false finality |
| `GREQ-055–058` | `TST-KEYS-*` | split view, wrong boundary, predecessor mismatch, missing history |
| `GREQ-059–066` | `TST-PRESERVE-*` | corruption, restore failure, lost key, hold conflict, partial deletion, lossy migration |
| `GREQ-067–074` | `TST-PACK-*` | path traversal, expansion bomb, omission, redaction ambiguity, conflicting entry |
| `GREQ-075–080` | `TST-VERIFY-*` | wrong context, outcome collapse, dependency substitution, report rewrite |
| `GREQ-081–091` | `TST-PROFILE-*` | missing mandatory control, invalid fallback, inheritance conflict, private semantics |
| `GREQ-092–102` | `TST-API-*` | status conflation, unsafe retry, silent downgrade, tenant leak, pagination gap |
| `GREQ-103–115` | traceability and `TST-GOV-*` | unmapped rule, unauthorized release, incompatible erratum, missing historical artifact |

## Coverage Status

Useful coverage states include:

- not allocated;
- planned;
- specified;
- implemented;
- independently implemented;
- passing;
- failing;
- blocked;
- partial;
- deprecated;
- superseded.

The status should be scoped to an exact requirement, normative version, role, feature set, suite, and environment.

## Pass, Fail, Skip, and Not Applicable

Conformance reports must distinguish:

- **pass** — the implementation produced the expected result for the test;
- **fail** — observed behavior contradicted the expected result;
- **unsupported** — the implementation does not implement a required tested semantic;
- **skipped** — the test was not executed;
- **not applicable** — the governed applicability rule excludes the test from scope;
- **blocked** — a prerequisite or environment prevented evaluation;
- **indeterminate** — available evidence cannot support a reliable pass or fail.

A skip is not a pass. An unsupported mandatory feature is not a harmless exclusion. A not-applicable result requires the applicability basis.

## Conformance Claim Scope

A conformance claim should identify:

- implementation and build;
- implemented role;
- TAIP Core and module versions;
- profiles, bindings, representations, and extensions;
- algorithms and optional features;
- suite and test-vector versions;
- environment and configuration;
- executed, skipped, failed, and unsupported tests;
- deviations and known gaps;
- report identity, integrity, and time.

```text
Parser conformance
≠ Producer conformance
≠ Verification Engine conformance
≠ Deployment conformance
≠ Trust Profile achievement for evidence
```

## Release Gates

A normative release should not claim full readiness for an applicable global requirement unless:

- the TAIP allocation is complete;
- source and global traceability are reviewed;
- mandatory behavior is testable;
- positive and critical negative tests exist;
- unsupported and failure outcomes are defined;
- cross-version and migration behavior is covered where applicable;
- security and privacy impacts are tested or explicitly governed;
- conformance artifacts and historical dependencies are preserved.

Experimental releases may publish known gaps when the affected scope and non-stable status are explicit.

## Index Self-Validation

The Global Index should itself be validated for:

- unique and sequential `GINV-*` and `GREQ-*` identifiers;
- stable titles and statements;
- every GINV having at least one implementing GREQ;
- every GREQ protecting at least one GINV;
- valid local source prefixes and ranges;
- valid chapter and service-document links;
- no incompatible identifier reuse;
- no missing ownership for active entries;
- no TAIP or test allocation without an exact version;
- preserved historical index releases.

Machine-generated validation can detect structural defects. Human review remains necessary for semantic adequacy and incorrect mappings.

## Coverage Does Not Replace Judgment

A passing suite demonstrates bounded observed behavior against the tested rules. It does not prove:

- absence of implementation defects outside tested cases;
- security against every threat;
- legal or regulatory compliance;
- correct deployment configuration;
- truth of evidence contents;
- achievement of a Trust Profile for every evidence set.

The conformance result must remain within its declared protocol scope.

---

# Security Considerations

The Global Index is security-relevant because implementers, reviewers, profiles, and conformance systems may use it to determine which obligations exist and where they are enforced.

## Identifier Reassignment

Reusing a `GINV-*`, `GREQ-*`, local requirement, or test identifier for incompatible meaning can make older reports and mappings appear to validate a different rule.

Published identifier meaning must remain immutable. Corrections require versioned records; incompatible replacement requires a new identifier or explicit supersession relationship.

## Mapping Omission

An attacker or captured process may omit a difficult local requirement from global mapping and then claim it no longer applies.

The index explicitly prevents that interpretation. Chapter-local obligations remain authoritative even when representative mappings omit them. Release validation should compare the source inventory with exhaustive machine-readable mapping records.

## Over-Consolidation

Combining similar local requirements too aggressively can erase object-specific failure semantics.

For example, append-only correction for an Evidence Record, a Hash Chain, a Verification Report, and a governance release protects one global property but uses different evidence and validation. Mapping must preserve each refining rule.

## False Coverage

A row linking a requirement to a test does not prove meaningful coverage. Tests can be stale, implementation-specific, incorrectly expected, skipped, or limited to success behavior.

Coverage records should bind exact versions, roles, expected outcomes, negative cases, execution results, and known gaps.

## Source-Precedence Confusion

Global summaries may be easier to read than detailed chapters and therefore risk becoming accidental substitutes.

The global statement controls only its cross-domain architectural meaning. Concrete protocol behavior comes from the applicable normative TAIP set, and detailed Project Bible scope remains in the cited source chapters.

## Index Substitution and Rollback

A malicious mirror may serve an older or modified index whose mappings omit later security rules.

Index releases should have stable versions, content integrity, authorized manifests, preserved history, and dependency relationships. Consumers should bind the exact index version used by a profile, specification, report, or conformance suite.

## Dependency and Link Attacks

Mutable links can resolve to changed specifications, Registries, or tests. Human-readable link text can conceal a different target.

Normative releases and historical Verification should use durable identifiers, exact versions, and integrity references in addition to navigational links.

## Automated-Generation Risk

Generated indexes can inherit parser defects, omit unusual Markdown, misread identifier ranges, or publish unreviewed mappings.

Generation pipelines should be versioned, reviewed, reproducible where practical, and validated against source counts. Generated changes affecting meaning require human approval.

## Governance Capture

Control of the global catalog could allow one authority to reshape architecture across every domain.

Changes require the governance, conflict, review, versioning, and preservation controls in Chapter 18. The index owner cannot override chapter or TAIP Authority boundaries unilaterally.

## Downgrade Through Partial Implementation

An implementation may cite only global requirements it supports and present that subset as TrustAgentAI conformance.

Conformance claims must identify the complete applicable requirement set, unsupported mandatory entries, profile, role, features, versions, and suite. Percentages and badges must not hide mandatory gaps.

## Mapping Cycles

Traceability relationships can become circular if a requirement is justified only by a test that derives its expectation from the same implementation, or if two profiles claim each other as their sole assurance basis.

Mappings must ultimately terminate in authoritative architecture, normative rules, independently specified expected behavior, and evidence.

## Stale Security Disposition

Security advisories, algorithm deprecations, profile changes, and normative interpretations may change current acceptance without changing historical meaning.

Coverage and allocation records must identify effective boundaries and distinguish historical Verification from authorization of new use.

---

# Privacy Considerations

The Global Index primarily contains identifiers, requirements, and mappings rather than operational evidence. It can still expose sensitive organizational and implementation information when extended with owners, test results, deployments, conflicts, or incident records.

## Owner and Contributor Data

Ownership records should use role identities or organizational contacts where personal identity is unnecessary. Public mapping history should not expose private contact information, credentials, or unrelated employment details.

Conflict-of-interest records may require restricted details while publishing sufficient recusal and decision evidence.

## Implementation Fingerprinting

Detailed coverage reports can reveal unsupported algorithms, missing controls, software versions, deployment limits, or unpatched behavior.

Public conformance claims should disclose enough for honest scope while protecting credentials, exploit details, internal topology, and customer information. Embargoed security gaps require bounded handling.

## Test Data

Conformance mappings should reference synthetic or minimized fixtures where practical. Test vectors and failure reports must not copy real sensitive evidence merely to demonstrate traceability.

## Evidence References

Verification mappings should prefer stable identifiers, digests, protected references, and evidence classes rather than embedding sensitive payloads in the index.

Access-controlled evidence may still satisfy a requirement when the applicable profile defines authorized Verification and the resulting limitation remains explicit.

## Resolver and Retrieval Metadata

Mapping and Verification systems may record queries, resolver choices, timestamps, cache state, and failure history. These records can reveal investigative interests or relationships among identities and evidence.

Collection and retention should be proportionate to reproducibility needs, with access controls and minimization.

## Public Historical Archives

Preserving index and governance history is necessary for accountability but can make contributor and organizational metadata permanently searchable.

Archives should retain decision provenance and Authority evidence while minimizing incidental personal data. Redactions for safety or legal obligations must remain attributable and must not falsify the normative history.

## Profile and Jurisdictional Inference

Mapping a deployment or evidence set to a Trust Profile can reveal risk level, regulatory environment, business sensitivity, or operational design.

The canonical global index need not identify individual deployments. Deployment-specific mappings may be access-controlled while their resulting claims remain properly scoped.

## Completeness and Privacy

Privacy filtering must not make omitted mapping records or unavailable evidence appear complete.

When protected information prevents full evaluation, the coverage, Completeness, or conformance outcome must identify the limitation without disclosing the protected content itself.

---

# Design Rationale

The Project Bible intentionally repeats core distinctions across chapters.

Identity/key separation appears in object, Witness, Checkpoint, Key Transparency, Verification, API, and governance contexts. Validity/Completeness separation appears in evidence, Dispute Pack, Verification, and Trust Profile contexts. No-silent-downgrade rules appear in profiles, APIs, compatibility, and outcome handling.

This repetition is necessary because the detailed failure mode differs by domain. It also creates a traceability problem: reviewers need a stable way to see that several local rules protect the same system-wide property.

The global catalog solves that problem without flattening the architecture.

```text
Local rule preserves exact domain meaning.
Global identifier preserves cross-domain intent.
TAIP allocation creates interoperable behavior.
Test mapping creates executable evidence.
Verification mapping explains the result.
```

Thirty-six global invariants are broad enough to describe durable architecture and specific enough to prevent vague “trust” claims. They cover identity, integrity, history, independence, Completeness, Verification, assurance, lifecycle, portability, privacy, evolution, governance, and conformance.

The 115 global requirements decompose those properties into implementable responsibilities. They are deliberately cross-domain: one requirement can receive allocations in Core, a module, a profile, a Registry, a binding, and conformance tests.

Bidirectional traceability prevents two failures.

First, an invariant cannot remain inspirational prose with no implementing obligation. Its **Implemented by** links identify the requirements intended to keep it true.

Second, a requirement cannot become arbitrary mechanism. Its **Protects** links explain the durable property that justifies it.

Representative local mappings keep the human-readable chapter usable. Exhaustive mappings belong in a machine-readable companion because 483 local invariants and 796 local requirements create many-to-many relationships that will evolve independently of prose layout.

The ownership, profile, TAIP, Verification, and conformance matrices turn the catalog into a lifecycle model rather than a glossary. A global requirement can be architecturally active while still awaiting TAIP allocation or test coverage. That gap remains visible instead of being mistaken for implementation freedom.

Finally, stable global identifiers support long-term reports and evidence. A future verifier can identify not only the exact rule version applied, but the enduring architectural property the rule protected.

---

# Summary

The Global Invariant and Requirement Index provides the canonical cross-domain traceability layer for TrustAgentAI.

It defines:

- `GINV-001–GINV-036` as the durable global architectural properties;
- `GREQ-001–GREQ-115` as the cross-domain obligations that protect those properties;
- bidirectional GINV-to-GREQ relationships;
- representative mappings to 483 local invariants and 796 local requirements;
- Equivalent, Refines, Supports, Partially Overlaps, and Superseded By mapping semantics;
- ownership, lifecycle, Trust Profile, TAIP allocation, Verification Evidence, and conformance coverage models;
- identifier stability, conflict handling, coverage rules, and index self-validation.

The governing relationship is:

```text
Project Bible Source
      ▼
Local INV-* and REQ-*
      ▼
Global GINV-* and GREQ-*
      ▼
TAIP, Profiles, Registries, Schemas, and Bindings
      ▼
Tests, Verification Evidence, and Conformance Reports
```

The index does not erase detail.

It makes detail navigable.

It does not turn architecture into a checklist score.

It makes every checklist item traceable to architectural purpose.

It does not prove conformance.

It defines what conformance evidence must be able to explain.

With stable identifiers and bidirectional traceability, TrustAgentAI can evolve without losing the connection between principle, protocol rule, implementation behavior, and independently verifiable result.

The final chapter returns to the central conclusion: durable accountability requires **Proof, not logs**.
