# Methodology Changelog

## v0.1.3 — 2026-09-10 — Stale-board sweep + tracker/branch/schema corollaries

Extracted from a backlog-readiness sweep where most still-open, priority-tagged issues turned out already shipped.

- `patterns/stale-board-sweep.md` (new): parallel read-only recon agents returning DONE/PARTIAL/OPEN with `file:line` evidence, reconciled against producer/consumer apps before any build.
- `principles/05` corollary: the issue tracker is an assumption, not evidence — an open issue is not proof of open work; verify the deployed code.
- `operations/branch-promotion-rule.md` corollary: the default branch must BE the deployed line, or state-checks lie, auto-close stops firing, and new branches inherit stale bases.
- `principles/11` corollary: the schema-apply mechanism is part of the environment contract — push-apply refuses data loss, so keep changes additive and plan destructive ones deliberately.

---

## v0.1.2 — 2026-09-10 — Browser smoke promotion pattern

Added `patterns/browser-smoke-promotion.md`, a public-safe pattern for turning UI verification gaps into reusable browser smoke infrastructure.

This captures a recurring AI-assisted engineering lesson: unit tests, adapter tests, and direct API checks are valuable, but they do not prove browser-visible behavior. The new pattern covers stable package-script entrypoints, deterministic mocks/seeds, `data-testid` selectors, negative controls, reload/rehydration assertions, service-worker/CORS/onboarding-modal hardening, and failure artifacts.

---

## v0.1.1 — 2026-07-05 — Environment portability principle

Added Principle 11: Environment portability.

Source: a cross-machine tooling failure during an engineering workflow. The first machine had the working toolchain and authenticated state; the second machine had the code but not the environment required to run it. The pipeline itself was fine. The failure was environmental, and it surfaced only when a real deliverable had to run.

The lesson generalizes: identify machine-bound state that version control does not carry — credentials, installed runtimes, environment variables, licensed tools, local CLIs — and commit an explicit reproduction path.

Also added: `patterns/spike-checklist.md` now emphasizes environment state as part of a complete spike outcome.

---

## v0.1 — initial extraction from production delivery work

First version of the methodology, extracted from repeated implementation, verification, and retrospective cycles across real projects.

Principles captured:
- 01: Spec before code
- 02: Trust boundaries
- 03: Controlled flexibility
- 04: Fail closed
- 05: Evidence over assumption
- 06: Observability before optimization
- 07: Stop and ask, don't fabricate
- 08: ADR for decisions
- 09: Retro after every sprint
- 10: Commit per ticket

Patterns captured:
- ADR template
- Sprint task spec template
- Smoke test plan template
- Retro template
- Handoff note template
- Agent review boundaries template
- Eval checklist

Operations captured:
- Session management
- Commit hygiene
- Verification pass prompt
- AI workflow
