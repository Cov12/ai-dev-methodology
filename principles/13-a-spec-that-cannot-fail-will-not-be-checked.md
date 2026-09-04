# Principle 13 — A spec that cannot fail will not be checked

## The principle

Every claim about a system needs a form that can be *violated*. "The sanitizer
strips onerror" can fail. "The refresh fires before expiry" can fail. "Two-pane
layout with a live preview" cannot fail — it's a description, not an assertion.
Descriptions get built approximately, or not at all, and nothing in the workflow
objects.

This is Principle 12's sibling. P12 says *verify the running system, not the
artifact describing it*. P13 says *if the artifact never made a checkable claim,
there's nothing to verify against*. P12 catches a wrong answer; P13 is about
questions that were never askable.

## Where it bites

Prose specs are most tempting exactly where they're least sufficient: user-facing
surfaces. Layout feels self-evident once you've seen a screenshot, so it gets
described instead of asserted. Meanwhile the same session writes rigorous tests
for a sanitizer allowlist — because that spec *had* a pass/fail and the UI spec
didn't.

The tell: after a session, the things that came out well all had verifiable
criteria tested against real data. The things that came out badly had none. That's
not variable competence. That's variable specification.

## The rule

**Acceptance criteria are observations, not descriptions.**

Not: "Left pane chat, right pane live preview with skeleton loading."

But: "Open /create. Send a message. → The right pane shows a skeleton loader
while the agent works. → When generation completes, POSITION SUMMARY renders in
the right pane; the chat remains on the left. → Click Accept. → The browser
lands on /jdex/create with all four sections prefilled."

Each arrow is a gate: what you do, what you should see, and — implicitly — what
it means if you don't. This is the deploy-runbook pattern (command → healthy
signal → STOP condition) applied to a browser instead of a shell. The pattern
already exists in this methodology; it just hadn't been aimed at UI.

**Screenshots are input, not specification.** Convert them to observable
assertions *before* building. A screenshot shows what someone imagined; a gate
says what must be true.

## Definition of done for any user-facing surface

A completed user journey, **observed in the running system**, matching the gates.

- Code existing is not done.
- "I built the component" is not done.
- The agent must open it, drive the full flow as the user would, and report what
  it *observed* — not what it wrote.

If the flow can't be completed, it isn't done, regardless of what shipped.

---

# Practice — Prototype UI before implementing it

Build the interface as a standalone prototype first, agree on it, *then* hand it
to the implementing session as the reference.

**Why:** it collapses the interpretation gap. A prose description of a layout gets
re-interpreted by whoever implements it, under whatever context pressure they're
under. A rendered prototype is unambiguous — you look at it, you fix it in
seconds, and the result is a concrete artifact the implementing session matches
rather than infers.

This is the same logic as ADRs: pin the decision as an artifact instead of leaving
it as intent.

**How:**
1. Describe the interface; get a rendered prototype (chat-based iteration is fast
   here — seconds per revision).
2. Iterate until it's right. Cheap, because nothing is wired.
3. Commit the prototype as the spec.
4. The implementing session builds *to the prototype*, and its definition of done
   is the observable-gates journey above.

**Scope note:** a UI surface deserves its own session. If a sprint contains a new
agent, new routes, new auth, an integration, *and* a UI, the UI will lose — it's
the piece with the weakest failure signal, so it absorbs the context pressure. Split it.