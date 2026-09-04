# Principle 03: Controlled Flexibility, Not Unlimited

## Principle

Systems are structured rather than open-ended. Customization happens within bounded surfaces, not as raw “build anything” affordances. The constraint is what enables consistency, maintainability, and safe evolution.

## Why

Open-ended systems are seductive at design time and painful at scale. The more unconstrained the surface, the harder it becomes to ship coherent upgrades, reason about behavior, or avoid undocumented dependencies.

Structured systems give up some flexibility in exchange for:

- consistent behavior across instances
- predictable upgrade paths
- reduced support burden
- the ability to reason about the system as a whole
- better protection against accidental edge-case sprawl

The goal is to be flexible enough, not maximally flexible.

## Where this came from

In one storefront platform design, the tempting option was a fully open-ended page builder. The better choice was a set of structured sections — hero, featured items, testimonials, FAQs, and reusable content blocks — that could be composed without making every page a custom system.

A similar pattern showed up in assistant safety architecture. The simplistic version was “one big system prompt that tells the model not to do bad things.” The more durable version was a layered design with explicit interfaces between layers.

## How to apply

When designing a system, ask: what is the smallest set of primitives that lets users or future code accomplish what they actually need? Build those primitives first.

When extending a system, ask whether the new need requires a new primitive or can be expressed as a composition of existing ones.
