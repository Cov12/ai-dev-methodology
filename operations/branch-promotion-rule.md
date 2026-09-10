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

## Corollary: the default branch must BE the deployed line

The rule above assumes the default branch is where deployed-and-validated code
lands. If production actually runs off a different branch, that assumption
inverts and three things break at once:

1. **State checks lie.** Grepping the default branch to answer "is this already
   fixed?" returns false negatives — the truth lives on the deployed branch.
2. **Auto-close stops firing.** Most trackers close linked issues ("Closes #N")
   only when the PR merges to the *default* branch. PRs merged to a non-default
   deployed branch leave their issues open, and the tracker drifts (see
   `principles/05` — that drift then reads as open work).
3. **New branches inherit stale bases.** Feature work cut from the nominal
   default lacks shipped fixes and can reintroduce them on the next promotion.

### Where this came from

A repository's default branch drifted dozens of commits behind its actual
deployed branch. A shipped fix on the deployed branch never reached the
default; its issue never auto-closed; and fresh feature branches cut from the
default lacked the fix. Every "is this fixed?" check was unreliable until the
divergence was named and reconciled.

### How to apply

- Make the default branch the promoted/deployed line, or keep them in lockstep
  through the promotion flow. Reconcile long-lived divergence deliberately —
  audit the gap for fixes that must not be lost.
- If a repo must deploy off a non-default branch, treat the default branch as
  untrusted for state checks: verify against the deployed ref, and close issues
  manually since auto-close won't fire.
