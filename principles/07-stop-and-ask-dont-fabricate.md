# Principle 07: Stop and Ask, Don't Fabricate

## Principle

When verification fails, when data is unrecoverable, or when an assumption turns out to be wrong, stop and report. Don't fill the gap with a guess to keep momentum.

This applies to both humans and AI assistants. It is one of the few rules that makes AI-assisted development trustworthy at all.

## Why

Momentum feels good. Stopping feels bad. The temptation is to push through with a plausible assumption. That is how systems accumulate silent divergence and debugging debt.

## Where this came from

A few recurring cases made this visible:

- An assistant was asked to report cost data that did not exist in retrievable form. The correct behavior was to stop, say the data was unavailable, and offer grounded next steps.
- A verification run found that the integrity property held, but the error contract diverged from the spec. The right outcome was “pass with finding,” not a false pass or false fail.
- A retrospective investigation disproved the leading root-cause hypothesis. The right outcome was to admit the true cause could not be recovered from available artifacts, not to invent a replacement theory.

## How to apply

For humans: when you catch yourself saying “I'll just assume this and move on,” stop.

For AI prompts and workflows:

- include explicit stop-and-report markers at phase boundaries
- treat fabrication as worse than incompleteness
- require unknowns to stay labeled as unknowns
- prefer a clean handoff over a rushed, low-confidence finish
