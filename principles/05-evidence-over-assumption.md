# Principle 05: Evidence Over Assumption

## Principle

When verification is possible, do it. Don't accept “the code looks right” or “the unit tests pass” as proof that a system works end-to-end. Manually exercise the trust-boundary paths. Inspect database state. Read the actual emails. Click the actual buttons.

## Why

Unit tests verify that pieces work in isolation. They do not prove that the pieces compose correctly, that environment variables are wired consistently, that migrations applied, or that external services are reachable.

Integration seams are where bugs hide.

## Where this came from

A verification pass on a production-bound sprint caught multiple integration-seam bugs that unit tests had missed. The tests genuinely passed. The system genuinely failed end-to-end. Both facts were true at the same time.

The manual pass took time. That was the point. It forced attention onto what the system actually did instead of what the implementation seemed to imply.

## Mini-example

In one case, an AI-generated implementation for a form workflow looked correct on first pass: the handler saved the record, returned a success state, and the unit tests passed. But the real flow still failed because the next system expected a normalized field name and a webhook payload shape the implementation had not matched. The bug was not in the local function. It was at the integration seam.

The methodology caught it because the work was verified against the real runtime path rather than accepted at code-review depth.

## How to apply

For any sprint that touches trust-boundary code, manual smoke testing is a deliverable, not an afterthought. Budget time for it in the sprint plan. Write the smoke plan before implementation begins. Run it after implementation completes.

## Corollary: the issue tracker is an assumption, not evidence

Principle 05 is usually applied forward — don't trust that code works because
the tests pass. It applies just as hard in reverse: **don't trust that work is
open because the issue is open.**

An issue tracker records intent at the moment it was written. It does not track
the code. Work ships, the branch merges, and the issue stays open because
nobody closed it — especially when a PR merges to a non-default branch (see
`operations/branch-promotion-rule.md`), so no `Closes #N` ever fires.

The failure mode: you pick up an open, priority-tagged issue, read its "current
state" section, and plan a build against a snapshot that is weeks or months
stale. The work may already be done, partly done, or done differently than the
issue describes.

### Where this came from

A backlog-readiness sweep found that a majority of the still-open,
priority-tagged issues had already shipped; the tracker had simply not caught
up. Each looked like real work until the code was read on the deployed branch.

### How to apply

Before treating any tracked issue as work:

1. Verify current state against the **deployed** ref — not the issue body, and
   not the default branch if it isn't the deployed line.
2. Classify DONE / PARTIAL / OPEN with `file:line` evidence.
3. Only then scope. If it's DONE, close it (citing the evidence) rather than
   rebuilding it.

For a whole backlog, batch this — see `patterns/stale-board-sweep.md`.

## Corollary: verify in the system of record, not the caller's response

A successful-looking response on the caller's side is not proof that the
target did the work. Confirm the effect where the target records it: for a
workflow engine, a **new run under Executions**; for a database, the row
exists; for a queue, the message was consumed.

### Where this came from

A webhook test was reported as passing because the HTTP call returned
something plausible. The workflow it was meant to trigger had never been
stored in the engine's database; it was absent from every backup across the
period. The "pass" was a false positive, and it held until someone looked
for the execution record that should have existed.

### How to apply

1. Before calling a test passed, find the new execution, row, or log entry
   it should have created.
2. Check that the thing under test exists in the target's store (e.g. the
   workflow is stored and published, not just present in a file).
3. Cross-check backups: if the object never appears in any backup, it was
   never deployed.
4. When the caller hides the target's real error, probe from the caller's
   side of the network (`operations/probe-from-calling-container.md`).

### Files referenced (corollary)

| File | What it is / does | When to use or change it | Key contents and gotchas |
|---|---|---|---|
| n8n Executions list (n8n UI, not a file) | n8n's record of each workflow run. | The proof step after any test request. | Look for a **new** run of the workflow under test. |
| Backups (database dumps or equivalent) | Pre-deploy snapshots. | Take one before every deploy; restore one on rollback. | Also evidence: an object missing from every backup was never deployed. |
| `operations/probe-from-calling-container.md` | How to debug a failing call between two containers. | When service A's call to service B fails or A shows only a generic error. | Exec into A, call B exactly as A does, print status and body, reference secrets as env vars. |
