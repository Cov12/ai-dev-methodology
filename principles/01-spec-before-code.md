# Principle 01: Spec Before Code

## Principle

For any non-trivial work, write the spec before writing the code. PRDs before sprints. Sprint task specs before tickets. Technical specs before complex subsystems. The act of writing forces clarity that's hard to recover later.

## Why

Writing reveals what you don't know. When you try to specify a system in words, the gaps in your thinking become visible. Catching those gaps in a doc costs minutes. Catching them in code costs hours. Catching them in production costs trust.

The spec also becomes the contract. When an agent, collaborator, or future-you implements from the spec, the spec is the authority. Code that diverges from the spec without an ADR is a bug to investigate, not a fait accompli to accept.

## Where this came from

Repeated production work made this visible:

- A product spec surfaced a strategic posture question before implementation began. Resolving it in the document prevented a costly downstream rebuild.
- A payment architecture spec defined the processor interface before any concrete adapter existed. That made a later provider pivot straightforward instead of invasive.
- A safety-oriented assistant spec defined its defense-in-depth model before any generation code was written. Without that, the implementation would have drifted into ad-hoc guardrails.

Counter-example: one delivery cycle had no spec for how background jobs should handle persistent failures versus transient ones. The gap only became obvious during verification, when the execution model had to be formalized retroactively. Cheaper to write that down before implementation than after.

## How to apply

For any work larger than roughly a half day of implementation, write a spec first. The spec answers, at minimum:

- What is being built and why
- What problem it solves
- What's in scope and out of scope
- What the interfaces look like
- What the failure modes are
- What success looks like

For smaller work, use judgment. The point is not ceremony. The point is avoiding avoidable ambiguity.

For AI-assisted work specifically: the spec is what you hand to the assistant. Without a spec, the assistant fills the gap with its own assumptions.

## Pattern references

- `patterns/sprint-task-spec-template.md`
