# TrustAgentAI — Proof, not logs

TrustAgentAI is developing infrastructure to collect, independently witness, preserve, and verify evidence of agent actions across systems.

The product goal is a portable evidence package linking authorization, an agent's request, witness observations, and the external system's response. A verifier should be able to evaluate each claim and see missing evidence and trust assumptions.

**Current milestone: one external team connects one action and independently verifies its exported evidence.**

## Start here

1. [Product direction and first external integration — scope, acceptance criteria, and 30-day plan](docs/FIRST_EXTERNAL_INTEGRATION.md)
2. [Document status — architecture, drafts, and implementation claims](project-bible/Document-Status.md)
3. [TAIP drafts — status and navigation](spec/README.md)

## Product focus

| Capability | Intended pilot result |
| --- | --- |
| Collect | Link an authorization decision, request, and external API response using stable identifiers; retain original source artifacts |
| Independently witness | Attest to specified received artifacts or commitments, with the witness operator and control domain disclosed |
| Preserve | Retain evidence and verification dependencies across restarts; make configured retention, access, and historical commitments explicit |
| Verify | Export a package and explain which claims are supported, contradicted, or inconclusive without access to TrustAgentAI's internal database |

The initial customer hypothesis is an AP automation or B2B finance platform with an agent-enabled workflow. The proposed pilot action is creating a supplier payout request in a partner's sandbox. Request creation and payment settlement are different claims.

Existing signed artifacts, including AP2 mandates and receipts, are potential evidence sources. Protocol adapters and compatibility are planned work unless supported by a tested release. We do not claim that portable dispute evidence or cryptographically protected logs are unique to TrustAgentAI.

Expansion of the Project Bible and TAIP remains paused during the integration cycle. Corrections, pilot blockers, and confirmed security issues remain in scope. The Bible is architectural reference, not evidence of implemented capabilities.

## Implementation status

This repository contains documentation and experimental specifications. The candidate implementation is [trustagent-a2a-protocol](https://github.com/kirbas/trustagent-a2a-protocol). A source-only review used [commit 8493f533](https://github.com/kirbas/trustagent-a2a-protocol/tree/8493f5330fec94914dd8d92ee648f49fe9b1477d); runtime behavior and a deployable pilot release were not validated by that review.

Before integration, record a reproducible implementation commit, package version, evidence-format version, setup and verification commands, and passing checks for every selected component. The [working plan](docs/FIRST_EXTERNAL_INTEGRATION.md) lists readiness gates. No completed external pilot is recorded in this repository.

Retain Intent → Acceptance → Execution, scoped witness observations, explicit degraded behavior, and threat model → decision → test. Observation does not establish enforcement; an inline co-signer does not automatically establish independent control.

**Proof, not logs** describes bounded, independently evaluable claims. A valid signature does not by itself establish authorization, a complete history, or the truth of a business event. A witnessed request does not establish successful execution.

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

- Preface, Document Status, and Terminology were originally committed with unclosed code fences. An earlier documentation update repaired the formatting and identified the surviving source as incomplete; it did not invent missing original text.
- Two Chapter 10 drafts exist. Navigation above uses the filename already referenced by Chapter 16. The [alternative Chapter 10 draft](project-bible/10-Checkpoint-and-External-Anchor-Specification.md) remains available. No semantic reconciliation or formal supersession is claimed in this update.
- TAIP documents are experimental drafts. Normative language within a draft does not establish stable conformance or implemented behavior.

