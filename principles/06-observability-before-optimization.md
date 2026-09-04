# Principle 06: Observability Before Optimization

## Principle

Instrument before you optimize. Before you can reason about whether a system is working, you need to be able to see what it's doing.

## Why

You will be tempted to skip instrumentation because it feels like overhead. That is usually a mistake. By the time you need it, you've already burned hours debugging blind and made decisions using gut feel that should have been driven by data.

## Where this came from

A production-bound build exposed this principle by violating it. The system needed budget and performance decisions, but the data required to make those decisions had never been captured. The lesson was simple: if a signal will matter later, install the instrumentation before the sprint that depends on it.

## How to apply

Early in a project, install:

- error monitoring
- product or usage analytics where appropriate
- structured application logging
- cost or token tracking for AI-assisted workflows
- query timing for persistence layers
- worker and queue health metrics for background jobs
