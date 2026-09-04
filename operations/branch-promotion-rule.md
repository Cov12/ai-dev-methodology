# Branch Promotion Rule

**Status:** Active
**Date:** 2026-07-22

## Rule

**main = deployed + verified + validated.**

Feature or sprint branches promote to main once proven, not just once implemented.

## Definitions

- **Deployed:** Code is running in the target environment.
- **Verified:** Technical validation is complete; health checks pass and obvious runtime errors are absent.
- **Validated:** The intended user or stakeholder confirms the feature solves the real problem it was built for.

## Rationale

A branch is still work-in-progress until it meets all three criteria. Closing a sprint, writing a retro, or passing tests is not sufficient for promotion by itself.

Example: a feature can be deployed and technically healthy while still waiting on user confirmation that the workflow is actually correct. In that state, it belongs on the feature branch, not on main.

## Workflow

1. Work happens on a feature or sprint branch.
2. Deploy from that branch.
3. Verify technical behavior.
4. Collect stakeholder or user validation.
5. Promote to main only after all three criteria are satisfied.

## Anti-pattern

Do **not** merge to main just because:

- the sprint is “done”
- the retro was written
- the code compiles and tests pass
- the deploy succeeded

User validation is a release criterion, not a nice-to-have.
