# TrustAgentAI — first external integration

Working decision · September 24, 2026 · 30 days from execution kickoff

[Repository overview](../README.md) · [Document status](../project-bible/Document-Status.md)

**Next outcome: an external team connects the mechanism to one of its actions and independently verifies the exported evidence.**

Status: direction agreed; external team and action not yet selected. This plan defines work and acceptance criteria. It does not establish current code readiness or an existing pilot.

## 1. Focus

Retain **Proof, not logs**, **Intent → Acceptance → Execution**, inline witness, explicit degraded behavior, and the Dispute Hardening discipline: threat model → decision → test.

Pause expansion of the Bible and TAIP for the 30-day cycle. Preserve the full documents as reference and target architecture. Corrections, work necessary for the pilot, and confirmed security issues remain in scope. New chapters, registries, and profiles are not independent milestones.

Describe supported release behavior using verified code, tests, and setup instructions. Confirm individual capabilities and any TAIP conformance separately. Track package and evidence-format versions separately.

## 2. Principles to retain

1. **Signature validity is not authorization.** Evaluate authority and applicable policy separately.
2. **Intended assurance is not achieved assurance.** Report actual guarantees; do not imply implemented Trust Profiles merely by naming them.
3. **Integrity is not completeness.** A valid object signature does not establish a complete history.
4. **Evidence verification is not business truth.** Identify the execution-result source and its trust limits.
5. **Replication is not independence.** Identify the witness operator, control domain, and precise attestation.
6. **Degradation must be visible.** Define stop/continue behavior for each failure; degraded mode must not bypass mandatory authorization.
7. **Evidence must be portable.** The external team repeats verification with explicit dependencies and trusted keys.

## 3. One partner, one action

Initial buyer hypothesis: a technical team already integrating an AI agent with write access through tools/APIs. Require an existing workflow, a named technical owner, and a control or incident-analysis problem the team considers material.

Choose one action in the partner's test environment, for example creating a payout request through its API. This is an illustrative scenario, not a committed market or a promise of bank-payment execution.

Before integration, record the action and side effect, authority source, policy, interception point, witness operator and role, execution-result source, failure rules, acceptable latency, and exportable data. Determine whether the agent can bypass the control point.

## 4. Minimum work

- Pin a reproducible implementation commit, package version, evidence-format version, and run command. Confirm what the inline witness actually does; do not treat co-signing and independent witnessing as equivalent.
- Connect one partner action to the three phases. Preserve the relationship between intent, admission decision, and outcome, including failed or unknown completion.
- Export an evidence package with a short verification command, explicit dependencies, and explained outcomes.
- Verify in the partner's environment without access to TrustAgentAI's internal database. List any required external services and trusted keys.

## 5. Acceptance criteria

| Check | Observable result |
| --- | --- |
| Allowed action | Action completes; evidence links the three phases; partner repeats verification |
| Denied action | No side effect occurs through the integrated path; evidence records the refusal reason |
| Witness unavailable | Agreed stop/continue rule is followed; lost guarantees are visible |
| Tampered package or missing required record | Verification reports failure or insufficient evidence, not full success |
| Portability | Partner verifies the exported package from instructions without founders operating each run |

If an integration only observes actions, label it as observation. The blocking criterion remains unmet; observation does not demonstrate enforcement.

Record integration time, founder assistance, added latency against a baseline without the integration, failure behavior, and verification time. Agree numeric limits with the partner before measurement.

## 6. Execution and ownership

| Timing | Work | Owner |
| --- | --- | --- |
| Days 1–3 | Reproducible release, mechanism checks, short instructions | TrustAgentAI technical owner, to be named at kickoff |
| Days 1–10 | Focused conversations; select team and action | Kirill, with technical participation for scenario review |
| Days 11–20 | Integrate and exercise failure cases in partner environment | Technical owners on both sides |
| Days 21–30 | Partner-led verification and continuation decision | Partner and founders |

Planning targets: 10 substantive conversations, two teams willing to discuss integration with a technical owner, and one completed external pilot. These are targets, not current traction. Once the first partner is selected, concentrate engineering effort on its scenario.

## 7. Completion and decision

**Technical completion:** acceptance criteria pass; retain the integration version, a sanitized evidence example, verification command, check results, and measurements. The partner confirms it repeated verification.

**Commercial evidence is separate:** record the problem solved, willingness to keep using the integration, the decision maker, and possible payment terms. A successful demonstration alone does not establish demand.

If no team has committed a specific action and time by day 10, revisit the offer and buyer hypothesis. If integration succeeds but continued use is unwanted, investigate before expanding the architecture.

**First action:** confirm release reproducibility and identify suitable external teams. The first checkpoint is a selected partner, action, and technical owner.

Basis: the agreed project direction and the existing Philosophy, Design Principles, and System Overview. The implementation repository was not audited for this documentation update. This document is the English working version of the agreed Russian integration plan.

