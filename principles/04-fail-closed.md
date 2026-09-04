# Principle 04: Fail Closed

## Principle

When in doubt, refuse. The default behavior for uncertainty, ambiguity, or system failure should be “this might be a problem, don't do it” rather than “this is probably fine, do it.”

## Why

A refusal that should have been an answer is mildly frustrating. An answer that should have been a refusal can be catastrophic. The asymmetry favors caution.

In safety-sensitive assistant workflows, a refusal has UX cost. A bad answer can have policy, compliance, or real-world harm cost.

In reliability-sensitive system workflows, a webhook with an invalid signature should be rejected. A system that accepts it and sorts out the consequences later has already lost the boundary.

## Where this came from

A layered assistant safety architecture and a signature-validated webhook path both made this principle concrete. In each case, side effects only happened after the validation chain passed. When any check was uncertain, malformed, or incomplete, the safe response was refusal.

## How to apply

Use fail-closed defaults for:

- validation logic
- authentication boundaries
- webhook handlers
- AI safety guardrails
- background job error handling
- geographic and demographic restrictions
- anything involving consent or attestation
