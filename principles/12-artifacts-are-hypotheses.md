# Principle 12: Artifacts Are Hypotheses

## Principle

Artifacts that describe a system — documentation, comments, configuration files, test fixtures, constructed examples — are hypotheses about the system, not the system itself. Confirm against the running thing before building on them.

## The pattern

This failure mode shows up repeatedly:

1. A source file implies a token refresh path is covered, but no one has actually observed a refresh event.
2. A second machine is assumed to match the first, but its runtime state is missing.
3. A synthetic test case behaves cleanly, but production-shaped data does not.
4. A comment or disabled code path suggests one editor or UI library is active; browser inspection shows another.

In each case, a plausible reading of artifacts stood in for observed reality.

## Why this happens

Artifacts are written at a point in time. The system evolves. The gap grows.

- comments describe intent at write-time, not current behavior
- docs describe the design, not the implementation
- configs describe expectation, not runtime state
- test fixtures describe a scenario, not production data

## How to apply

Before building on a system description:

1. Identify what you're assuming.
2. Find the running thing.
3. Confirm the assumption.
4. Note divergence if the artifact is stale or wrong.
