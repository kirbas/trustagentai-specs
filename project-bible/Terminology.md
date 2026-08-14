# TrustAgentAI Project Bible — Terminology

## Purpose

This document defines the canonical architectural terminology used throughout the **TrustAgentAI Project Bible**.

Consistent terminology is essential because TrustAgentAI separates concepts that conventional systems often collapse together, including:

- identity and cryptographic keys;
- signatures and authorization;
- submission and commitment;
- object integrity and historical integrity;
- replication and independence;
- storage and preservation;
- current state and historical state;
- evidence validity and evidence completeness;
- intended assurance and achieved assurance.

TAIP may provide more precise normative definitions for individual protocol objects and fields.

Where terminology differs between informal implementation language and the Project Bible, the Project Bible terminology should be preferred for architectural discussion.

---

# Accountable Action

An **Accountable Action** is an action, decision, authorization, state transition, or other event whose occurrence or consequences require durable accountability evidence.

Examples may include:

- approving a payment;
- initiating a transfer;
- changing a financial policy;
- granting authority to an Agent;
- revoking authority;
- overriding a control;
- changing a Trust Profile;
- rotating an accountability-critical key.

Not every software event is an Accountable Action.

The applicable business, policy, regulatory, or Trust Profile context determines which actions require accountability evidence.

---

# Accountability Claim

An **Accountability Claim** is a proposition about an Accountable Action or protocol state that a verifier attempts to evaluate using available evidence.

Examples include:

```text
"Agent A authorized Payment P."
