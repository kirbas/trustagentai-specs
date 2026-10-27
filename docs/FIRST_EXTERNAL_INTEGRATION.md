# TrustAgentAI — product direction and first external integration

Direction agreed October 26, 2026 · Supersedes the September 24 focus within this working plan · 30 days from cycle start

[Repository overview](../README.md) · [Document status](../project-bible/Document-Status.md)

**Product direction: collect, independently witness, preserve, and verify evidence of agent actions across systems.**

**Next outcome: an external team connects one action, verifies its exported evidence, and evaluates whether to continue using the product.**

Status: product direction agreed; partner, action, and cycle start remain unconfirmed. Capabilities and dates below are work targets, not claims of a released product, completed pilot, or customer demand.

## 1. Customer and value hypothesis

Start with an AP automation or B2B finance platform whose agents already call business APIs. The engineering user needs a reproducible integration; the proposed buyer is the CTO or operational/risk owner responsible for investigating disputed actions.

The proposed value is less manual evidence gathering and a clearer account of an operation across the agent, authorization system, and external API. Measure investigation time against the partner's current process. Do not promise reduced financial losses or successful dispute outcomes without evidence.

Initial action: **create a supplier payout request in the partner's sandbox**. One team, one workflow, one API adapter. A created request does not demonstrate an executed or settled bank transfer. Change this action only if a qualified partner brings a more concrete problem; record the reason.

A single platform should get useful evidence without requiring all counterparties to adopt TrustAgentAI. Label unsigned or unobserved counterparties and weaker claims explicitly.

## 2. Minimum product

| Capability | Deliverable | Boundary |
| --- | --- | --- |
| Collect | One adapter joins authorization evidence, the exact API request, and response using stable operation/source IDs | Preserve original signed bytes and source identity; normalization must not replace the material being verified |
| Independently witness | Signed observations bind specific artifacts or commitments and the time/scope of observation | A pre-execution observation covers intent/admission; an outcome claim requires separately sourced outcome evidence |
| Preserve | Recoverable evidence, historical key/policy material, and committed history, with access and retention rules | Witnessing a hash alone does not preserve its underlying record; define who retains each required artifact |
| Verify and explain | Versioned export bundle, one verification command, and a readable per-claim report | No internal database access; missing, unsupported, or conflicting evidence remains visible |

Retain the existing Intent → Acceptance → Execution model where supported. Add adapters and a usable verification/export path around the current core, with a small pilot profile of required fields and checks.

The partner must be able to find an operation by its own business ID, retrieve the linked artifacts, and re-export the package after a service restart. An API or CLI is sufficient for this pilot.

Accept existing signed artifacts where useful. AP2 mandates and receipts are a candidate adapter after the first partner's requirements are known, not a mandatory dependency of the initial pilot or a claim of implemented compatibility.

## 3. Evidence and trust boundaries

Before partner integration begins, record the action and side effect, authority source, policy version, interception point, result source, witness operator, key custody, storage operators, trusted roots, and any path that can bypass collection or enforcement.

- **Signature is not authorization:** bind the actor and key to the permitted action and relevant historical policy.
- **Observation is not enforcement:** an observer can report a denial or invalid evidence; only an integrated gate can prevent the side effect. State the mode before testing.
- **Recorded history is not all history:** describe covered sources and interval. Detecting an omitted tail or alternate history requires an independently retained commitment or equivalent evidence; a hash chain alone cannot establish completeness.
- **Witnessing is scoped:** a witness attests to what it received or checked. It does not certify supplier data, an agent's internal reasoning, or payment settlement it did not observe.
- **Independence is operational:** disclose common control of keys, deployment, administration, and storage. A separate process is not an independent witness. If operated under common control in a demo, label that limitation; independent-witness acceptance remains unmet.
- **Uncertainty is an outcome:** a timeout after submission may mean an unknown side effect. Preserve the operation/idempotency identifier and reconcile against the source system; do not blindly retry or call it a failed payment.
- **Late evidence stays late:** later reconciliation must not be presented as contemporaneous witnessing. Append the new status and retain the earlier report.

## 4. Minimal export and verification contract

The pilot bundle contains a versioned manifest, operation and source identifiers, hashes of included artifacts, original signed evidence, authorization/policy context, witness observations, execution-response evidence, relevant historical key material and commitments, and an explicit coverage/dependency list. Minimize sensitive data and agree permitted exports and access with the partner.

Keys included in the bundle are evidence, not automatically trusted roots. The verifier receives approved roots and trust policy separately, and identifies required external checks. For the portable verification target, the declared core checks must work without a live TrustAgentAI service; unavailable external facts remain inconclusive.

The report separates signature integrity, authorization, source linkage, witness scope, historical commitments, and execution outcome. Each claim is **supported**, **contradicted**, or **inconclusive**, with reasons and evidence references. Report execution state separately from proof validity. Do not use one green verdict to imply all claims are established.

## 5. Engineering order and readiness gates

The candidate implementation is [trustagent-a2a-protocol](https://github.com/kirbas/trustagent-a2a-protocol), source-review baseline [8493f533](https://github.com/kirbas/trustagent-a2a-protocol/tree/8493f5330fec94914dd8d92ee648f49fe9b1477d). That review did not execute tests or validate deployment. Confirm findings and readiness in the selected release.

| Priority | Work before the selected component enters the pilot | Completion evidence |
| --- | --- | --- |
| P0 | Validate authorization decisions and witness receipts before gated execution, including signature, binding, identity, and expiry | Invalid or mismatched artifacts never reach the side-effect callback |
| P0 | Separate verification denial from witness unavailability; define explicit stop/continue policy | Verification denial blocks gated execution; permitted outage degradation is visible and capped |
| P0 | Persist key history and applicable degradation counters; enforce revocation for new actions while evaluating old signatures historically | Restart and rotation/revocation tests pass |
| P0 | Separate demo administration from the deployed service; authenticate onboarding and protect evidence access | Production configuration exposes no destructive demo reset; access checks pass |
| P1 | Build the single adapter, versioned export, and independent verifier | Partner can repeat setup, collection, export, and verification |
| P1 | Produce a measured release and short onboarding guide | Pinned commit, package/format versions, commands, fixtures, and recorded results |

These are acceptance gates, not assertions that fixes exist. The schedule must move if the selected path fails a gate. Observation-only work must not be described as an enforcement pilot. Expansion of Bible chapters, new registries, broad connector catalogs, and additional trust profiles remains outside this cycle.

## 6. Pilot acceptance

| Check | Observable result |
| --- | --- |
| Allowed action | Request and source response are linked to authorization; the report describes the actual sandbox outcome |
| Denied action in gated mode | No side effect occurs through the controlled path; the refusal is retained. An observer-only integration cannot pass this enforcement check |
| Changed amount/payee, wrong signature or binding | Verification reports the failed check; the execution gate, if present, refuses before side effects |
| Witness denies verification | No degraded fallback to bypass the denial on a gated path; observation mode records a failed claim |
| Witness unavailable | Agreed stop/continue policy applies; lost guarantees remain visible. Absence of a witness is not success |
| Revocation, restart, concurrent requests | New actions using revoked authority are rejected; retained history is usable; configured limits survive restart and concurrency |
| Timeout after submission | Outcome remains unknown until reconciled; retries do not create duplicate requests |
| Tampered or incomplete bundle | Altered bytes or missing mandatory dependencies produce failure/inconclusive results; no claim of completeness outside declared coverage |
| Find and re-export | Partner locates an operation by its business ID and re-exports its linked evidence after restart |
| Preservation and access | Required evidence remains retrievable after restart; unauthorized access is refused; agreed retention/export rules are demonstrated |
| Independent witness | Operator/control boundaries and attestation scope are documented and satisfy the partner's agreed independence criteria |
| Portable verification | Partner verifies the bundle from instructions, without internal database access or founders operating the run; core checks work without a live TrustAgentAI service |

Record integration time, founder assistance, added latency against the uninstrumented baseline, failure behavior, verification time, and investigation time against the existing process. Agree numeric thresholds before measurement.

## 7. Execution and commercial validation

Proposed owners: Iliia for engineering and release readiness; Kirill for partner selection and commercial validation. Confirm founder availability at cycle start. Select the partner and its technical owner during discovery, then agree the action, trust boundaries, and thresholds before integration begins.

| Timing from cycle start | Outcome |
| --- | --- |
| Days 1–7 | Confirm and address selected-path blockers; publish reproducible setup and negative test results |
| Days 1–10 | Conduct 10 substantive conversations; identify two teams willing to scope an integration and select one |
| Days 11–20 | Integrate the chosen action in the partner's sandbox and exercise failure cases |
| Days 21–30 | Partner-led verification, measurements, and continuation/payment discussion |

Interview around the last disputed operation, missing evidence, investigation cost/time, current tooling, and the person owning the problem and budget. Interest alone is not a pilot: require a named engineer, one workflow, and allocated time. Start planning conversations while engineering hardens the selected path; do not promise a release date before checks pass.

Pricing hypothesis: a subscription with included witnessed volume and separately priced additional volume/retention; SDK and basic verification remain easy to adopt. Validate service cost and willingness to pay before setting prices. An unpaid pilot requires a defined end date, feedback, and a continuation decision.

**Technical completion:** passing applicable checks, a sanitized bundle, partner-repeated verification, pinned versions and measured results. **Commercial evidence:** a named buyer, a specific reason to continue, and discussed payment terms. These are separate outcomes.

If no team commits a workflow and time by day 10, revisit the offer and customer segment. If technical completion does not lead to continued use, investigate before expanding functionality.

## 8. Positioning and references

Describe the differentiation to test as useful evidence across the customer's systems, independently witnessed under explicit assumptions, preserved and retrievable for later review. Independent verifiability, signatures, or WORM storage alone are not unique product claims.

- [AP2 specification](https://ap2-protocol.org/ap2/specification/) defines dispute evidence and verification, while retention and retrieval requirements are outside its scope.
- [Verifiable Intent security model, section 2.5](https://verifiableintent.dev/spec/security-model/) describes independently verifiable dispute evidence.
- [AWS CloudTrail integrity validation](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-log-file-custom-validation.html) provides a stronger comparison baseline than ordinary mutable logs.

References reviewed October 26, 2026. Compatibility must be tested against a pinned upstream version; these references do not establish a partnership or implementation.

Update site and investor materials to distinguish implemented, tested, pilot-validated, and planned capabilities. Preserve **Proof, not logs** as the objective, with the limits above.
