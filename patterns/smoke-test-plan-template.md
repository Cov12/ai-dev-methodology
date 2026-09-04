# [Project] — Sprint N Smoke Test Plan

**Sprint scope tested:** TICKET-NNN through TICKET-NNN
**Estimated time:** [N hours]
**Approach:** Work through each Path in order. If a Path fails, fix
before continuing.

---

## 0. Pre-Flight Verification

_Verify environment, services, schema, baseline data. The system must be
in a known good state before testing begins._

### 0.1 Services and environment
_Commands to confirm services running, env vars set, dev server starts._

### 0.2 Database schema state
_Verify all expected tables exist; new sprint tables present._

### 0.3 Seed data baseline
_Note starting values for anything inventory-like that will change
during tests._

---

## Path 1: Happy Path End-to-End

_The most important test. Click through the full user journey._

### 1.1 [Step name]
_Numbered steps, each a discrete action._

### 1.N Verify [thing]
_Database query or visual confirmation that the step's side effects
landed correctly._

---

## Path 2: [Specific trust-boundary test]

_One Path per major risk area. Examples:_
- _State machine validity_
- _Race conditions_
- _Authentication boundaries_
- _Permission enforcement_
- _AI guardrail layers_

---

## Path N: Adversarial Scenarios

_Things bad actors might try. Should not succeed._

- _Replay attacks_
- _Signature tampering_
- _Forged input_
- _Permission bypass_
- _Rate limit bypass_

---

## Path N+1: Database Integrity Spot-Checks

_SQL queries that confirm system invariants hold._

- _No orphan records_
- _Foreign key integrity_
- _Constraint enforcement_
- _Idempotency dedup actually working_

---

## Path N+2: Cleanup After Testing

_Remove test data, restore baseline state, document residual changes._

---

## Summary Checklist

- [ ] Path 0: Pre-flight clean
- [ ] Path 1: Happy path
- [ ] Path 2: _[specific risk]_
- [ ] Path 3: _[specific risk]_
- [ ] Path N: Adversarial
- [ ] Path N+1: Integrity
- [ ] Path N+2: Cleanup

## What to do with results

_Failure categorization: which failures block the sprint vs. which are
acceptable findings. The criteria depend on the sprint's risk profile._
