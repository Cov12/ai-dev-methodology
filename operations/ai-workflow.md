# AI Workflow

This document describes how AI fits into the engineering workflow represented by this repository.

## Good uses of AI

- drafting first-pass implementations
- summarizing logs, docs, and diffs
- generating test scaffolds
- turning rough ideas into structured specs
- exploring multiple implementation approaches quickly

## Bad uses of AI

- treating generated code as self-validating
- letting an assistant invent missing facts
- skipping runtime verification because the draft looks plausible
- using AI output as a substitute for architecture decisions
- treating a polished explanation as proof that the system works

## Workflow posture

1. Write or refine the spec.
2. Mark trust boundaries explicitly.
3. Use AI for scoped implementation help.
4. Verify the running system, not just the diff.
5. Capture decisions and lessons in durable artifacts.
6. Start a fresh session when context has decayed.

## Review standard

AI output is held to the same standard as code from any other contributor:

- the scope should be explicit
- the risky paths should be visible
- claims should be checkable
- unknowns should stay labeled
- runtime behavior matters more than persuasive text
