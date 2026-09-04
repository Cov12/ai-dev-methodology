# Eval Checklist

A lightweight checklist for evaluating AI-assisted engineering work.

## Before running the eval

- [ ] What exact claim is being tested?
- [ ] What would count as failure?
- [ ] What runtime surface or artifact will be observed?
- [ ] What assumptions are still unverified?

## During the eval

- [ ] Exercise the happy path
- [ ] Exercise at least one meaningful failure path
- [ ] Check the integration seam, not just the local unit
- [ ] Observe the real side effect where applicable
- [ ] Record any contract drift between spec and implementation

## After the eval

- [ ] Classify the result: pass, fail, pass-with-finding, or blocked
- [ ] Record what was actually observed
- [ ] Record what remains unknown
- [ ] Feed reusable lessons back into specs, retros, or ADRs

## Why this exists

The point of an eval is not just to confirm success. It is to make failure and uncertainty visible before they turn into production surprises.
