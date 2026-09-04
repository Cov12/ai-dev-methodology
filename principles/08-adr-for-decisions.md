# Principle 08: ADR for Non-Trivial Decisions

## Principle

When you make a decision that affects how the system is built — choice of library, architecture pattern, deployment model, or trade-off between reasonable alternatives — write an ADR. The code shows what was built; the ADR shows why.

## Why

The reasoning behind a decision is invisible from looking at the code. Months later, a contributor will ask why a framework, queue model, or storage approach was chosen. Without an ADR, the answer gets re-derived badly or lost entirely.

ADRs are also a forcing function. Writing context, alternatives, and consequences makes you think more clearly before the decision calcifies.

## Where this came from

Across production delivery work, short ADRs consistently paid for themselves. They captured decisions ranging from ORM choice to execution-model boundaries for side effects. The cost to write them was small; the value came later when verification, maintenance, or onboarding required the original reasoning.

## How to apply

Every project should have a `docs/adr/` directory or equivalent. Write an ADR when you decide:

- which library or framework to use when alternatives existed
- an architectural pattern
- a deployment model
- a meaningful trade-off between two reasonable approaches
- a workaround you will likely revisit
