# AI Workflow Rules

These are the working rules behind the AI-assisted engineering posture reflected in this repository.

## Core rules

1. **Specs before non-trivial implementation.**
2. **AI drafts are not proof.** Generated output still needs review and verification.
3. **Trust-boundary code gets explicit human review.**
4. **Integration seams get runtime checks, not just unit tests.**
5. **Unknowns stay labeled as unknowns.** If something cannot be verified, stop and report.
6. **Artifacts are allowed to be wrong.** Comments, docs, and configs are hypotheses until checked against the running system.
7. **Acceptance criteria must be falsifiable.** A claim that cannot fail will not be meaningfully verified.
8. **Decisions worth revisiting are worth recording.** Use ADRs for non-trivial technical choices.
9. **Retros are operating artifacts.** Capture what failed, drifted, or surprised the team.
10. **Promotion requires validation, not just code merge.**

## Where AI helps most

- drafting first-pass implementations
- summarizing logs or docs
- turning rough ideas into structured specs
- generating test scaffolds
- accelerating refactors with clear guardrails
- exploring alternative implementations quickly

## Where AI needs tighter control

- authentication and authorization
- payments, billing, or irreversible side effects
- cryptographic operations
- compliance and policy enforcement
- audit logging
- production migrations
- any change where a subtle bug has a large blast radius

## Minimum bar before calling work done

- the claim is specific enough to fail
- the implementation matches the stated scope
- the relevant runtime path was exercised
- important side effects were observed directly
- remaining unknowns are named explicitly

## Short version

Use AI to accelerate the work. Do not use it to skip the work.
