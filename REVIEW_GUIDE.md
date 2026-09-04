# Review Guide

This file is a fast orientation guide for reviewers evaluating this repository as a proof-of-work artifact.

## What this repo is

This is a methodology repository for AI-assisted software engineering.

It contains principles, templates, and workflow rules derived from real implementation and verification work across multiple projects. The emphasis is not abstract best-practice language; it is repeatable habits shaped by things that actually fail in software delivery.

## What this repo is meant to demonstrate

This repository is intended to show:

- **Engineering judgment** — especially around verification, failure modes, and production behavior
- **AI workflow maturity** — using AI as a contributor inside constraints, not as an unquestioned source of truth
- **Systems thinking** — documenting interfaces, dependencies, review boundaries, and release criteria
- **Documentation rigor** — treating specs, ADRs, retros, and handoffs as operating artifacts
- **Team enablement** — creating artifacts that scale beyond one engineer's memory or one long chat thread
- **Operational realism** — grounding process in integration seams, missing environment state, stale docs, and validation gaps

## Suggested reading order

1. `README.md`
2. `AI_WORKFLOW_RULES.md`
3. `patterns/sprint-task-spec-template.md`
4. `principles/05-evidence-over-assumption.md`
5. `principles/07-stop-and-ask-dont-fabricate.md`
6. `principles/11-environment-portability.md`
7. `principles/12-artifacts-are-hypotheses.md`
8. `principles/13-a-spec-that-cannot-fail-will-not-be-checked.md`
9. `patterns/agent-review-boundaries-template.md`
10. `patterns/eval-checklist.md`
11. `operations/branch-promotion-rule.md`

## Why these files matter

### AI workflow rules
Shows there is an explicit rule set for how AI is used, constrained, and verified.

### Sprint task spec template
Shows a real agent workflow artifact: scoped work, stop conditions, review boundaries, and verification requirements written down before execution.

### Evidence over assumption
Shows a bias toward runtime truth over code that merely looks correct.

### Stop and ask, don't fabricate
Shows the discipline required to make AI-assisted development trustworthy.

### Environment portability
Shows awareness that working code is not the same as a reproducible engineering setup.

### Artifacts are hypotheses
Shows skepticism toward stale docs, comments, configs, and synthetic fixtures when the running system can be observed directly.

### A spec that cannot fail will not be checked
Shows a concrete philosophy for turning vague requirements into falsifiable acceptance criteria.

### Agent review boundaries
Shows a practical way to separate AI-draftable work from high-risk code that needs explicit human review.

### Branch promotion rule
Shows a delivery model where technical readiness and user validation both matter before work is treated as complete.

## How AI fits into this workflow

AI is treated as a force multiplier for:

- drafting
- synthesis
- iteration
- scaffolding
- refactoring
- exploratory implementation

AI is **not** treated as sufficient proof.

The methodology assumes that AI-generated work still requires:

- specs for non-trivial changes
- explicit review boundaries
- runtime verification
- honest uncertainty reporting
- artifact cleanup and documentation
- human judgment on risky or irreversible behavior

## What the examples are

The examples in this repo are anonymized, but they are derived from real engineering patterns such as:

- verification passes catching integration bugs that tests missed
- cross-machine failures caused by undocumented environment state
- architecture decisions that needed durable reasoning records
- late-stage debugging caused by stale docs or misleading artifacts
- AI-assisted workflows needing clearer rules about when to stop, verify, escalate, or document

They are meant to show how the methodology was shaped, not to claim a universal process.
