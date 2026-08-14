# TrustAgentAI Project Bible — Acronyms

## Purpose

This document defines the canonical acronyms and abbreviations used throughout the **TrustAgentAI Project Bible**, the **TrustAgentAI Interoperability Protocol (TAIP)**, Trust Profiles, schemas, registries, test vectors, conformance materials, reference bindings, and related project documentation.

Its purpose is to maintain consistent terminology across TrustAgentAI specifications and implementations.

Where practical, an acronym SHOULD be expanded on first use within a standalone document.

---

# Core Project Acronyms

## TAIP

**TAIP — TrustAgentAI Interoperability Protocol**

TAIP is the normative interoperability protocol derived from the TrustAgentAI architecture.

TAIP defines interoperable behavior including:

- Protocol Objects;
- canonicalization;
- identifiers;
- cryptographic inputs;
- signatures;
- evidence lifecycle;
- Hash Chain semantics;
- Witness Observations;
- Checkpoints;
- Key Transparency;
- verification outcomes;
- extension behavior;
- compatibility rules.

The Project Bible defines architectural intent.

TAIP defines normative interoperable behavior.

---

## TP

**TP — Trust Profile**

A Trust Profile defines a versioned set of assurance requirements.

Trust Profiles may specify requirements for:

- cryptographic protection;
- key custody;
- Witnesses;
- Witness independence;
- Witness quorum;
- Checkpoint cadence;
- external anchoring;
- Preservation;
- verification dependencies.

Standard Trust Profiles may use identifiers such as:

```text
TP0
TP1
TP2
TP3
TP4
```

The exact semantics of each Trust Profile are defined by the applicable Trust Profile specification.

---

# Evidence and Protocol Acronyms

## ER

**ER — Evidence Record**

The primary Protocol Object representing an Accountable Action or another accountability-relevant event.

In normative prose, **Evidence Record** SHOULD generally be written in full when clarity benefits from the expanded term.

---

## ERID

**ERID — Evidence Record Identifier**

A stable identifier for an Evidence Record.

The exact syntax and generation rules belong to TAIP.

An ERID is distinct from the cryptographic digest of the Evidence Record.

```text
Evidence Record Identifier
≠
Evidence Record Digest
```

---

## CE

**CE — Chain Entry**

A Chain Entry is an element of the TrustAgentAI append-only Hash Chain.

A Chain Entry cryptographically binds committed evidence to preceding Chain state according to applicable TAIP rules.

---

## CID

**CID — Chain Identifier**

A stable identifier for a TrustAgentAI Chain.

A Chain Identifier is distinct from:

- a Chain Entry identifier;
- a Chain Head;
- a Registry identifier;
- a storage location;
- a network location.

Because `CID` is also used by other technologies, the full term **Chain Identifier** SHOULD be used where ambiguity is possible.

---

## CH

**CH — Chain Head**

The cryptographic commitment representing a specified terminal state of a TrustAgentAI Chain.

A Chain Head may subsequently be:

- observed;
- witnessed;
- checkpointed;
- externally anchored;
- preserved.

---

## CR

**CR — Commitment Receipt**

A Commitment Receipt is a Protocol Object or cryptographically protected artifact providing evidence that a defined Commitment occurred.

Because `CR` is highly ambiguous outside the TrustAgentAI context, the full term SHOULD normally be preferred.

---

## WO

**WO — Witness Observation**

A Witness Observation is a signed Protocol Object representing a Witness's observation of defined protocol state.

The Observation Scope determines exactly what the Witness claims to have observed.

---

## OS

**OS — Observation Scope**

Observation Scope defines the exact scope of a Witness Observation.

Because `OS` commonly means **Operating System**, the expanded term SHOULD generally be preferred in prose.

---

## WQ

**WQ — Witness Quorum**

Witness Quorum defines the number and/or composition of eligible Witness Observations required by an applicable Trust Profile or verification policy.

Witness Quorum may include independence conditions and therefore MUST NOT be interpreted solely as an instance count.

---

## CP

**CP — Checkpoint**

A Checkpoint is a cryptographically protected commitment to defined historical protocol state.

Because `CP` is ambiguous in many technical domains, **Checkpoint** SHOULD generally be written in full.

---

## CA

**CA — Checkpoint Authority**

The logical protocol role authorized to create TrustAgentAI Checkpoints.

`CA` commonly means **Certificate Authority** in PKI terminology.

The full term **Checkpoint Authority** SHOULD therefore be preferred whenever ambiguity is possible.

---

## EA

**EA — External Anchor**

An External Anchor is a commitment to TrustAgentAI state placed into an independently controlled external system.

---

## AE

**AE — Anchor Evidence**

Anchor Evidence supports verification that a TrustAgentAI commitment was placed into an External Anchor.

Because `AE` is ambiguous, the expanded term SHOULD normally be preferred.

---

# Identity and Key Acronyms

## PID

**PID — Protocol Identity**

A stable TrustAgentAI identifier representing an Actor, Agent, Organization, service, or other protocol subject.

A PID is not equivalent to an operating-system Process Identifier.

The expanded term SHOULD be used where ambiguity is possible.

---

## KID

**KID — Key Identifier**

A stable reference identifying specific cryptographic key material or a defined key record.

A Key Identifier is distinct from the Protocol Identity associated with the key.

```text
Protocol Identity
≠
Key Identifier
```

---

## KT

**KT — Key Transparency**

Key Transparency preserves independently verifiable historical evidence concerning cryptographic key bindings and lifecycle state.

It supports evaluation of events including:

- registration;
- activation;
- rotation;
- suspension;
- reactivation;
- retirement;
- revocation;
- compromise;
- recovery.

---

## KTR

**KTR — Key Transparency Record**

A Protocol Object representing an accountability-relevant Key Transparency lifecycle event or identity-key relationship.

---

## HKS

**HKS — Historical Key State**

The state of a cryptographic key and its applicable authorization context at the historical boundary relevant to verification.

Historical Key State is distinct from Current Key State.

---

## CKS

**CKS — Current Key State**

The cryptographic key state known or applicable at the current evaluation time.

Current Key State MUST NOT silently replace Historical Key State when historical verification requires the latter.

---

## KP

**KP — Key Purpose**

The protocol purpose or purposes for which a cryptographic key is authorized.

Examples may include:

- Evidence Record signing;
- Witness Observation signing;
- Checkpoint signing;
- Migration Record signing.

The full term SHOULD normally be preferred in normative prose.

---

# Verification Acronyms

## VE

**VE — Verification Engine**

A Verification Engine evaluates TrustAgentAI evidence according to explicit protocol, cryptographic, policy, historical, and Trust Profile requirements.

---

## VC

**VC — Verification Context**

Verification Context is the complete context under which evidence is evaluated.

It may include:

- protocol version;
- schema version;
- Trust Profile;
- policy;
- verification time;
- resolver state;
- historical key state;
- algorithm policy.

Because `VC` commonly means **Verifiable Credential** in identity systems, **Verification Context** SHOULD normally be written in full.

---

## VR

**VR — Verification Report**

A durable representation of the result of evaluating TrustAgentAI evidence.

A Verification Report may identify:

- Verification Context;
- evidence evaluated;
- checks performed;
- failures;
- missing dependencies;
- warnings;
- achieved Trust Profile;
- overall outcome.

---

## VD

**VD — Verification Dependency**

Information or evidence required to interpret or verify another piece of protocol evidence.

Examples include:

- historical keys;
- Chain evidence;
- Witness Observations;
- Checkpoints;
- Trust Profiles;
- schemas;
- algorithm definitions.

---

## VDG

**VDG — Verification Dependency Graph**

The set of relationships between evidence and the dependencies required for its interpretation and verification.

Long-term Preservation SHOULD consider the Verification Dependency Graph rather than preserving only primary Evidence Records.

---

# Preservation Acronyms

## PS

**PS — Preservation Service**

The logical role responsible for durable retention of Accountability Evidence and required verification dependencies.

Because `PS` is highly ambiguous, the full term **Preservation Service** SHOULD generally be preferred.

---

## PE

**PE — Preservation Evidence**

Evidence supporting claims concerning:

- retention;
- integrity;
- archival state;
- storage policy;
- migration;
- custody;
- recovery;
- erasure.

---

## WORM

**WORM — Write Once, Read Many**

A storage property or mechanism intended to restrict modification or deletion of data after it is written.

WORM may support TrustAgentAI Preservation requirements.

WORM alone is not a complete accountability mechanism.

---

## LH

**LH — Legal Hold**

A preservation state or policy preventing normal disposal of evidence because the evidence may be required for:

- litigation;
- investigation;
- audit;
- regulatory review;
- another formal process.

TrustAgentAI does not define jurisdiction-specific Legal Hold law.

---

# Portable Evidence Acronyms

## DP

**DP — Dispute Pack**

A portable evidence package intended to support independent verification outside the originating production environment.

A Dispute Pack may contain:

```text
Manifest
Evidence Records
Commitment Evidence
Chain Evidence
Witness Observations
Checkpoints
Key Transparency Records
Preservation Evidence
Trust Profiles
Verification Dependencies
```

---

## DPM

**DPM — Dispute Pack Manifest**

The canonical Manifest describing the contents, integrity references, dependencies, omissions, and claims associated with a Dispute Pack.

---

## MR

**MR — Migration Record**

A protocol object or signed evidence describing an accountable transition between protocol-relevant states.

Examples include migration between:

- protocol versions;
- Chains;
- cryptographic algorithms;
- infrastructure operators;
- Preservation Services;
- Trust Profiles.

---

# Cryptographic Acronyms

## PKI

**PKI — Public Key Infrastructure**

A system for managing public-key identities, certificates, trust relationships, and cryptographic keys.

TrustAgentAI MAY integrate with existing PKI systems.

It does not require one universal PKI.

---

## HSM

**HSM — Hardware Security Module**

Hardware designed to protect cryptographic keys and perform cryptographic operations.

A Trust Profile MAY require HSM-backed or equivalent key custody.

---

## KMS

**KMS — Key Management System**

A system used to create, protect, rotate, control, and use cryptographic keys.

Use of a KMS does not automatically establish independent key custody.

---

## TEE

**TEE — Trusted Execution Environment**

A hardware- or platform-supported isolated execution environment intended to protect code or data from other components of a computing system.

TEE use is not mandatory for the TrustAgentAI base architecture.

---

## TSA

**TSA — Time-Stamping Authority**

A service providing cryptographically protected evidence relating data to a time value under a defined trust model.

A TSA timestamp MUST NOT automatically be interpreted as Event Time for the underlying business action.

---

## RNG

**RNG — Random Number Generator**

A mechanism producing values used by cryptographic or protocol operations.

---

## CSPRNG

**CSPRNG — Cryptographically Secure Pseudorandom Number Generator**

A pseudorandom number generator designed to provide properties suitable for cryptographic use.

---

## MAC

**MAC — Message Authentication Code**

A cryptographic mechanism providing message integrity and authenticity using shared secret material.

A MAC does not provide the same trust separation properties as an asymmetric digital signature.

---

## HMAC

**HMAC — Hash-based Message Authentication Code**

A standardized Message Authentication Code construction based on a cryptographic hash function.

---

## AEAD

**AEAD — Authenticated Encryption with Associated Data**

An encryption construction providing confidentiality and integrity for encrypted data together with authenticated associated metadata.

Encryption does not replace evidence signatures or historical commitments.

---

## SHA

**SHA — Secure Hash Algorithm**

A family of cryptographic hash functions.

Specific algorithms permitted by TAIP are defined by applicable algorithm registries and Trust Profiles.

---

## SHA-256

**SHA-256 — Secure Hash Algorithm 256-bit**

A cryptographic hash function in the SHA-2 family that produces a 256-bit digest.

Use of SHA-256 in examples MUST NOT be interpreted as permanently fixing TrustAgentAI to that algorithm.

TrustAgentAI requires algorithm agility.

---

# Serialization and Data Acronyms

## JSON

**JSON — JavaScript Object Notation**

A structured text data format that may be used by TrustAgentAI bindings.

Ordinary JSON serialization MUST NOT automatically be assumed to be canonical.

---

## JCS

**JCS — JSON Canonicalization Scheme**

A standardized mechanism for producing deterministic JSON representations.

Whether TAIP uses JCS or another canonicalization mechanism is determined by the applicable normative specification.

---

## CBOR

**CBOR — Concise Binary Object Representation**

A binary structured-data serialization format.

CBOR does not automatically provide canonical representation unless the applicable deterministic encoding rules are specified.

---

## YAML

**YAML — YAML Ain't Markup Language**

A human-readable structured-data format commonly used for configuration and documentation.

YAML SHOULD NOT be assumed to provide cryptographic canonicalization without explicit canonicalization rules.

---

## UTF-8

**UTF-8 — Unicode Transformation Format, 8-bit**

A Unicode character encoding commonly used for interoperable text representation.

TAIP MUST define any applicable encoding and normalization requirements where cryptographic representation depends on them.

---

## URI

**URI — Uniform Resource Identifier**

A standardized identifier syntax for resources.

---

## URL

**URL — Uniform Resource Locator**

A URI identifying a resource by location or access mechanism.

A mutable URL SHOULD NOT normally serve as the sole permanent cryptographic identity of Accountability Evidence.

---

## URN

**URN — Uniform Resource Name**

A URI intended primarily to identify a resource by name within a defined namespace.

---

## UUID

**UUID — Universally Unique Identifier**

A standardized identifier format commonly used to generate identifiers without centralized coordination.

Use of UUIDs for specific TAIP objects is determined by the relevant normative specification.

---

## MIME

**MIME — Multipurpose Internet Mail Extensions**

A system of media-type identifiers used to describe data formats.

---

# Infrastructure Acronyms

## API

**API — Application Programming Interface**

A software interface used by systems to interact programmatically.

An API binding is not itself TAIP.

---

## SDK

**SDK — Software Development Kit**

A developer-facing collection of libraries and tools supporting TrustAgentAI integration.

An SDK MUST preserve TAIP semantics and MUST NOT become a substitute normative specification.

---

## CLI

**CLI — Command-Line Interface**

A text-oriented interface that may be used for:

- verification;
- evidence inspection;
- Dispute Pack operations;
- Registry interaction;
- conformance testing.

---

## RPC

**RPC — Remote Procedure Call**

A communication model allowing software to invoke operations on another system.

RPC may be used by a TAIP binding but is not required by the architecture.

---

## REST

**REST — Representational State Transfer**

An architectural style commonly used for network APIs.

REST may be used by TrustAgentAI reference bindings.

REST is not part of the core protocol trust model.

---

## HTTP

**HTTP — Hypertext Transfer Protocol**

An application-layer protocol commonly used for APIs and resource retrieval.

HTTP may be used as a TAIP transport binding.

---

## HTTPS

**HTTPS — Hypertext Transfer Protocol Secure**

HTTP transported over TLS.

---

## TLS

**TLS — Transport Layer Security**

A protocol providing confidentiality and integrity for network communications.

TLS protects transport.

It does not replace durable object-level Accountability Evidence.

---

## mTLS

**mTLS — Mutual Transport Layer Security**

TLS in which both endpoints authenticate to one another.

mTLS strengthens transport-level identity but does not by itself establish historical protocol Authorization or Commitment.

---

## DNS

**DNS — Domain Name System**

The distributed naming system commonly used to resolve Internet domain names.

A DNS name is not automatically a durable Protocol Identity.

---

## RPC

**RPC — Remote Procedure Call**

A mechanism by which one software process requests execution of an operation by another.

---

## gRPC

**gRPC — Google Remote Procedure Call**

An RPC framework frequently using Protocol Buffers and HTTP/2 or related transports.

gRPC is a possible implementation binding and is not part of TAIP Core.

---

## SSE

**SSE — Server-Sent Events**

A mechanism for server-to-client event streaming over HTTP.

---

## WS

**WS — WebSocket**

A protocol supporting persistent bidirectional communication between network endpoints.

---

# Identity and Access Acronyms

## IAM

**IAM — Identity and Access Management**

Systems and processes used to manage identities, authentication, authorization, and access.

Historical IAM state MAY be relevant to TrustAgentAI verification.

---

## RBAC

**RBAC — Role-Based Access Control**

An authorization model in which permissions are assigned according to roles.

---

## ABAC

**ABAC — Attribute-Based Access Control**

An authorization model in which access decisions depend on attributes associated with subjects, resources, actions, and context.

---

## MFA

**MFA — Multi-Factor Authentication**

Authentication requiring multiple authentication factors.

MFA can strengthen administrative security but does not itself create TrustAgentAI Accountability Evidence.

---

## DID

**DID — Decentralized Identifier**

A class of identifiers defined by decentralized identity specifications.

TrustAgentAI MAY integrate with DIDs but does not require them as a universal identity mechanism.

---

## VC

**VC — Verifiable Credential**

A credential model commonly associated with digital identity systems.

Because `VC` may also be interpreted informally as Verification Context, TrustAgentAI documentation SHOULD use expanded terminology whenever ambiguity is possible.

---

# Financial and Enterprise Acronyms

## ERP

**ERP — Enterprise Resource Planning**

Enterprise systems managing business processes such as:

- accounting;
- procurement;
- inventory;
- finance;
- operations.

---

## AP

**AP — Accounts Payable**

Financial obligations owed by an Organization to suppliers or creditors.

---

## AR

**AR — Accounts Receivable**

Amounts owed to an Organization by customers or counterparties.

---

## PO

**PO — Purchase Order**

A commercial document authorizing or requesting the acquisition of goods or services.

---

## AML

**AML — Anti-Money Laundering**

Legal, regulatory, and operational controls intended to detect and prevent money laundering.

TrustAgentAI evidence may support AML processes.

TAIP conformance does not automatically establish AML compliance.

---

## KYC

**KYC — Know Your Customer**

Processes used to identify and evaluate customers under applicable policy or regulation.

TrustAgentAI does not itself define universal KYC requirements.

---

## KYB

**KYB — Know Your Business**

Processes used to identify and evaluate business entities.

---

## KYT

**KYT — Know Your Transaction**

Processes used to evaluate transaction behavior and associated risk.

---

## PCI DSS

**PCI DSS — Payment Card Industry Data Security Standard**

A security standard applicable to environments handling payment-card information.

TrustAgentAI conformance does not imply PCI DSS compliance.

---

## SOX

**SOX — Sarbanes-Oxley Act**

United States legislation relating to corporate governance and financial reporting controls.

TrustAgentAI evidence may support relevant control processes but does not itself establish SOX compliance.

---

# Governance and Standards Acronyms

## RFC

**RFC — Request for Comments**

Within TrustAgentAI governance, an RFC is a structured proposal for a significant architectural, protocol, registry, profile, or governance change.

A TrustAgentAI RFC MUST NOT be confused with an IETF RFC unless the external IETF document is explicitly identified.

---

## ADR

**ADR — Architecture Decision Record**

A durable record of an important architectural decision, including its context, alternatives, consequences, and rationale.

---

## IETF

**IETF — Internet Engineering Task Force**

An international standards community responsible for many Internet protocol specifications.

---

## ISO

**ISO — International Organization for Standardization**

An international standards organization publishing technical and organizational standards.

---

## IEC

**IEC — International Electrotechnical Commission**

An international standards organization focused on electrical, electronic, and related technologies.

---

## NIST

**NIST — National Institute of Standards and Technology**

A United States government organization publishing widely used cryptographic, cybersecurity, and technical standards and guidance.

---

## W3C

**W3C — World Wide Web Consortium**

An international standards organization responsible for Web technologies and related specifications.

---

## OWASP

**OWASP — Open Worldwide Application Security Project**

An open community producing security guidance, standards, and tools for software systems.

---

# Security Acronyms

## DoS

**DoS — Denial of Service**

An attack intended to make a system or protocol capability unavailable.

---

## DDoS

**DDoS — Distributed Denial of Service**

A Denial-of-Service attack originating from multiple distributed sources.

---

## MITM

**MITM — Man-in-the-Middle**

An attack in which an adversary intercepts or modifies communications between parties.

---

## TOCTOU

**TOCTOU — Time of Check to Time of Use**

A condition in which relevant state changes between validation and subsequent use.

This may affect:

- Authority;
- policy;
- key state;
- limits;
- approval conditions.

---

## PII

**PII — Personally Identifiable Information**

Information capable of identifying or being associated with an individual.

TrustAgentAI SHOULD minimize unnecessary PII in Accountability Evidence.

---

## DLP

**DLP — Data Loss Prevention**

Controls intended to detect or prevent unauthorized disclosure of sensitive information.

---

## SIEM

**SIEM — Security Information and Event Management**

Systems used to aggregate and analyze security logs and events.

SIEM records and TrustAgentAI Accountability Evidence serve different purposes.

---

## SOC

**SOC — Security Operations Center / System and Organization Controls**

`SOC` has multiple common meanings.

TrustAgentAI documents SHOULD expand this acronym on first use whenever ambiguity exists.

---

# Availability and Operations Acronyms

## SLA

**SLA — Service-Level Agreement**

A contractual or operational agreement describing expected service characteristics.

An SLA is not itself a TrustAgentAI assurance mechanism.

---

## SLO

**SLO — Service-Level Objective**

A target level of service performance or reliability.

---

## SLI

**SLI — Service-Level Indicator**

A measurement used to evaluate service performance relative to an SLO.

---

## RPO

**RPO — Recovery Point Objective**

The maximum acceptable amount of data loss measured in time following disruption.

---

## RTO

**RTO — Recovery Time Objective**

The targeted time within which a service should recover following disruption.

---

# Time Acronyms

## UTC

**UTC — Coordinated Universal Time**

The primary global time standard used for interoperable time representation.

A UTC timestamp inside a Protocol Object is not automatically Trusted Time.

---

# Requirement Prefixes

## REQ

**REQ — Requirement**

Prefix used for chapter-local normative requirements.

Examples:

```text
REQ-EXEC-001
REQ-PROB-001
REQ-DESIGN-001
REQ-SYS-001
REQ-TERM-001
REQ-STATUS-001
```

The middle component identifies the relevant architectural domain.

The numeric suffix identifies the requirement within that domain.

Published Requirement identifiers SHOULD remain stable.

---

## INV

**INV — Invariant**

Prefix used for chapter-local architectural invariants.

Examples:

```text
INV-EXEC-001
INV-PROB-001
INV-DESIGN-001
INV-SYS-001
INV-TERM-001
INV-STATUS-001
```

An Invariant describes a property intended to remain true across conforming protocol and implementation evolution.

---

## GREQ

**GREQ — Global Requirement**

Prefix used for requirements consolidated into the Global Invariant and Requirement Index.

Example:

```text
GREQ-037
```

Global Requirements provide canonical traceability across Project Bible chapters, TAIP, Trust Profiles, conformance tests, and verification evidence.

---

## GINV

**GINV — Global Invariant**

Prefix used for globally applicable invariants consolidated into the Global Invariant and Requirement Index.

Example:

```text
GINV-007
```

---

# Test Identifier Prefixes

TrustAgentAI conformance tests may use stable identifier families such as:

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
TST-VERSION-*
TST-GOV-*
```

These prefixes are identifiers rather than ordinary acronyms.

Their exact normative meaning belongs to the Conformance Test Suite specification.

---

# Acronym Usage Rules

## First Use

Where practical, an acronym SHOULD be expanded on first use in a standalone document.

For example:

```text
TrustAgentAI Interoperability Protocol (TAIP)
```

Subsequent references may use:

```text
TAIP
```

---

## Prefer Canonical Terms in Normative Prose

Canonical Protocol Object names SHOULD generally be written in full when used in normative requirements.

Prefer:

```text
The Witness Observation MUST...
```

over:

```text
The WO MUST...
```

unless the abbreviated form clearly improves readability without introducing ambiguity.

---

## Avoid Ambiguous Acronyms

Particular caution should be used with acronyms such as:

```text
AE
CA
CID
CP
OS
PID
PS
VC
```

because they have common meanings outside TrustAgentAI.

The expanded term SHOULD be used whenever multiple interpretations are plausible.

---

## Acronyms Do Not Define Technology Requirements

The presence of an acronym in this document does not imply mandatory use of the corresponding technology.

For example, inclusion of:

- HSM;
- TEE;
- CBOR;
- JCS;
- DID;
- gRPC;

does not make those technologies mandatory for TAIP.

Only applicable normative specifications can establish such requirements.

---

## Identifiers Are Not Automatically Acronyms

Values such as:

```text
TP3
GREQ-037
INV-SYS-004
TST-CHAIN-010
```

are identifiers.

Their meaning is established by the applicable specification or registry.

---

# Canonical Acronym Summary

| Acronym | Meaning |
|---|---|
| TAIP | TrustAgentAI Interoperability Protocol |
| TP | Trust Profile |
| ER | Evidence Record |
| ERID | Evidence Record Identifier |
| CE | Chain Entry |
| CID | Chain Identifier |
| CH | Chain Head |
| CR | Commitment Receipt |
| WO | Witness Observation |
| OS | Observation Scope |
| WQ | Witness Quorum |
| CP | Checkpoint |
| CA | Checkpoint Authority |
| EA | External Anchor |
| AE | Anchor Evidence |
| PID | Protocol Identity |
| KID | Key Identifier |
| KT | Key Transparency |
| KTR | Key Transparency Record |
| HKS | Historical Key State |
| CKS | Current Key State |
| KP | Key Purpose |
| VE | Verification Engine |
| VC | Verification Context / Verifiable Credential; expand when ambiguous |
| VR | Verification Report |
| VD | Verification Dependency |
| VDG | Verification Dependency Graph |
| PS | Preservation Service |
| PE | Preservation Evidence |
| WORM | Write Once, Read Many |
| LH | Legal Hold |
| DP | Dispute Pack |
| DPM | Dispute Pack Manifest |
| MR | Migration Record |
| PKI | Public Key Infrastructure |
| HSM | Hardware Security Module |
| KMS | Key Management System |
| TEE | Trusted Execution Environment |
| TSA | Time-Stamping Authority |
| RNG | Random Number Generator |
| CSPRNG | Cryptographically Secure Pseudorandom Number Generator |
| MAC | Message Authentication Code |
| HMAC | Hash-based Message Authentication Code |
| AEAD | Authenticated Encryption with Associated Data |
| SHA | Secure Hash Algorithm |
| SHA-256 | Secure Hash Algorithm 256-bit |
| JSON | JavaScript Object Notation |
| JCS | JSON Canonicalization Scheme |
| CBOR | Concise Binary Object Representation |
| YAML | YAML Ain't Markup Language |
| UTF-8 | Unicode Transformation Format, 8-bit |
| URI | Uniform Resource Identifier |
| URL | Uniform Resource Locator |
| URN | Uniform Resource Name |
| UUID | Universally Unique Identifier |
| MIME | Multipurpose Internet Mail Extensions |
| API | Application Programming Interface |
| SDK | Software Development Kit |
| CLI | Command-Line Interface |
| RPC | Remote Procedure Call |
| REST | Representational State Transfer |
| HTTP | Hypertext Transfer Protocol |
| HTTPS | Hypertext Transfer Protocol Secure |
| TLS | Transport Layer Security |
| mTLS | Mutual Transport Layer Security |
| DNS | Domain Name System |
| gRPC | Google Remote Procedure Call |
| SSE | Server-Sent Events |
| WS | WebSocket |
| IAM | Identity and Access Management |
| RBAC | Role-Based Access Control |
| ABAC | Attribute-Based Access Control |
| MFA | Multi-Factor Authentication |
| DID | Decentralized Identifier |
| ERP | Enterprise Resource Planning |
| AP | Accounts Payable |
| AR | Accounts Receivable |
| PO | Purchase Order |
| AML | Anti-Money Laundering |
| KYC | Know Your Customer |
| KYB | Know Your Business |
| KYT | Know Your Transaction |
| PCI DSS | Payment Card Industry Data Security Standard |
| SOX | Sarbanes-Oxley Act |
| RFC | Request for Comments |
| ADR | Architecture Decision Record |
| IETF | Internet Engineering Task Force |
| ISO | International Organization for Standardization |
| IEC | International Electrotechnical Commission |
| NIST | National Institute of Standards and Technology |
| W3C | World Wide Web Consortium |
| OWASP | Open Worldwide Application Security Project |
| DoS | Denial of Service |
| DDoS | Distributed Denial of Service |
| MITM | Man-in-the-Middle |
| TOCTOU | Time of Check to Time of Use |
| PII | Personally Identifiable Information |
| DLP | Data Loss Prevention |
| SIEM | Security Information and Event Management |
| SOC | Security Operations Center / System and Organization Controls |
| SLA | Service-Level Agreement |
| SLO | Service-Level Objective |
| SLI | Service-Level Indicator |
| RPO | Recovery Point Objective |
| RTO | Recovery Time Objective |
| UTC | Coordinated Universal Time |
| REQ | Requirement |
| INV | Invariant |
| GREQ | Global Requirement |
| GINV | Global Invariant |

---

# Acronym Invariants

### INV-ACR-001 — Stable Meaning

A TrustAgentAI-specific acronym used normatively MUST NOT be silently reassigned to an incompatible meaning.

---

### INV-ACR-002 — Ambiguity Avoidance

Specifications SHOULD prefer expanded terminology when an acronym could reasonably be interpreted as another common technical term.

---

### INV-ACR-003 — Protocol Meaning

An acronym MUST NOT change the semantic meaning of the canonical term it abbreviates.

---

### INV-ACR-004 — Historical Interpretability

Acronyms appearing in historical TrustAgentAI specifications MUST remain interpretable after later terminology evolves.

---

### INV-ACR-005 — Technology Neutrality

The inclusion of an external technology acronym in this document MUST NOT imply that the technology is mandatory for TrustAgentAI.

---

# Acronym Requirements

### REQ-ACR-001

Normative specifications SHOULD expand TrustAgentAI-specific acronyms on first use.

---

### REQ-ACR-002

Canonical Protocol Object names SHOULD be preferred over abbreviations where normative clarity is important.

---

### REQ-ACR-003

A newly introduced TrustAgentAI-specific acronym SHOULD be documented before becoming widely used across normative specifications.

---

### REQ-ACR-004

Acronyms that conflict with common external terminology SHOULD be avoided or explicitly disambiguated.

---

### REQ-ACR-005

Published acronym meanings SHOULD remain stable across compatible specification revisions.

---

### REQ-ACR-006

Historical specifications SHOULD remain available so that deprecated or superseded acronym meanings remain interpretable.

---

# Summary

Acronyms exist to make technical documentation more compact.

They must not make protocol meaning less precise.

TrustAgentAI therefore treats acronym usage as a documentation convenience rather than a replacement for canonical terminology.

Where an abbreviation introduces ambiguity, the full canonical term SHOULD be used.

The canonical architectural terminology remains defined by `Terminology.md`.