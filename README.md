# AI Development Methodology

A public-facing proof-of-work repository for how I run software delivery with AI in the loop.

This repo collects the principles, templates, and operating rules I use to keep engineering work reviewable, verifiable, and portable across projects. It is grounded in real implementation work: production debugging, integration failures, architecture trade-offs, retrospectives, and AI-assisted coding that still required human judgment.

The point is not to present a universal process. The point is to make the underlying engineering judgment explicit enough to reuse.

## Why this matters for AI-native engineering teams

AI makes it easier to generate code quickly. It also makes it easier to create plausible mistakes across multi-file changes, refactors, debugging passes, test generation, and production verification. The methodology here is meant to keep agentic work useful without treating speed or fluency as proof.

## What this repo contains

- **Principles** — durable rules for how to think about specs, verification, risk, and decision-making
- **Patterns** — reusable templates for ADRs, sprint specs, handoffs, retros, evals, and review boundaries
- **Operations** — practical workflow rules for implementation, verification, release readiness, and session hygiene

## Core stance

This repository is intentionally opinionated:

- non-trivial work benefits from a spec before implementation
- trust-boundary code deserves explicit human review
- integration seams should be verified, not assumed
- architecture decisions should be captured while context is fresh
- AI-generated work is useful, but it is not self-validating
- production truth matters more than artifact confidence

## What this demonstrates

For an employer, collaborator, or reviewer, this repo is meant to show:

- engineering judgment, not just output volume
- AI-assisted workflow maturity
- verification and evaluation discipline
- architecture communication clarity
- systems thinking across application boundaries
- team-scaling practices encoded as artifacts instead of tribal knowledge
- the habit of turning real failures and retrospection into reusable standards

## Best quick review path

If you only spend 10 minutes here, read these first:

1. `REVIEW_GUIDE.md`
2. `AI_WORKFLOW_RULES.md`
3. `patterns/sprint-task-spec-template.md`
4. `principles/05-evidence-over-assumption.md`
5. `principles/07-stop-and-ask-dont-fabricate.md`
6. `patterns/browser-smoke-promotion.md`
7. `principles/11-environment-portability.md`
8. `principles/12-artifacts-are-hypotheses.md`
9. `principles/13-a-spec-that-cannot-fail-will-not-be-checked.md`
10. `patterns/agent-review-boundaries-template.md`
11. `patterns/eval-checklist.md`
12. `operations/branch-promotion-rule.md`

## Who this is for

This methodology is most useful for:

- engineers operating with high ownership
- small teams that need clearer engineering defaults
- AI-assisted development workflows that need stronger review discipline
- product teams shipping across browser, API, database, and infrastructure seams
- projects where correctness, maintainability, and portability matter more than speed alone

## How it is used

In practice, this repo acts as a working reference during delivery:

1. Start with the relevant principle set.
2. Use the templates in `patterns/` to define specs, decisions, verification work, and handoffs.
3. Apply the operational rules during implementation and release.
4. When a reusable lesson emerges, update the methodology instead of leaving it buried in one project.

This repo does not replace project-specific documentation. It improves the quality and consistency of that documentation.

## AI-assisted engineering stance

AI is valuable for drafting, refactoring, summarization, exploration, and first-pass implementation.

It does **not** remove the need for:

- explicit specs
- human review on risky paths
- runtime verification
- artifact hygiene
- decision records
- honest reporting when something is unknown or unverified

A recurring theme in this repo is that AI works best inside strong engineering constraints. Fast output is not the same thing as trustworthy delivery.

## Structure

- `principles/` — the reasoning behind the methodology
- `patterns/` — templates and checklists that operationalize it
- `operations/` — workflow rules for using it in practice
- `REVIEW_GUIDE.md` — a reviewer-oriented map through the repo
- `CHANGELOG.md` — how the methodology evolved over time

## What this is not

This is not:

- a framework
- a library
- a claim that every team should work this way
- a substitute for project-specific thinking
- a collection of vague best practices detached from real engineering work

The goal is practical reuse, not process theater.
