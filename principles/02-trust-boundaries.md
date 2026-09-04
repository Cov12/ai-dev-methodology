# Principle 02: Trust Boundaries Get Human Review

## Principle

Code that handles authentication, payments, attestation, compliance, audit logging, AI safety guardrails, or any irreversible action must be reviewed by a human, not just by an AI assistant. Other code can be largely AI-drafted. The boundary is explicit and documented per project.

## Why

AI systems are excellent at producing code that looks right and often passes local tests. They are less reliable at catching subtle failures that only emerge under adversarial or production-like conditions.

The categories above share a property: the cost of a subtle bug is extreme and often non-reversible. A bug in a marketing component is a fixable embarrassment. A bug in a payment webhook verifier is a financial vulnerability. A bug in audit logging can erase evidence you needed to keep.

Human review is slower than AI-only development. The slowness is the point.

## Where this came from

A verification cycle on a production-bound system surfaced multiple trust-boundary bugs that unit tests missed:

1. Email dispatch read the wrong environment variable.
2. A tenant identifier was hardcoded to a nonexistent scope.
3. A webhook event table existed, but the handler never wrote to it.
4. A denormalized payment status drifted from the real state machine.
5. Signature comparison crashed on malformed input instead of failing closed.

All five passed unit tests. All five required manual integration verification to surface. The project had an explicit review-boundaries document, which is what made those areas visible enough to audit deliberately.

## How to apply

Every project should have an `agent-review-boundaries.md` file or equivalent that lists which file paths or code categories are on which side of the boundary.

Categories that are almost always on the heavy-review side:

- Authentication and session management
- Payment processing and webhooks
- Attestation, consent, and legal evidence
- Audit logging
- AI guardrails and safety layers
- Compliance enforcement
- Cryptographic operations
- Inventory or stock management for physical goods
- Any code that creates side effects in external systems

Categories that are often safe to draft first with AI:

- UI components and styling
- Static page content
- Test fixtures and helpers
- Build configuration
- Documentation

## Pattern references

- `patterns/agent-review-boundaries-template.md`
