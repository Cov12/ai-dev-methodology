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

## Corollary: required secrets

Fail closed applies at startup too. An entrypoint exits with a `FATAL`
message when a required secret is empty, rather than starting in a degraded
state that looks healthy until the first real request.

In compose files, reference secrets as `${VAR:-}` so a service that doesn't
use a given secret can still start without it. Each service then fails
closed only on the secrets it actually needs, instead of every service
demanding every secret. See `operations/shared-server-deploy.md`.
