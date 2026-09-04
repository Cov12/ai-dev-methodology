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
