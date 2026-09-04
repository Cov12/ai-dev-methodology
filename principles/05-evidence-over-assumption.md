# Principle 05: Evidence Over Assumption

## Principle

When verification is possible, do it. Don't accept “the code looks right” or “the unit tests pass” as proof that a system works end-to-end. Manually exercise the trust-boundary paths. Inspect database state. Read the actual emails. Click the actual buttons.

## Why

Unit tests verify that pieces work in isolation. They do not prove that the pieces compose correctly, that environment variables are wired consistently, that migrations applied, or that external services are reachable.

Integration seams are where bugs hide.

## Where this came from

A verification pass on a production-bound sprint caught multiple integration-seam bugs that unit tests had missed. The tests genuinely passed. The system genuinely failed end-to-end. Both facts were true at the same time.

The manual pass took time. That was the point. It forced attention onto what the system actually did instead of what the implementation seemed to imply.

## How to apply

For any sprint that touches trust-boundary code, manual smoke testing is a deliverable, not an afterthought. Budget time for it in the sprint plan. Write the smoke plan before implementation begins. Run it after implementation completes.
