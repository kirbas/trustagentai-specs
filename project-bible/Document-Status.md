# TrustAgentAI Project Bible — Document Status

## Current working status — September 24, 2026

The Project Bible is reference material describing the target architecture. Its expansion, together with expansion of TAIP, is paused for the 30-day cycle in the [first external integration plan](../docs/FIRST_EXTERNAL_INTEGRATION.md). Corrections, pilot blockers, and confirmed security issues remain in scope.

| Material | Current role |
| --- | --- |
| Project Bible | Architectural reference; existing requirements describe design intent, not demonstrated implementation capabilities |
| TAIP | Experimental drafts with obligations within their stated scope; no stable or generic conformance implied |
| Implementation | Supported behavior must be established from an identified commit, package version, evidence-format version, tests, and reproducible instructions |
| Integration plan | Current work scope and acceptance criteria; the external milestone is not yet achieved |

Retain **Proof, not logs**, the three-phase mechanism, inline witness, explicit degraded behavior, and Dispute Hardening discipline as the integration focus. Validate each claimed capability in the selected implementation. Signature validity is not authorization; intended assurance is not achieved assurance.

Normative language in historical architecture and TAIP drafts is preserved. This status update does not assert that existing software implements those requirements or change the historical meaning of published evidence.

[Repository navigation](../README.md) · [TAIP draft index](../spec/README.md)

## Original architectural description

The source below was introduced as an incomplete document with an unclosed code fence. The history reachable from `main` contains only that initial addition for this path. This update closes the fence and adds the current status above; it does not reconstruct missing original sections.

## Status of This Document

The **TrustAgentAI Project Bible** is the architectural foundation of the TrustAgentAI project.

It defines the long-term concepts, principles, trust assumptions, system boundaries, evidence model, security objectives, and architectural requirements from which the **TrustAgentAI Interoperability Protocol (TAIP)** and related specifications are derived.

This document is intended to remain more architecturally stable than individual protocol versions or software implementations.

---

## Document Role

The TrustAgentAI documentation model separates architecture from normative interoperability specifications.

```text
TrustAgentAI Project Bible
          │
          ▼
Architectural Principles
          │
          ▼
Architectural Requirements
          │
          ▼
TAIP
          │
          ├── Trust Profiles
          ├── Registries
          ├── Schemas
          ├── Test Vectors
          └── Reference Bindings
                  │
                  ▼
             Implementations
```

