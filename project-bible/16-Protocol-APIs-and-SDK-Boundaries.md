# Chapter 16 — Protocol APIs and SDK Boundaries

> **APIs transport protocol operations and SDKs make them usable; neither may redefine the evidence, lifecycle, trust, or Verification semantics established by TAIP.**

## Purpose

This chapter defines the architectural boundaries for TrustAgentAI **protocol application programming interfaces (APIs)**, **transport bindings**, **software development kits (SDKs)**, resolver interfaces, export interfaces, and Verification interfaces.

TrustAgentAI must be implementable across languages, vendors, deployment models, and transports without allowing a convenient client method, HTTP status, queue acknowledgment, database write, or product-specific object model to become the protocol definition.

This chapter establishes:

- separation of protocol semantics from API, transport, storage, and presentation behavior;
- the responsibilities and limits of SDKs and reference implementations;
- normative-source precedence and implementation-neutral conformance;
- request, response, operation, status, and error semantics;
- creation, finalization, signing, Submission, Acceptance, Commitment, and receipt interfaces;
- retry, replay, idempotency, concurrency, batching, and partial-failure rules;
- authentication, Authorization, delegation, and Policy boundaries;
- version negotiation, capability discovery, extension, and compatibility behavior;
- representation, media-type, canonical-byte, identifier, locator, and resolver boundaries;
- pagination, streaming, subscriptions, callbacks, and webhook semantics;
- Witness, Checkpoint, Key Transparency, Preservation, Dispute Pack, and Verification APIs;
- offline operation, export, privacy, encryption, resource limits, observability, and caching;
- safe SDK defaults, multi-tenant isolation, conformance, and test vectors;
- API and SDK invariants and architectural requirements.

This chapter applies the common Protocol Object rules defined in [06-Protocol-Objects.md](06-Protocol-Objects.md).

It composes the Evidence Record lifecycle in [07-Evidence-Record-Specification.md](07-Evidence-Record-Specification.md), the Hash Chain Commitment model in [08-Hash-Chain-Specification.md](08-Hash-Chain-Specification.md), the Witness model in [09-Witness-Observation-Specification.md](09-Witness-Observation-Specification.md), the Checkpoint and External Anchor model in [10-Checkpoints-and-External-Anchors.md](10-Checkpoints-and-External-Anchors.md), the Historical Key State model in [11-Key-Transparency.md](11-Key-Transparency.md), the Preservation model in [12-Preservation.md](12-Preservation.md), the Dispute Pack model in [13-Dispute-Packs.md](13-Dispute-Packs.md), the Verification model in [14-Verification.md](14-Verification.md), and the Trust Profile model in [15-Trust-Profiles.md](15-Trust-Profiles.md).

It also applies the system model in [05-System-Overview.md](05-System-Overview.md), the principles in [04-Design-Principles.md](04-Design-Principles.md), and the canonical terms in [Terminology.md](Terminology.md) and [Acronyms.md](Acronyms.md).

This chapter defines architectural semantics.

It does not mandate HTTP, REST, gRPC, GraphQL, message queues, event streams, one media type, one authentication scheme, one programming language, one SDK architecture, one cloud provider, or one product interface. It does not define final endpoint paths, method names, field names, generated client layouts, or user-interface text.

Those concrete bindings belong to TAIP binding specifications, schemas, registries, API descriptions, SDK contracts, conformance suites, and implementation documentation. They must preserve the boundaries and invariants defined here.

---

# 16.1 Protocol API Definition

A **Protocol API** is a versioned interface through which a party requests, performs, observes, retrieves, or verifies a TAIP-defined operation.

A Protocol API may expose operations for:

- constructing or validating Protocol Objects;
- submitting finalized evidence;
- requesting historical Commitment;
- retrieving receipts and lifecycle state;
- resolving references and dependencies;
- obtaining Witness, Checkpoint, Anchor, or Key Transparency evidence;
- preserving or exporting evidence;
- assembling Dispute Packs;
- requesting Verification and retrieving Verification Reports;
- discovering supported versions, profiles, algorithms, and capabilities.

The API is a binding to protocol semantics. It does not create new semantics merely through endpoint naming or transport behavior.

---

# 16.2 Architectural Objectives

Protocol APIs and SDKs should make correct behavior easier while preserving:

- deterministic object meaning;
- explicit lifecycle states;
- stable identity and integrity;
- bounded trust assumptions;
- failure and uncertainty visibility;
- historical interpretation;
- portable evidence;
- independent Verification;
- implementation and vendor neutrality;
- privacy proportionality.

```text
Convenient Integration
    +
Stable Protocol Semantics
    +
Explicit Trust Boundaries
    =
Interoperable Accountability
```

Ease of use must not come from hiding protocol-significant distinctions.

---

# 16.3 Scope

This chapter applies to:

- public and private service APIs implementing TAIP operations;
- local in-process libraries;
- generated clients and servers;
- command-line and batch interfaces;
- event-driven and message-oriented bindings;
- resolver, storage, and export adapters;
- Agent tool interfaces;
- Verification Engine interfaces;
- administrative interfaces when they affect protocol state;
- reference implementations and sample SDKs.

It does not make every internal method a Protocol API.

An internal interface becomes protocol-relevant when its behavior affects canonical content, object identity, cryptographic input, lifecycle state, evidence Completeness, Trust Profile achievement, interoperability, or an externally relied-upon claim.

---

# 16.4 Layered Interface Model

TrustAgentAI separates four layers:

```text
Application and Product Experience
              │
SDK and Convenience Abstractions
              │
API and Transport Binding
              │
TAIP Protocol Semantics
```

The lower layer constrains the meaning exposed by the layer above it.

Products may choose different workflows and presentations. SDKs may provide typed builders, retries, caching, and ergonomic error types. Bindings may use HTTP, RPC, queues, or local calls. None may contradict TAIP semantics.

Conformance is evaluated against applicable TAIP and binding requirements, not against accidental behavior of one SDK or product.

---

# 16.5 Protocol Semantics and API Semantics

**Protocol semantics** define what an object, operation, state, receipt, or outcome means.

**API semantics** define how that meaning is requested or returned through a particular interface.

```text
HTTP 202 Accepted
≠
TAIP Acceptance
≠
TAIP Commitment
```

An API may return transport success while the protocol operation is pending, rejected, incomplete, unsupported, or indeterminate.

Binding specifications must map transport results to protocol results explicitly. Implementations must not infer protocol success from a generic transport status.

---

# 16.6 Transport Binding and Core Meaning

A **transport binding** defines how protocol messages and operations are carried through a communication mechanism.

A binding may define:

- endpoint or method mapping;
- request and response serialization;
- transport authentication;
- correlation and tracing metadata;
- synchronous and asynchronous patterns;
- transport error mapping;
- retry and timeout guidance;
- streaming or subscription behavior.

Transport metadata is not automatically part of canonical Protocol Object content or cryptographic coverage.

Request paths, headers, connection identities, queue offsets, and message-broker acknowledgments may affect operation handling, but they supply protocol meaning only when TAIP explicitly binds them.

---

# 16.7 SDK Definition and Responsibility

An **SDK** is an implementation aid that exposes protocol and binding operations through language- or platform-specific abstractions.

An SDK may provide:

- object builders and validators;
- canonicalization and digest functions;
- signing and Verification adapters;
- typed API clients;
- resolver and cache interfaces;
- retry, pagination, and streaming helpers;
- local key or HSM integration;
- Dispute Pack import and export;
- conformance fixtures and diagnostics.

An SDK must not become the sole source of protocol meaning.

Its undocumented defaults, serialization behavior, exception types, retry policy, or internal object model cannot redefine TAIP. A conforming independent implementation must be possible without using the SDK.

---

# 16.8 Normative Sources and Precedence

When sources differ, the applicable governed specification determines meaning.

A typical precedence model is:

```text
TAIP Core Semantics
      ▼
Governed Profile, Registry, and Schema Semantics
      ▼
Transport Binding Specification
      ▼
SDK Contract and API Description
      ▼
Examples, Tutorials, and Product Documentation
```

The exact precedence must be governed and versioned.

Examples and generated documentation are informative unless explicitly incorporated into a normative specification. A test suite may demonstrate conformance but must not silently invent requirements absent from its governed source.

Conflicts among normative sources must produce an explicit compatibility or specification defect rather than an implementation-specific guess.

---

# 16.9 Conceptual Roles

API and SDK interactions may involve:

- an **API Client** requesting an operation;
- an **API Service** implementing a binding;
- an **Evidence Producer** constructing Protocol Objects;
- a **Commitment Service** or Chain operator;
- a **Witness Service**;
- a **Checkpoint Authority** or Anchor adapter;
- a **Key Transparency Service**;
- a **Preservation Service**;
- a **Resolver**;
- a **Verification Engine**;
- a **Profile, Registry, or Policy Authority**;
- an **SDK Provider**;
- an **Operator** administering an implementation.

One component may perform several roles. Role combination must not obscure Authority, Control Domain, conflict of interest, or independence requirements.

---

# 16.10 API Trust Boundary

Every API crosses one or more trust boundaries.

Relevant boundaries may include:

- caller to service;
- application to SDK;
- SDK to key custodian;
- service to Registry or resolver;
- producer domain to independent Witness;
- online service to archival or offline environment;
- tenant to shared infrastructure;
- verifier to untrusted evidence source.

The interface contract should identify which inputs are trusted, authenticated, authorized, integrity-protected, validated, or treated as untrusted.

Network encryption protects a transport session. It does not replace object-level integrity, Historical Key State, Authority evidence, historical Commitment, or independent Verification.

---

# 16.11 Operation Model

A protocol operation should have a stable operation type and explicit lifecycle.

An operation may be:

- immediately completed;
- accepted for asynchronous processing;
- conditionally accepted pending dependencies;
- partially completed;
- rejected;
- conflicted;
- cancelled before a protected transition;
- indeterminate after a timeout or connection loss.

The operation identifier is distinct from the target Protocol Object identifier, idempotency key, request correlation ID, Chain Entry identifier, and receipt identifier.

```text
Operation ID
≠
Object ID
≠
Idempotency Key
≠
Receipt ID
```

---

# 16.12 Request Context

A request may need to identify:

- operation type and binding version;
- target object or canonical payload;
- caller identity and authenticated session;
- claimed Authority or delegation context;
- tenant, Organization, or namespace;
- Intended Trust Profile;
- expected predecessor or state precondition;
- idempotency and correlation identifiers;
- deadline, cancellation, and response preferences;
- permitted disclosure or resolver behavior;
- requested output representation.

Protocol-significant request values must be inside a governed protected boundary or explicitly bound into the resulting evidence.

Transport-only metadata must not silently change the meaning of finalized evidence.

---

# 16.13 Response Envelope

A response should distinguish:

- transport handling;
- operation identity;
- protocol operation state;
- object- or member-level results;
- receipts or proof artifacts;
- errors, warnings, and limitations;
- retry guidance;
- dependency and asynchronous status references;
- binding and representation versions.

A successful response envelope does not make every member successful.

A response should provide enough stable information to correlate later status, receipt, export, or Verification results without requiring undocumented server state.

Human-readable messages may aid diagnosis but must not replace stable machine-readable outcome codes.

---

# 16.14 Protocol Object Creation Interfaces

Object-creation APIs may support:

- draft construction;
- schema and semantic validation;
- identifier assignment;
- canonicalization;
- digest calculation;
- Signature preparation;
- Signature attachment;
- finalization.

The interface must distinguish client-authored properties from service-derived properties.

A server that adds or changes protected content is a producer or co-producer for that transformation and should leave attributable evidence. It must not claim that the caller signed content the caller never saw or approved.

Server timestamps, generated identifiers, default values, and normalized fields must have explicit protected-boundary semantics.

---

# 16.15 Validation, Finalization, and Canonicalization

Validation may occur at several stages:

- draft validation;
- finalization validation;
- admission validation;
- Commitment validation;
- Verification.

These checks may have different scope and must remain distinguishable.

Finalization establishes the content intended for identity, digest, Signature, Submission, or Commitment. After finalization, protected content must not change in place.

An SDK may expose canonical bytes or a signing input, but it must identify the exact object type, version, canonicalization method, included properties, and domain separation.

Pretty-printed, compressed, encrypted, multipart, or transport bytes are not canonical cryptographic input unless the applicable specification says so.

---

# 16.16 Signing and Key Interfaces

Signing interfaces may integrate with local keys, HSMs, KMSs, remote signers, or threshold systems.

They must bind:

- exact canonical signing input;
- algorithm and parameters;
- KID and exact key material or resolvable key reference;
- Key Purpose;
- protected object type and version;
- signer role;
- multi-Signature or threshold context;
- requested and produced Signature encoding.

A remote signing API must not treat possession of an API credential as unlimited signing Authority.

SDKs should prevent accidental signing of mutable drafts, ambiguous encodings, unknown critical semantics, or inputs outside the caller's intended scope.

---

# 16.17 Submission Interfaces

**Submission** means that material was sent to a defined receiver for processing.

A Submission interface should identify:

- submitted object, digest, or batch;
- receiver and target namespace;
- requested operation;
- Submission Time semantics;
- idempotency and correlation context;
- applicable admission rules;
- initial protocol state.

A transport acknowledgment proves only the bounded handling it declares.

```text
Request Delivered
≠
Submission Recorded
≠
Accepted
≠
Committed
```

Where interoperable proof of Submission is required, TAIP should define a receipt or evidence object rather than relying solely on client logs.

---

# 16.18 Acceptance and Admission Interfaces

**Acceptance** means that a receiver accepted material for defined processing under stated rules.

Acceptance may follow:

- structural validation;
- type and version support checks;
- identifier and digest validation;
- Signature validation;
- Policy or Authority checks;
- duplicate and conflict detection;
- size and resource checks;
- expected-state or predecessor checks.

The applied admission scope and deferred checks should be identifiable.

Acceptance does not automatically mean Commitment, Witnessing, Checkpointing, Preservation, Completeness, or business approval.

Rejected material must identify stable reason categories without falsely claiming checks that were never performed.

---

# 16.19 Commitment Interfaces

A Commitment interface requests that defined material enter protected historical state.

It should identify:

- committed object or typed commitment;
- target Chain or commitment namespace;
- expected predecessor or Chain Head where applicable;
- atomicity and ordering semantics;
- applicable Chain, Policy, and Trust Profile versions;
- requested proof or receipt form;
- synchronous or asynchronous completion behavior.

A database write, queue acknowledgment, locally assigned sequence, or generic `success` result is not automatically Commitment.

An interoperable Commitment claim requires a valid Chain Entry, Commitment Receipt, or other TAIP-defined proof sufficient for the claim.

---

# 16.20 Receipts and Asynchronous Completion

Long-running operations may return before protocol completion.

An asynchronous response should identify:

- operation ID;
- accepted request identity and integrity;
- current protocol state;
- status or result retrieval method;
- expiry and retention of operation state;
- callback or subscription options;
- cancellation limits;
- expected terminal outcomes.

A later receipt should bind to the original request and exact completed state.

Polling status is not a substitute for a durable Commitment Receipt when the claim requires portable evidence. A mutable dashboard may report operational progress but cannot silently become historical proof.

---

# 16.21 Idempotency Keys

An **idempotency key** helps a service recognize retransmission of the same intended operation.

Its scope must define:

- caller or tenant namespace;
- operation type;
- target service;
- protected request identity or digest;
- validity interval;
- comparison and collision behavior;
- retained result semantics.

An idempotency key is not a Protocol Object identifier, ERID, causal identifier, or proof of execution.

Reuse of one key with different protected content is a conflict, not an ordinary duplicate. A service must not return the first successful result for a semantically different request merely because the same key was reused.

Idempotency state should preserve enough integrity to prevent result substitution across callers or operations.

---

# 16.22 Retry, Duplicate, and Replay

Distributed clients retry after timeouts, connection loss, and ambiguous responses.

Interfaces must distinguish:

- retransmission of the same finalized request;
- duplicate delivery after successful processing;
- a new attempt of the same business action;
- intentional recommitment to another Chain;
- a conflicting object using the same identifier;
- malicious replay outside the allowed context;
- recovery after an indeterminate result.

SDK retry policy must consider whether the operation is safely idempotent and whether the first attempt may have crossed an irreversible protocol boundary.

A retry must not create duplicate exclusive Commitment. Deduplication must not erase a distinct accountability-relevant attempt.

---

# 16.23 Concurrency and State Preconditions

Concurrent operations may target the same Chain, object, Policy, key, or lifecycle state.

APIs should support explicit preconditions such as:

- expected object version;
- expected predecessor;
- expected Chain Head;
- expected lifecycle state;
- expected Registry snapshot;
- compare-and-set token;
- governed conflict identifier.

An opaque storage entity tag may be used as a binding mechanism only when its relationship to protocol state is defined.

Last-write-wins behavior must not silently resolve protocol conflicts or rewrite protected history. A failed precondition should produce an explicit conflict result that allows the caller to resolve, reconstruct, or retry under current state.

---

# 16.24 Batching and Partial Failure

Batch APIs must define whether processing is:

- atomic for the whole batch;
- independent per member;
- atomic for identified subgroups;
- accepted as a manifest with later member outcomes.

The response must preserve:

- batch identity and integrity;
- every member's object identity;
- member ordering where meaningful;
- duplicate, rejected, conflicted, pending, and committed states;
- aggregate and inclusion proof semantics;
- rollback or retry behavior.

```text
Batch Transport Success
≠
Every Member Accepted
≠
Every Member Committed
```

SDK helpers must not flatten member-level results into one Boolean.

---

# 16.25 Lifecycle and Status APIs

Status APIs may expose local and protocol lifecycle states.

Local states may include:

- draft;
- validated;
- finalized;
- signed;
- queued;
- retrying.

Protocol states may include:

- submitted;
- accepted;
- committed;
- witnessed;
- checkpointed;
- anchored;
- preserved;
- verified.

The state model must identify authoritative evidence for each state. Operational status must remain distinguishable from protocol evidence. A status API must not infer `committed` from a queue state or `preserved` from backup success.

---

# 16.26 Error Model

Bindings should use stable machine-readable error and outcome categories.

Relevant categories may include:

- malformed request;
- unsupported type, version, algorithm, or extension;
- authentication failure;
- insufficient Authority or Policy denial;
- invalid object, identifier, digest, or Signature;
- duplicate or idempotent replay;
- integrity conflict;
- state or predecessor conflict;
- missing or unavailable dependency;
- incomplete evidence;
- resource limit or rate limit;
- transient service unavailability;
- operation pending or indeterminate;
- internal processing failure.

Error payloads should identify which layer failed and which checks were completed. A convenient exception hierarchy cannot replace canonical protocol outcome codes.

---

# 16.27 Transport Status and Protocol Outcome

HTTP, RPC, messaging, and local-call statuses serve transport needs.

They must map to protocol outcomes without conflation.

Examples:

- an HTTP `200` may carry a Verification Report containing `Incomplete`;
- an HTTP `202` may mean the operation is queued, not TAIP Accepted;
- an HTTP `409` may represent state conflict, duplicate conflict, or another defined condition;
- a queue acknowledgment may mean only durable delivery;
- an RPC timeout may leave Commitment indeterminate.

The binding specification should define the mapping and require the structured protocol result to remain authoritative.

Clients must not treat every `2xx` response as protocol success or every `5xx` response as proof that no state transition occurred.

---

# 16.28 Authentication Boundary

Authentication establishes a bounded claim about the caller, channel, credential, or session.

API authentication may use:

- mutual TLS;
- signed requests;
- OAuth or access tokens;
- workload identities;
- delegated credentials;
- local process identity;
- another governed mechanism.

Authentication does not automatically establish business Authority, Key Purpose, Policy compliance, object validity, or evidence truth.

```text
Authenticated Caller
≠
Authorized Accountable Action
```

Bindings must identify which identity the service authenticated and how that identity relates to Protocol Identity, producer, signer, operator, or delegated actor.

---

# 16.29 Authorization and Policy Boundary

An API service must separately evaluate whether the authenticated caller may request the operation.

Authorization may depend upon:

- protocol role;
- tenant and resource scope;
- delegated Authority;
- Policy and Trust Profile;
- action type and value limits;
- lifecycle state;
- separation of duties;
- historical effective interval;
- required approvals.

Transport access does not grant authority to sign, commit, witness, checkpoint, export, decrypt, preserve, or verify every object.

Where Authorization affects an Accountability Claim, the decision and applicable evidence should be representable beyond an ephemeral API gateway log.

---

# 16.30 Sessions, Delegation, and Agent Context

Long-lived sessions and Agent workflows may act through delegated Authority.

Interfaces should preserve:

- delegator and delegate identity;
- delegation chain;
- granted operations and resources;
- amount, purpose, time, and Trust Profile limits;
- approval or Human intervention requirements;
- revocation and suspension state;
- session and request binding;
- onward-delegation rules.

A bearer token, session cookie, API key, or tool-call credential must not be represented as the complete Authority model.

SDKs should expose delegation and purpose explicitly rather than hiding them in a generic client configuration. Confused-deputy risks increase when one client instance silently reuses credentials across tenants, Agents, or purposes.

---

# 16.31 Version Dimensions

An API interaction may depend upon distinct versions for:

- TAIP core;
- transport binding;
- operation schema;
- Protocol Object type;
- object schema;
- canonicalization;
- algorithm suite;
- Trust Profile;
- Registry snapshot;
- extension;
- package format;
- SDK implementation.

```text
API Version
≠
Protocol Version
≠
Object Version
≠
Trust Profile Version
```

One `version` field must not silently collapse dimensions that affect interpretation. SDK version numbers are implementation metadata and do not determine protocol semantics by themselves.

---

# 16.32 Version Negotiation

Version negotiation should produce one explicit, deterministic effective context.

Negotiation may consider:

- client-supported versions;
- server-supported versions;
- profile requirements;
- object and dependency versions;
- security deprecation policy;
- extension support;
- representation and compression;
- historical compatibility.

The selected versions should be returned or bound into the operation evidence where they affect interpretation.

Silent fallback to an older protocol, weaker algorithm, incomplete profile, or lossy representation is prohibited.

If no mutually supported safe version exists, the result is Unsupported rather than best-effort reinterpretation.

---

# 16.33 Capability Discovery

A service may publish a versioned capability document describing:

- supported operations;
- object types and versions;
- encodings and media types;
- algorithms and proof systems;
- Trust Profiles;
- extensions;
- batch, streaming, and asynchronous features;
- resolver and export behavior;
- limits and availability targets;
- conformance-suite versions.

Capability discovery is an assertion about service support. It does not prove that a particular request was processed correctly or that evidence achieved a profile.

Capability documents should be authenticated, integrity-protected, cacheable under explicit freshness rules, and historically preservable where negotiation depends upon them.

SDKs must not assume capabilities solely from product name or endpoint shape.

---

# 16.34 Media Types and Representations

A binding may support JSON, CBOR, binary proofs, multipart packages, archives, streams, or other registered representations.

Each representation must define:

- media type and version parameters;
- mapping to logical Protocol Object semantics;
- character encoding and normalization;
- duplicate and unknown member behavior;
- canonicalization relationship;
- size and nesting limits;
- extension handling;
- encryption and compression boundaries.

Content negotiation must not change protected meaning silently.

Two representations may be semantically equivalent only when their mapping is deterministic and preserves all mandatory semantics. A lossy representation must have a distinct identity or explicit derived-representation relationship.

---

# 16.35 Wire Bytes and Canonical Bytes

**Wire bytes** are bytes transmitted through a binding.

**Canonical bytes** are the deterministic cryptographic input defined for an object, Signature, identifier, or commitment.

```text
Wire Bytes
≠
Canonical Bytes
≠
Stored Bytes
≠
Rendered Content
```

Compression, chunking, framing, whitespace, field order, encryption, and multipart boundaries may change wire bytes without changing canonical meaning.

An API or SDK must state which bytes are being returned or signed. Hashing an arbitrary transport serialization as if it were canonical can cause cross-implementation failures or Signature substitution.

---

# 16.36 Identity, URLs, and Locators

Protocol identity must remain distinct from retrieval location.

```text
Object ID
≠
Object Digest
≠
API URL
≠
Storage Locator
≠
Operation ID
```

URLs and locators may change because of migration, replication, tenancy, access routing, or Preservation.

An API should return stable protocol identifiers separately from convenience links. A retrieved object must be validated for expected identity, type, version, integrity, and applicability rather than trusted because it came from a familiar URL.

Opaque vendor resource names must not become mandatory global protocol identifiers unless registered with defined semantics.

---

# 16.37 Resolver Interfaces

A **Resolver API** retrieves Protocol Objects, keys, schemas, profiles, registries, Policies, proofs, or other Verification Dependencies.

A resolver response should identify:

- requested identifier and namespace;
- returned object identity, type, version, and integrity;
- authoritative or mirror role;
- retrieval time and freshness;
- cache and source information where material;
- redacted, unavailable, unsupported, or conflicting state;
- applicable access and disclosure constraints.

Resolution success is not Verification success.

Resolvers must not substitute current defaults for exact historical dependencies. Multiple conflicting results must remain visible or be resolved under an explicit governed rule.

---

# 16.38 Pagination and Collection Boundaries

Paginated APIs must define the collection and consistency boundary being traversed.

Pagination should identify:

- collection identity and query semantics;
- ordering relation;
- snapshot, cursor, or live-view behavior;
- cursor integrity and expiry;
- insertions, deletions, and concurrent updates;
- omitted or unauthorized members;
- completion indication;
- maximum page size.

Page order is not automatically Chain order, causal order, or Commitment order.

A client must not infer Completeness merely because it reached an empty page. If the collection changed during traversal, the API should provide a stable snapshot, consistency proof, or explicit limitation.

---

# 16.39 Streaming and Subscriptions

Streaming interfaces may deliver new objects, Chain Entries, Witness Observations, Checkpoints, Registry changes, or Verification results.

The stream contract should define:

- stream and partition identity;
- ordering and delivery guarantees;
- replay and resume semantics;
- cursor or offset meaning;
- duplicate handling;
- gap and truncation detection;
- backpressure;
- authentication and Authorization changes;
- terminal and reconnect behavior.

Transport stream order is not automatically authoritative protocol order.

At-most-once delivery can omit evidence. At-least-once delivery requires idempotent processing. Exactly-once marketing claims must state the actual boundary and proof.

---

# 16.40 Callbacks and Webhooks

Callbacks and webhooks may notify a client that an asynchronous state changed.

They should provide:

- notification identity;
- event type and version;
- operation and target references;
- resulting protocol state;
- event time and delivery time semantics;
- authenticity and replay protection;
- retry and ordering rules;
- a method to retrieve authoritative evidence.

A webhook is a notification, not necessarily the authoritative object or receipt.

Clients should verify notification authenticity and then validate referenced protocol evidence. Duplicate, delayed, reordered, or missing callbacks must not corrupt canonical state.

---

# 16.41 Witness APIs

Witness APIs may support observation requests, eligibility discovery, Observation retrieval, conflict reporting, and gossip.

An observation request should identify:

- exact target and integrity;
- Observation Scope;
- applicable Witness role and Trust Profile;
- requested timing or deadline;
- privacy and disclosure constraints;
- required response or proof type.

A successful request does not prove that observation occurred.

A valid Witness Observation must remain a separate verifiable Protocol Object or governed artifact. API authentication does not replace Witness Signature, Historical Key State, eligibility, independence, or quorum evaluation.

Witness services must not sign statements outside the scope they actually observed.

---

# 16.42 Checkpoint and Anchor APIs

Checkpoint APIs may submit candidate commitments, retrieve issued Checkpoints, resolve consistency evidence, and report conflicts.

Anchor APIs may submit commitments to external systems and retrieve publication, inclusion, finality, or confirmation evidence.

Interfaces must distinguish:

- request accepted;
- candidate validated;
- Checkpoint issued;
- anchor transaction submitted;
- externally published;
- required finality reached;
- proof retrieved and verified.

One transport or vendor status must not collapse these states.

Returned artifacts must bind the exact commitment, authority, namespace, time semantics, and applicable external trust assumptions.

---

# 16.43 Key Transparency APIs

Key Transparency APIs may expose:

- identity and key-state lookup;
- inclusion and consistency proofs;
- Registry or log checkpoints;
- key lifecycle events;
- gossip and conflict reports;
- historical snapshots.

Requests must identify the relevant Protocol Identity, KID, Key Purpose, and historical boundary.

A current-key lookup is insufficient for historical Signature Verification. The API must not silently return a replacement key when exact historical material is required.

Privacy-sensitive identity queries should support minimized, authenticated, cached, or offline resolution where possible. Resolver observations materially affecting Verification should be retainable.

---

# 16.44 Preservation APIs

Preservation APIs may accept evidence, produce custody or retention evidence, test recoverability, retrieve preserved dependencies, and perform migration.

They should distinguish:

- upload or transfer success;
- custody acceptance;
- integrity validation;
- retention commitment;
- immutability state;
- replication;
- recoverability test success;
- migration or renewal completion;
- deletion or legal-hold state.

```text
Upload Complete
≠
Preservation Accepted
≠
Preserved for Required Evidence Lifetime
```

Preservation evidence must remain portable and verifiable beyond a mutable provider dashboard where the Trust Profile requires it.

---

# 16.45 Dispute Pack and Export APIs

Export APIs should produce complete, bounded, and independently interpretable evidence packages.

An export request should define:

- focal claims, objects, Chain ranges, or historical boundary;
- intended verifier or disclosure context;
- required dependencies and Trust Profile;
- redaction and encryption policy;
- package format and version;
- asynchronous generation and expiry;
- Completeness expectations.

The result should include a Dispute Pack Manifest identifying included, external, redacted, omitted, unavailable, and unsupported material.

Export layout and file order are not canonical protocol order. Export success does not make included evidence valid or complete.

An implementation must not require proprietary production access for core Verification when the applicable profile requires portability.

---

# 16.46 Verification APIs

A Verification API submits evidence and an explicit Verification Context to a Verification Engine.

It should support:

- focal Accountability Claims;
- evidence or Dispute Pack inputs;
- exact profiles, schemas, registries, and algorithms;
- historical boundary and Verification Time;
- resolver and trust-root policy;
- privacy and disclosure constraints;
- resource limits;
- requested report form.

The response must preserve per-check, dependency, object, claim, Completeness, control, and aggregate outcomes where applicable.

Transport success is compatible with a protocol result of Invalid, Incomplete, Indeterminate, Unsupported, Unavailable, or Conflicting.

An SDK must not reduce a Verification Report to one `isValid` Boolean when material distinctions exist.

---

# 16.47 Trust Profile Interfaces

Profile APIs may discover definitions, select Intended Trust Profiles, evaluate controls, retrieve registries, and report Achieved Trust Profiles.

They must bind exact:

- namespace, profile ID, and version;
- definition integrity and Authority;
- action or claim scope;
- selection Policy;
- mandatory controls and dependencies;
- downgrade and exception rules;
- control-level results.

A capability endpoint stating `supports TP3` does not prove that evidence achieved TP3.

SDK profile enums are convenience aliases. They must not erase namespace, version, custom-profile semantics, or unsupported mandatory extensions.

---

# 16.48 Offline and Air-Gapped Operation

TAIP operations may need to run without live network access.

Offline interfaces may support:

- local object construction and signing;
- queued Submission;
- package-based dependency import;
- local Registry and profile snapshots;
- offline Verification;
- removable-media export;
- later synchronization.

The offline boundary must identify unavailable current-state checks, freshness limits, trusted snapshots, clock assumptions, and synchronization behavior.

Queued local state is not Submitted or Committed until the defined receiver processes it.

Synchronization must preserve idempotency, conflicts, actual event times, and later protocol transition times without backdating.

---

# 16.49 Confidentiality, Encryption, and Redaction

APIs may protect evidence through channel encryption, object encryption, field protection, package encryption, or access control.

These layers have different boundaries.

```text
TLS Protection
≠
Object Encryption
≠
Signed Canonical Content
≠
Authorization to Disclose
```

Bindings must define whether encryption occurs before or after canonicalization, what metadata remains visible, how keys are referenced, and how integrity is verified.

A redacted or derived representation must have an explicit relationship to canonical evidence. SDK serialization must not silently remove protected fields while retaining the same identity or Signature claim.

---

# 16.50 Privacy and Minimum Disclosure

API design should minimize collection, transmission, logging, caching, and disclosure to what the operation and Trust Profile require.

Relevant controls include:

- claim-bounded queries;
- field selection with explicit derived-representation semantics;
- selective disclosure;
- protected resolver requests;
- short-lived operation metadata;
- minimized error detail;
- compartmentalized Dispute Packs;
- tenant-scoped identifiers;
- access and disclosure evidence;
- retention and deletion rules.

Convenient debugging must not become indefinite capture of raw prompts, secrets, credentials, personal data, or complete evidence graphs.

Privacy filtering must not cause a mandatory omission to appear as successful full Verification.

---

# 16.51 Rate Limits and Resource Bounds

Services must protect themselves from unbounded input, dependency traversal, proof verification, export generation, and streaming load.

Limits may apply to:

- request and object size;
- batch cardinality;
- graph depth and breadth;
- cryptographic operations;
- concurrent operations;
- resolver requests;
- stream backlog;
- report or export size;
- CPU, memory, disk, and wall time.

Limits should be discoverable where interoperability depends upon them.

Reaching a limit must produce an explicit rate-limited, unavailable, incomplete, unsupported, or indeterminate state as defined by the binding. It must not produce partial success disguised as full Commitment or Verification.

---

# 16.52 Timeouts and Cancellation

A client timeout means the client stopped waiting. It does not prove that the server did not accept or complete the operation.

```text
Client Timeout
≠
Operation Failure
≠
No State Change
```

Cancellation semantics must identify the last reversible boundary.

An operation may be cancellable while queued but not after atomic Commitment. Cancellation of a client request must not delete already committed evidence.

After an ambiguous timeout, clients should query status or receipt state using the operation ID and idempotency context before retrying a non-repeatable operation.

SDKs must not promise cancellation guarantees stronger than the binding provides.

---

# 16.53 Observability and Accountability Evidence

Metrics, traces, logs, and health endpoints support API operation.

They may record:

- request latency and status;
- dependency failures;
- retry and queue behavior;
- resource use;
- service health;
- diagnostic correlation.

Operational observability is not automatically Accountability Evidence.

Where an operational event affects a claim—such as Authorization, admission, Commitment, override, key use, or deletion—the protocol should produce typed evidence with defined integrity and lifecycle semantics.

Tracing IDs may correlate events but do not establish causality, ordering, identity, or historical Commitment by themselves.

---

# 16.54 Caching and Consistency

SDKs and services may cache objects, keys, profiles, Registries, capabilities, resolver results, and Verification outputs.

Cache validity should consider:

- stable identity and integrity;
- exact version;
- tenant and Authorization scope;
- historical boundary;
- freshness and expiry;
- revocation, correction, or conflict;
- negative-result caching;
- Verification Context;
- dependency state.

Stale current-state data must not replace required historical evidence. A cache hit does not waive integrity or applicability validation.

Consistency models—strong, eventual, snapshot, session, or best effort—must be stated where they affect protocol outcomes.

---

# 16.55 Multi-Tenant Isolation

Shared services must isolate tenant identities, objects, keys, idempotency state, operation results, caches, streams, exports, and resolver permissions.

Isolation controls should cover:

- tenant-qualified namespaces;
- authentication and Authorization;
- key and signing context;
- storage and cache keys;
- rate limits and quotas;
- callbacks and subscription filters;
- diagnostics and error detail;
- export and deletion scope;
- administrative operations.

An unqualified identifier that is unique only within one tenant must not be resolved globally without its namespace.

SDK client reuse across tenants must require explicit context switching and prevent credential, cache, or idempotency leakage.

---

# 16.56 SDK Safe Defaults

SDKs should make the safest conforming behavior the easiest path.

Safe defaults may include:

- strict parsing and unknown-critical rejection;
- explicit finalization before signing;
- secure algorithm allowlists;
- exact version and profile binding;
- disabled silent downgrade;
- bounded retries only for safe operations;
- integrity validation on resolution;
- rich outcome types;
- tenant-scoped caches;
- secret-safe diagnostics;
- explicit opt-in for lossy conversion or insecure test modes.

Defaults must be documented and versioned where behavior affects interoperability.

Convenience methods must expose material limitations. A one-call helper may orchestrate several lifecycle stages, but it must return evidence and state for each protocol-significant boundary.

---

# 16.57 Extensions, Plugins, and Adapters

SDKs and services may support custom object types, algorithms, resolvers, transports, storage providers, Witnesses, or Verification checks through extension points.

Extensions must declare:

- stable namespace and version;
- criticality;
- input and output types;
- canonicalization and cryptographic behavior;
- trust and privilege boundary;
- failure and unsupported semantics;
- conformance and test material.

Unknown critical extensions must fail safely.

Plugins processing untrusted evidence should be isolated and resource-bounded. A proprietary adapter may implement a binding, but it must not silently change Core semantics or make the originating vendor the only possible verifier.

---

# 16.58 Conformance and Test Vectors

API and SDK conformance material should cover:

- normative-source and version behavior;
- object construction, finalization, and canonical bytes;
- Submission, Acceptance, Commitment, and receipts;
- retry, idempotency, replay, concurrency, and timeouts;
- batch atomicity and partial failure;
- authentication and Authority separation;
- error and transport-status mapping;
- capability and version negotiation;
- media types and representation conversion;
- resolution, pagination, streaming, and callbacks;
- export, offline operation, privacy, and resource limits;
- Verification and Trust Profile outcome preservation.

Tests should include positive, negative, boundary, adversarial, cross-version, and cross-implementation cases.

An SDK must be tested against independent conforming services, and a service must be usable without the reference SDK.

---

# 16.59 Deployment and Compatibility Patterns

Protocol APIs may be deployed as:

- embedded libraries;
- local sidecars;
- organizational gateways;
- centralized services;
- federated services;
- public multi-tenant platforms;
- offline tools;
- hybrid combinations.

Deployment changes trust, availability, latency, privacy, and correlated-failure assumptions. It does not alter Core protocol meaning.

Compatibility strategies may include additive optional fields, parallel binding versions, explicit adapters, capability negotiation, migration windows, and preserved historical endpoints or package readers.

Breaking semantic changes require explicit new versions. Compatibility layers must not translate unsupported mandatory meaning into apparent success.

---

# 16.60 Anti-Patterns and Relationship to Other Controls

The following are architectural anti-patterns:

## SDK as Specification

Treating one library's current behavior as normative protocol meaning.

## HTTP Success as Commitment

Reporting evidence as committed because an endpoint returned `200` or `202`.

## API Credential as Authority

Treating network access or token possession as unlimited business Authorization.

## Wire Bytes as Canonical Bytes

Signing or hashing an incidental transport representation without governed canonicalization.

## URL as Identity

Using a mutable endpoint or storage location as the Protocol Object identifier.

## Boolean Verification

Reducing rich Verification Outcomes and profile results to one success flag.

## Blind Automatic Retry

Repeating a non-idempotent operation after an ambiguous timeout without status reconciliation.

## Idempotency-Key Collision Suppression

Returning an earlier result when the same key is reused with different protected content.

## Last Write Wins

Silently resolving protocol conflicts through database arrival order.

## Batch Result Flattening

Reporting an entire batch successful when members differ.

## Silent Version Fallback

Choosing an older protocol, weaker algorithm, or incomplete profile without explicit agreement and result impact.

## Current Dependency Substitution

Resolving current keys, Profiles, Policies, or Registries when historical versions are required.

## Pagination Equals Completeness

Treating an exhausted mutable listing as proof that every relevant object was returned.

## Webhook as Proof

Treating a callback payload as authoritative historical evidence without validating the referenced artifact.

## Logging as Accountability

Using traces or application logs instead of typed, protected evidence for accountability-critical transitions.

## Proprietary Export

Requiring one vendor's SDK or production database to interpret supposedly portable evidence.

Protocol APIs and SDKs connect the architecture:

```text
Applications and Agents      request accountable operations
SDKs                         provide safe language abstractions
Transport Bindings           carry versioned operations
Protocol Objects             preserve typed evidence
Commitment Services          create historical state and receipts
Witness and Checkpoint APIs  expose independent evidence
Resolvers                    retrieve governed dependencies
Preservation and Pack APIs   support durable portability
Verification APIs            produce bounded outcomes
Trust Profiles               select required controls
```

APIs and SDKs expose these mechanisms. They do not replace their evidence, trust, lifecycle, or Verification semantics.

---

# API and SDK Invariants

### INV-API-001 — Protocol/API Separation

TAIP protocol meaning MUST remain distinguishable from the API operation used to expose it.

### INV-API-002 — Protocol/Transport Separation

Protocol outcomes MUST remain distinguishable from HTTP, RPC, queue, stream, local-call, or other transport outcomes.

### INV-API-003 — Specification/SDK Separation

SDK, reference implementation, generated client, or product behavior MUST NOT become the normative source of Core protocol semantics.

### INV-API-004 — Semantic Representation Independence

Equivalent conforming representations MUST preserve the same logical protocol meaning and mandatory semantics.

### INV-API-005 — Wire/Canonical Separation

Wire, stored, rendered, encrypted, compressed, and canonical cryptographic bytes MUST remain distinguishable.

### INV-API-006 — Identity/Locator Separation

Protocol Object identity, digest, operation identity, API URL, and storage or network locator MUST remain distinct.

### INV-API-007 — Authentication/Authority Separation

API authentication MUST NOT automatically establish Protocol Identity, Key Purpose, business Authority, Policy compliance, or claim truth.

### INV-API-008 — Lifecycle Separation

Draft, finalized, submitted, accepted, committed, witnessed, checkpointed, anchored, preserved, and verified states MUST remain distinct across interfaces.

### INV-API-009 — Response/Member Separation

Success of a response envelope or batch MUST NOT automatically imply success of every operation, object, member, control, or claim it contains.

### INV-API-010 — Submission/Acceptance/Commitment Separation

Delivery, Submission, Acceptance, and Commitment MUST NOT be represented as equivalent states.

### INV-API-011 — Identifier-Dimension Separation

Operation ID, Protocol Object ID, ERID, idempotency key, correlation ID, Chain Entry ID, and receipt ID MUST remain semantically distinct.

### INV-API-012 — Retry/Replay Separation

Legitimate retransmission, a new accountability-relevant attempt, intentional repetition, duplicate delivery, and malicious replay MUST remain distinguishable.

### INV-API-013 — Duplicate/Conflict Separation

Delivery of identical protected content MUST remain distinguishable from reuse of an identifier or idempotency key with conflicting content.

### INV-API-014 — Batch Atomicity Explicitness

Batch atomicity, subgroup atomicity, per-member processing, and aggregate Commitment MUST remain explicit.

### INV-API-015 — Timeout/Outcome Separation

A client timeout, cancellation, or lost response MUST NOT automatically be interpreted as proof that no protocol transition occurred.

### INV-API-016 — Error Fidelity

Malformed, unsupported, unauthenticated, unauthorized, invalid, conflicting, incomplete, unavailable, indeterminate, rate-limited, and internal-failure states MUST NOT be silently conflated.

### INV-API-017 — Version-Dimension Separation

API, binding, TAIP, object, schema, canonicalization, algorithm, Registry, profile, extension, package, and SDK versions MUST remain distinguishable where they affect interpretation.

### INV-API-018 — No Silent Negotiation Downgrade

Version or capability negotiation MUST NOT silently select weaker, older, lossy, or incomplete semantics.

### INV-API-019 — Resolution/Verification Separation

Successful retrieval from a Resolver MUST NOT automatically establish returned identity, integrity, Authority, historical applicability, or Validity.

### INV-API-020 — Historical/Current Separation

Current API, key, Policy, profile, Registry, or resolver state MUST NOT silently replace required historical state.

### INV-API-021 — Pagination/Completeness Separation

Completion of pagination or collection traversal MUST NOT automatically establish evidence Completeness.

### INV-API-022 — Stream/Protocol-Order Separation

Delivery order, queue offset, page order, and callback order MUST NOT automatically establish causal, Chain, Commitment, or event order.

### INV-API-023 — Notification/Evidence Separation

A callback, webhook, push event, or dashboard state MUST NOT automatically substitute for the referenced Protocol Object, receipt, or proof.

### INV-API-024 — Observability/Evidence Separation

Operational logs, metrics, and traces MUST NOT automatically be represented as Accountability Evidence.

### INV-API-025 — Offline/Protocol-State Separation

Locally queued, cached, or synchronized state MUST remain distinguishable from state accepted or committed by a defined protocol role.

### INV-API-026 — Conformance Independence

Service conformance MUST be testable without requiring one reference SDK, and SDK conformance MUST be testable against independent services.

### INV-API-027 — Extension Containment

Extensions, plugins, and adapters MUST NOT silently redefine Core semantics or cause unknown mandatory meaning to be ignored.

### INV-API-028 — Tenant Isolation

Tenant identity, Authority, keys, objects, operations, idempotency state, caches, streams, and exports MUST remain isolated under shared infrastructure.

### INV-API-029 — Privacy Proportionality

API convenience, debugging, observability, or conformance MUST NOT require unnecessary collection, retention, linkage, or disclosure.

### INV-API-030 — Portable Evidence

Core evidence required for independent Verification MUST NOT depend solely upon ephemeral API state, proprietary dashboards, or undocumented SDK data structures.

### INV-API-031 — Reproducible Outcomes

Equivalent protocol inputs and equivalent governed contexts MUST produce equivalent canonical outcomes across conforming bindings and implementations.

### INV-API-032 — Implementation Neutrality

Normative API and SDK architecture MUST NOT depend upon one language, framework, transport, vendor, cloud, database, or deployment topology.

---

# Architectural Requirements

### REQ-API-001

TAIP binding specifications MUST identify the exact Core protocol semantics, versions, operations, representations, and outcome mappings they expose.

### REQ-API-002

Bindings MUST distinguish transport handling from protocol operation state and MUST NOT derive protocol success solely from a generic transport status.

### REQ-API-003

SDKs and reference implementations MUST identify their implemented TAIP, binding, schema, Registry, algorithm, and Trust Profile versions.

### REQ-API-004

Undocumented SDK defaults or implementation behavior MUST NOT be required to interpret canonical Protocol Objects or outcomes.

### REQ-API-005

Normative-source precedence and conflict behavior MUST be governed, versioned, and explicit.

### REQ-API-006

Every protocol-relevant operation MUST have a stable operation type, explicit input scope, defined lifecycle, and machine-readable terminal and non-terminal outcomes.

### REQ-API-007

Operation ID, target object ID, idempotency key, correlation ID, Chain Entry ID, and receipt ID MUST use distinct fields or typed semantics.

### REQ-API-008

Protocol-significant request values MUST be included in or integrity-bound to the governed request, object, or resulting evidence.

### REQ-API-009

Transport metadata MUST NOT silently supply canonical object meaning, cryptographic input, Authority, Policy, or lifecycle state.

### REQ-API-010

Response envelopes MUST expose operation state, member-level outcomes, stable error codes, warnings, limitations, and receipt or status references where applicable.

### REQ-API-011

Human-readable messages MUST NOT replace stable machine-readable protocol, error, and outcome semantics.

### REQ-API-012

Object-creation interfaces MUST distinguish caller-supplied, service-derived, defaulted, normalized, and omitted protected properties.

### REQ-API-013

A service that changes protected object content MUST identify the transformation and attributable producer or co-producer role.

### REQ-API-014

Finalization interfaces MUST establish the immutable protected content intended for identifier calculation, canonicalization, digesting, signing, Submission, or Commitment.

### REQ-API-015

Canonicalization interfaces MUST identify exact object type, version, method, included properties, normalization, and domain-separation rules.

### REQ-API-016

Bindings and SDKs MUST distinguish canonical bytes from wire, storage, presentation, encrypted, compressed, and package representations.

### REQ-API-017

Signing interfaces MUST bind exact canonical input, algorithm, parameters, KID, Key Purpose, signer role, object type, version, and Signature encoding.

### REQ-API-018

Signing services MUST evaluate applicable caller Authority and MUST NOT treat possession of an API credential as unlimited signing permission.

### REQ-API-019

Submission interfaces MUST identify the submitted material, receiver, target namespace, requested operation, correlation context, and Submission state.

### REQ-API-020

A transport delivery acknowledgment MUST NOT be represented as TAIP Acceptance or Commitment unless the applicable binding explicitly supplies the required evidence.

### REQ-API-021

Acceptance responses MUST identify admission checks performed, checks deferred, applicable rules, and rejection or pending reasons.

### REQ-API-022

Acceptance MUST NOT be represented as Commitment, Witnessing, Checkpointing, anchoring, Preservation, Verification, or business approval.

### REQ-API-023

Commitment interfaces MUST identify committed material, target Chain or namespace, expected predecessor where applicable, atomicity, ordering, and proof semantics.

### REQ-API-024

An interoperable Commitment claim MUST be supported by a valid Chain Entry, Commitment Receipt, or other TAIP-defined artifact sufficient for the claim.

### REQ-API-025

Asynchronous responses MUST identify operation ID, accepted-request integrity, current state, status retrieval, expiry, cancellation semantics, and terminal outcomes.

### REQ-API-026

A durable receipt MUST bind to the exact original request or object and the completed protocol state it asserts.

### REQ-API-027

Idempotency-key scope MUST identify caller or tenant, operation type, target service, request integrity, validity interval, and collision behavior.

### REQ-API-028

Reuse of an idempotency key with different protected content MUST produce an explicit conflict and MUST NOT return an unrelated prior result as success.

### REQ-API-029

SDK automatic retry MUST be limited to operations whose idempotency and irreversible-state boundaries make that retry safe under the binding.

### REQ-API-030

After an ambiguous timeout, clients SHOULD reconcile operation and receipt state before retrying a potentially non-idempotent or irreversible operation.

### REQ-API-031

Duplicate handling MUST distinguish identical retransmission, conflicting identity reuse, intentional repetition, new attempts, and malicious replay.

### REQ-API-032

Concurrency-sensitive interfaces MUST support or define state preconditions and MUST report conflicts rather than silently apply last-write-wins behavior.

### REQ-API-033

Batch interfaces MUST define atomicity, member identity, ordering, partial-failure, duplicate, inclusion-proof, and retry semantics.

### REQ-API-034

Batch responses MUST preserve every member's accepted, rejected, duplicate, conflicting, pending, committed, or indeterminate state.

### REQ-API-035

Status APIs MUST distinguish operational progress from protocol lifecycle states and MUST identify authoritative evidence for each claimed state.

### REQ-API-036

Bindings MUST define stable categories for malformed, unsupported, unauthenticated, unauthorized, invalid, conflicting, incomplete, unavailable, indeterminate, rate-limited, pending, and internal-failure results.

### REQ-API-037

Transport-status mappings MUST preserve structured protocol outcomes and MUST NOT cause every nominal transport success to be interpreted as protocol Validity.

### REQ-API-038

Authentication results MUST identify the authenticated identity and credential context and MUST remain distinct from Protocol Identity, signer identity, and business Authority.

### REQ-API-039

Authorization interfaces MUST evaluate operation, resource, tenant, role, delegation, Policy, Trust Profile, lifecycle, and applicable limits.

### REQ-API-040

Delegated or Agent requests MUST preserve delegator, delegate, scope, purpose, effective interval, revocation state, and onward-delegation rules where applicable.

### REQ-API-041

Implementations MUST represent binding, TAIP, object, schema, canonicalization, algorithm, Registry, Trust Profile, extension, package, and SDK version dimensions separately where interpretation depends upon them.

### REQ-API-042

Version negotiation MUST produce an explicit effective context and MUST reject unsupported mandatory semantics rather than silently fall back.

### REQ-API-043

Negotiation MUST NOT silently select a weaker algorithm, older unsafe protocol, lower Trust Profile, or lossy representation.

### REQ-API-044

Capability documents SHOULD identify supported operations, types, versions, representations, algorithms, profiles, extensions, features, limits, and conformance suites.

### REQ-API-045

Capability assertions materially affecting negotiation MUST be authenticated, integrity-protected, freshness-bounded, and preservable where historical interpretation requires them.

### REQ-API-046

Every supported representation MUST define deterministic mapping to logical protocol meaning, unknown and duplicate member behavior, limits, and canonicalization relationship.

### REQ-API-047

Lossy, redacted, or derived representations MUST be explicitly identified and MUST NOT retain canonical identity or completeness claims without governed proof.

### REQ-API-048

APIs MUST return stable Protocol Object identifiers separately from URLs, operation handles, storage locators, and convenience links.

### REQ-API-049

Resolver responses MUST identify requested namespace, returned identity, type, version, integrity, source role, retrieval state, and conflicts where applicable.

### REQ-API-050

Resolved dependencies MUST be validated for identity, integrity, applicability, Authority, and historical boundary before use.

### REQ-API-051

Pagination MUST define collection identity, ordering, snapshot or live-view behavior, cursor semantics, concurrent-change behavior, and completion limitations.

### REQ-API-052

Exhausted pagination MUST NOT be represented as evidence Completeness without a governed population and consistency boundary sufficient for that claim.

### REQ-API-053

Streaming interfaces MUST define identity, partitioning, ordering, delivery, resume, duplicate, gap, truncation, backpressure, and reconnect semantics.

### REQ-API-054

Callbacks and webhooks MUST provide authenticity, replay protection, event and delivery time semantics, operation references, and a method to retrieve authoritative evidence.

### REQ-API-055

Witness, Checkpoint, Anchor, Key Transparency, Preservation, and Verification APIs MUST return or reference independently verifiable governed artifacts for protocol claims.

### REQ-API-056

Export APIs MUST identify requested scope, dependencies, disclosure rules, package version, Completeness, and every omitted, redacted, unavailable, or external item through a governed Manifest.

### REQ-API-057

Verification APIs MUST preserve per-check, dependency, object, claim, Completeness, Trust Profile control, and aggregate outcomes where applicable.

### REQ-API-058

Offline and synchronization interfaces MUST preserve actual local, Submission, Acceptance, Commitment, observation, and synchronization time semantics without backdating.

### REQ-API-059

Rate limits, resource limits, timeouts, cancellation, or partial processing MUST produce explicit non-success or bounded results and MUST NOT be disguised as complete Commitment or Verification.

### REQ-API-060

Shared implementations MUST isolate tenant namespaces, credentials, keys, objects, idempotency state, operations, caches, streams, exports, and diagnostics.

### REQ-API-061

SDKs SHOULD default to strict parsing, safe algorithms, explicit finalization, exact version binding, integrity validation, bounded retries, rich outcomes, and no silent downgrade.

### REQ-API-062

API and SDK conformance suites MUST include positive, negative, boundary, adversarial, timeout, retry, cross-version, cross-transport, and cross-implementation cases.

---

# Security Considerations

Protocol APIs and SDKs process untrusted content, exercise powerful roles, cross administrative boundaries, and mediate irreversible accountability transitions. Their convenience and reach make them important targets for semantic confusion, credential misuse, replay, downgrade, data leakage, and supply-chain compromise.

## Protocol/Transport Confusion

An attacker or defective client may present an HTTP, RPC, queue, or SDK success as proof of Acceptance or Commitment. Structured protocol states and portable receipts must remain authoritative.

## Canonicalization Differential

Client and server may derive different canonical bytes from the same visible object. Strict parsing, duplicate rejection, governed normalization, version dispatch, and cross-language vectors are required before signing or digesting.

## Signature Input Substitution

A signing helper may display one representation while signing another. SDKs should expose or bind exact canonical input, object identity, purpose, and protected fields, with human confirmation where risk requires it.

## Algorithm Downgrade

Negotiation or server fallback may select a weak algorithm or parameter. Allowlisted suites, explicit negotiated context, deprecation Policy, and rejection of unsafe fallback constrain downgrade.

## Version Confusion

One generic API version may conceal incompatible object, schema, profile, canonicalization, or Registry versions. Every interpretation-relevant dimension must remain explicit.

## Endpoint Substitution

DNS, configuration, service discovery, or dependency injection may direct a client to a malicious endpoint. Endpoint authentication helps, but returned objects and receipts still require identity, integrity, Authority, and applicability validation.

## Credential Theft

Stolen API keys, tokens, sessions, certificates, or workload credentials may permit powerful operations. Short lifetimes, least privilege, audience restriction, proof of possession, secure storage, rotation, revocation, and auditable use reduce exposure.

## Confused Deputy

A privileged SDK or gateway may perform an operation for an unprivileged caller under the gateway's own Authority. Explicit caller, delegator, tenant, purpose, resource, and Policy binding is required.

## Cross-Tenant Substitution

Shared caches, operation IDs, idempotency keys, resolvers, callbacks, or exports may return another tenant's data or result. Tenant-qualified namespaces and authorization checks must cover every layer.

## Idempotency-Key Poisoning

An attacker may preempt an idempotency key or reuse it with different content. Keys must be caller-scoped and bound to protected request integrity, with conflicting reuse surfaced.

## Replay

Valid signed or authenticated requests may be replayed outside their intended time, audience, nonce, state, or Chain context. Replay defenses must preserve legitimate retransmission while rejecting unauthorized repetition.

## Ambiguous Timeout Duplication

A client may retry an irreversible operation after losing the response, causing duplicate effects. Operation status, idempotency, expected-state preconditions, and receipt reconciliation are required.

## Last-Write-Wins Rewrite

Generic database or REST update behavior may overwrite protected or historically meaningful state. Protocol state transitions need explicit preconditions, append-only correction, and conflict evidence.

## Batch Smuggling

A valid batch may contain malformed, unauthorized, oversized, or semantically incompatible members. Each member and aggregate construction require validation; outer-envelope validity cannot authorize inner content.

## Partial-Failure Suppression

A service may report batch success while hiding rejected or indeterminate members. Member-level outcomes and proof bindings must remain accessible.

## Parser Exploitation

Malformed JSON, CBOR, archives, media types, compressed bodies, proofs, or extension data may exploit parsers. Implementations should use safe parsers, isolate risky codecs, reject ambiguity, patch dependencies, and enforce bounds before expensive work.

## Deserialization Gadgets and Active Content

SDKs and Pack readers must not instantiate arbitrary classes, execute scripts, expand macros, follow active links, or load plugins based solely on untrusted evidence metadata.

## Request Smuggling and Binding Ambiguity

Intermediaries may interpret lengths, headers, paths, encodings, or duplicated parameters differently. Bindings should use unambiguous framing and normalize only under specified rules.

## Resolver Poisoning

A compromised Resolver may return substituted keys, profiles, schemas, or Policies. Stable identity, integrity protection, authoritative-source rules, conflict handling, snapshots, and Historical Key State are necessary.

## Cache Poisoning

Unqualified cache keys may mix tenants, versions, Authorization scopes, or historical boundaries. Cache identity must include all context that affects correctness.

## Stale Capability Replay

An old capability document may cause unsafe negotiation or use of deprecated algorithms. Capability assertions need freshness, integrity, and revocation semantics.

## Webhook Forgery

Forged callbacks may cause clients to mark operations committed or trigger downstream actions. Notifications require authentication, replay resistance, audience binding, and verification of authoritative referenced evidence.

## Stream Gap Concealment

A dropped or truncated stream may hide corrections, revocations, conflicts, or checkpoints. Resume tokens, gap detection, consistency evidence, and reconciliation APIs are required.

## Pagination Manipulation

Mutable pagination may omit, duplicate, or reorder evidence. Snapshot semantics, authenticated cursors, stable ordering, and explicit Completeness limits reduce manipulation.

## Excessive Authorization

A generic administrative API may combine signing, committing, exporting, deleting, and key rotation. Least-privilege roles, separation of duties, scoped credentials, and distinct Key Purposes reduce compromise impact.

## SSRF and Resolver Reach

Evidence-controlled locators may induce server-side requests to internal services or metadata endpoints. Resolvers need scheme, namespace, network, size, redirect, and content restrictions.

## Decompression and Expansion Bombs

Compressed objects, archives, proofs, and recursive dependencies may expand beyond safe limits. Implementations must bound compressed and expanded sizes, entry counts, nesting, and processing time.

## Resource Exhaustion

Large batches, deep graphs, expensive cryptography, repeated status polling, long streams, or export jobs can exhaust resources. Quotas and limits must produce honest bounded outcomes.

## Error Oracle

Detailed errors may reveal whether identities, objects, keys, tenants, or confidential evidence exist. Interfaces should balance stable diagnostics with authorization-aware disclosure.

## Secret Leakage in Diagnostics

SDK exceptions, traces, request dumps, crash reports, and support bundles may expose tokens, keys, raw evidence, decryption material, or sensitive payloads. Secret-safe logging and redaction are mandatory operational practices.

## Supply-Chain Compromise

Generated clients, SDK packages, plugins, dependencies, build pipelines, and update channels may be compromised. Reproducible builds, signed releases, dependency pinning, provenance, review, and independent conformance testing reduce risk.

## Malicious Plugin

A resolver, algorithm, transport, or storage plugin may exfiltrate data or alter semantics. Plugins should be least-privileged, isolated, signed or approved, and prohibited from overriding Core outcomes.

## Insecure Test Mode

SDKs may expose options that disable Signature checks, TLS validation, strict parsing, or profile enforcement. Test modes must require explicit opt-in, remain visibly nonconforming, and be difficult to enable in production accidentally.

## Downgrade Through Convenience Method

A high-level SDK method may fall back to unsigned creation, producer-only storage, or a lower profile when a service is unavailable. Safe defaults must prohibit silent downgrade and return control-level status.

## Operation-ID Enumeration

Predictable operation IDs may expose state or evidence across callers. IDs should provide appropriate unpredictability or authorization checks and must be tenant-scoped.

## Cancellation Race

Cancellation may race with Commitment or external publication. The API must return the authoritative terminal state and must not claim reversal of an already irreversible transition.

## Time Manipulation

Client, service, or intermediary clocks may affect token expiry, replay windows, deadlines, event times, or profile rules. Time source, precision, trust, and ordering semantics must remain explicit.

## Receipt Substitution

A valid receipt for one object, Chain, or attempt may be attached to another response. Receipts must cryptographically bind exact target, operation, context, and resulting state.

## Verification Result Flattening

SDKs may convert rich results into `true`, hiding Unsupported, Incomplete, or Conflicting state. Canonical outcomes and material details must propagate through typed interfaces.

---

# Privacy Considerations

APIs can expose not only evidence content but also who is creating, resolving, witnessing, preserving, exporting, or verifying it. Privacy must cover payloads, metadata, access patterns, errors, caches, and operational telemetry.

## Data Minimization

Requests and responses should contain only information required by the operation, claim, Policy, and Trust Profile. Generic `send everything` interfaces increase compromise and disclosure risk.

## Metadata Leakage

Headers, URLs, operation types, profile identifiers, object IDs, timestamps, sizes, and status polling may reveal sensitive activity even when payloads are encrypted.

## Resolver Query Privacy

Queries for identities, keys, Policies, profiles, or historical periods reveal investigative and relationship information. Local mirrors, batching, private retrieval, offline Packs, and minimized queries may reduce leakage.

## Witness Relationship Disclosure

Observation requests and quorum retrieval can expose counterparties, Control Domains, and business relationships. Profiles should require only the disclosure needed to evaluate eligibility and independence.

## Tenant Linkability

Global operation, correlation, or idempotency identifiers may link activities across tenants or services. Scope identifiers to the domain needed for Verification.

## Pagination and Enumeration

Collection APIs may enable bulk discovery of identities, objects, or events. Authorization, query limits, non-enumerable identifiers, purpose controls, and bounded result sets reduce abuse.

## Streaming Surveillance

Real-time subscriptions can expose organizational behavior at high resolution. Stream scope, retention, replay, subscriber Authority, and downstream use should be governed.

## Callback Disclosure

Webhook destinations and payloads may expose evidence to third-party infrastructure or logs. Callbacks should minimize content and use protected retrieval for sensitive artifacts.

## Error Disclosure

Differences among not found, unauthorized, wrong tenant, revoked, and redacted can reveal protected existence. Binding semantics may preserve internal diagnostic precision while returning appropriately minimized external detail.

## SDK Telemetry

SDKs must not transmit usage, object types, identifiers, errors, evidence, or environment information to a vendor without explicit governed disclosure and user control.

## Local Cache Exposure

SDK caches may retain evidence, keys, profiles, resolver results, and reports after application use. Encryption, access control, expiry, tenant separation, and secure deletion should match sensitivity and Evidence Lifetime.

## Debug Logging

Request and response bodies, canonical bytes, bearer tokens, Signatures, or decrypted Pack entries should not be logged by default. Diagnostic modes need explicit scope and retention.

## Export Overbreadth

An export or Dispute Pack may include more history and dependencies than the focal claim requires. Claim-bounded selection, compartments, redaction, and Manifest disclosure reduce unnecessary exposure.

## Verification Report Sensitivity

Reports can reveal control failures, key incidents, organizational structure, disputed claims, and missing evidence. Detailed and derived reports may use different access scopes while preserving canonical consistency.

## Retention of Operation State

Operation records, idempotency mappings, callbacks, and status histories should have explicit retention. They must not become indefinite shadow histories outside Preservation and deletion governance.

## Deletion and Commitments

Deleting API-accessible content may leave historical commitments and metadata. Interfaces must not promise erasure of immutable evidence that the protocol preserves, and should explain the governed distinction.

## Authentication Privacy

Authentication mechanisms may reveal stable identity to every service. Scoped credentials, pairwise identities, delegated tokens, and minimized claims can reduce cross-service correlation.

## Selective Disclosure

APIs should support governed redacted, committed, encrypted, or proof-based representations where they satisfy the claim. Selective disclosure must not hide missing mandatory evidence.

## Rate-Limit Side Channels

Shared quotas and timing can reveal another tenant's activity. Tenant isolation, constant-shape error handling where feasible, and separate quotas reduce cross-tenant inference.

## Offline Privacy

Offline Verification and local dependency bundles can reduce network-query leakage, but removable media and local archives require encryption, custody, and deletion controls.

---

# Design Rationale

TrustAgentAI separates protocol semantics from APIs and SDKs because interfaces evolve faster than accountability meaning.

HTTP frameworks, RPC systems, cloud services, programming languages, and developer ergonomics change frequently. Evidence may need to remain interpretable for years. If an endpoint shape, library default, database field, or product dashboard defines meaning, historical Verification becomes dependent upon the original vendor and software version.

The architecture therefore follows **Protocol Before Product**.

TAIP defines object, lifecycle, evidence, trust, and outcome semantics. A binding maps those semantics to a transport. An SDK makes the binding safe and usable in a language. A product presents workflows to people and Agents. Each layer may innovate without silently changing the layer below it.

Submission, Acceptance, and Commitment receive special treatment because distributed systems commonly conflate them. A network response can show that bytes moved or a server queued work. It cannot, without the applicable proof, establish that evidence entered protected history.

Retry and idempotency rules are equally important. Accountability systems must tolerate duplicate delivery without duplicating exclusive actions, while preserving distinct attempts that matter to causality. This requires typed identifiers, content binding, state reconciliation, and explicit conflict behavior.

Rich errors and outcomes prevent convenience from creating false certainty. Unsupported semantics are different from invalid evidence. Unavailable dependencies are different from contradictions. A timeout is different from confirmed failure. An API or SDK that collapses those distinctions would undo the Verification model.

Representation independence permits JSON, CBOR, packages, streams, and future encodings while maintaining one governed logical object and canonical cryptographic input. Identity remains separate from URLs so evidence survives migration and export.

Independent SDKs and services are a deliberate conformance goal. A reference implementation is useful for adoption and test material, but it must not become an oracle that only itself can satisfy. Interoperability requires cross-language, cross-service, negative, boundary, and adversarial testing.

Finally, safe SDK defaults reduce integration error without shifting normative authority. Libraries should make strict parsing, exact versions, safe algorithms, bounded retry, explicit finalization, rich outcomes, and portable evidence easy. They must still expose the underlying protocol boundaries instead of hiding them behind a misleading one-call success result.

---

# Summary

Protocol APIs carry TAIP operations. Transport bindings map those operations to communication mechanisms. SDKs provide language- and platform-specific convenience. Products build experiences on top of them.

These layers must preserve:

- TAIP as the normative source of Core meaning;
- transport status separately from protocol outcome;
- Submission, Acceptance, Commitment, and later lifecycle states;
- object, operation, correlation, idempotency, and receipt identifiers;
- retry, replay, concurrency, batching, and partial-failure semantics;
- authentication separately from Authority and Policy;
- exact version dimensions and downgrade-resistant negotiation;
- wire, canonical, storage, encrypted, and rendered representations;
- object identity separately from URLs and locators;
- resolver retrieval separately from Verification;
- pagination, streams, and callbacks separately from Completeness and proof;
- portable receipts, exports, Dispute Packs, and Verification Reports;
- privacy, tenant isolation, safe resource limits, and honest uncertainty;
- independent conformance across SDKs, services, transports, and vendors.

The governing relationship is:

```text
TAIP Semantics
      │ implemented by
Transport Bindings
      │ made usable by
SDKs
      │ incorporated into
Products and Agent Workflows
```

No higher layer may silently redefine a lower layer.

An API call is not evidence merely because it succeeded.

An SDK result is not normative merely because it is convenient.

Accountability depends upon the governed Protocol Objects, receipts, dependencies, lifecycle states, Trust Profiles, and Verification Outcomes that remain portable across all of them.
