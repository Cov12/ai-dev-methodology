# Pattern: Stale-Board Sweep

## When to use

A backlog of tracked issues has accumulated faster than it's been closed, and
you suspect some are already done. Before planning a work cycle against them,
establish which are actually open. Supports `principles/05` (the tracker is not
evidence) and `operations/branch-promotion-rule.md` (verify against the
deployed line).

## The pattern

Fan out **read-only** recon — one agent per issue (or per repo/cluster) — each
answering the same contract, in parallel:

- Read the issue's full text and its claimed "current state."
- Fetch and check the **deployed** ref (not the issue body, not a stale local
  checkout, not the default branch if it isn't the deployed line).
- Verify each concrete claim against the code.
- Return a verdict — **DONE / PARTIAL / OPEN** — with `file:line` evidence per
  sub-claim and a short list of what (if anything) is genuinely left.

Then reconcile and act:

- **DONE** → close with the evidence (see governance below).
- **PARTIAL** → narrow/retitle the issue to the real residual scope.
- **OPEN** → now safe to scope as work.

## Guardrails

- **Recon is read-only.** The sweep produces verdicts, not commits. Building
  happens after, deliberately, per verdict.
- **Reconcile cross-app before dispatching.** A single-repo agent cannot see a
  producer/consumer app that enforces a concern upstream. "Unguarded here" may
  be "enforced at the producer." Always check the other side before filing or
  building — this is the repo-scoped over-scoping trap.
- **Governance: close on the owner's say-so.** The sweep flags stale-resolved
  issues; the human decides what closes. PRs that resolve an issue carry
  `Closes #N`; issues resolved outside a PR (or found already-shipped) are
  closed manually with an evidence comment.
- **Verify freshness first.** Fetch before reading; a stale local tree produces
  false "already broken" or "not yet built" claims.

## Where this came from

A backlog-readiness sweep across several repositories used parallel read-only
recon agents, each returning DONE/PARTIAL/OPEN with evidence. Reconciliation
found most "open blockers" had already shipped, one was partial (narrowed), and
one was enforced upstream in a producer app — which a single-repo view would
have mis-built as a local fix. The sweep also surfaced a branch-divergence root
cause behind the drift.

## Related

- `principles/05-evidence-over-assumption.md`
- `operations/branch-promotion-rule.md`
- `operations/agent-prompts/verification-pass.md`
- `patterns/agent-review-boundaries-template.md`
