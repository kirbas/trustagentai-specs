# TrustAgentAI — Proof, not logs

TrustAgentAI explores portable, independently verifiable evidence for consequential AI-driven actions. This repository contains architectural reference material and experimental TAIP specifications.

**Current milestone: an external team connects one action to the mechanism and independently verifies the exported evidence.**

## Start here

1. [First external integration — scope, acceptance criteria, and 30-day plan](docs/FIRST_EXTERNAL_INTEGRATION.md)
2. [Document status — architecture, drafts, and implementation claims](project-bible/Document-Status.md)
3. [TAIP drafts — status and navigation](spec/README.md)

Expansion of the Project Bible and TAIP is paused for the 30-day integration cycle. Corrections, pilot blockers, and confirmed security issues remain in scope. Existing architectural requirements are preserved as design reference; they are not evidence that a released implementation satisfies them.

## Implementation status

This is a documentation repository, not an SDK or a runnable integration. The implementation repository, reproducible commit, package version, evidence-format version, and tested setup/verification commands must be recorded at pilot kickoff. They have not been verified for this documentation update.

The integration plan retains Intent → Acceptance → Execution, inline witness, explicit degraded behavior, and the threat-model → decision → test discipline of Dispute Hardening. Each behavior must be confirmed in the selected implementation. An inline co-signer does not automatically establish an independent witness, and observing an action does not establish enforcement.

**Proof, not logs** describes the objective: verify defined evidence claims with explicit trust assumptions. Cryptographic integrity does not by itself establish authorization, complete history, or the truth of a business event.

## Project Bible navigation

The Bible is a reference architecture, not a prerequisite reading list for a pilot.

| Start | Reference |
| --- | --- |
| Context | [Preface](project-bible/Preface.md) · [Document status](project-bible/Document-Status.md) |
| Vocabulary | [Terminology](project-bible/Terminology.md) · [Acronyms](project-bible/Acronyms.md) |
| Foundations | [01 Philosophy](project-bible/01-Philosophy.md) · [02 Executive Summary](project-bible/02-Executive-Summary.md) · [03 Problem Statement](project-bible/03-Problem-Statement.md) |
| Architecture | [04 Design Principles](project-bible/04-Design-Principles.md) · [05 System Overview](project-bible/05-System-Overview.md) · [06 Protocol Objects](project-bible/06-Protocol-Objects.md) |
| Evidence and history | [07 Evidence Record](project-bible/07-Evidence-Record-Specification.md) · [08 Hash Chain](project-bible/08-Hash-Chain-Specification.md) · [09 Witness Observation](project-bible/09-Witness-Observation-Specification.md) |
| Historical assurance | [10 Checkpoints and External Anchors](project-bible/10-Checkpoints-and-External-Anchors.md) · [11 Key Transparency](project-bible/11-Key-Transparency.md) · [12 Preservation](project-bible/12-Preservation.md) |
| Verification | [13 Dispute Packs](project-bible/13-Dispute-Packs.md) · [14 Verification](project-bible/14-Verification.md) · [15 Trust Profiles](project-bible/15-Trust-Profiles.md) |
| Implementation boundaries | [16 Protocol APIs and SDK Boundaries](project-bible/16-Protocol-APIs-and-SDK-Boundaries.md) · [17 TAIP Mapping](project-bible/17-TAIP-Mapping-and-Normative-Specification-Boundary.md) |
| Reference governance | [18 Governance, Versioning, and Compatibility](project-bible/18-Governance-Versioning-and-Compatibility.md) · [19 Invariant and Requirement Index](project-bible/19-Global-Invariant-and-Requirement-Index.md) · [20 Conclusion](project-bible/20-Conclusion-Proof-Not-Logs.md) |

## Known documentation limitations

- Preface, Document Status, and Terminology were originally committed with unclosed code fences. This update repairs the formatting and identifies the surviving source as incomplete; it does not invent missing original text.
- Two Chapter 10 drafts exist. Navigation above uses the filename already referenced by Chapter 16. The [alternative Chapter 10 draft](project-bible/10-Checkpoint-and-External-Anchor-Specification.md) remains available. No semantic reconciliation or formal supersession is claimed in this update.
- TAIP documents are experimental drafts. Normative language within a draft does not establish stable conformance or implemented behavior.

