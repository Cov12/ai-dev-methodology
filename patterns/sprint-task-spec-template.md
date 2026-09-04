# [Project] — Sprint N Task Spec

**Sprint name:** [Short descriptive name]
**Sprint duration:** [N weeks / working days]
**Sprint prerequisite:** [What must be true before sprint can start]
**Document type:** Executable sprint plan
**Revision context:** [Only if revised from an earlier version]

---

## 1. Sprint Goal

_One paragraph stating what success looks like at end of sprint. Be specific about what will be true that isn't true now._

## 2. Pre-Sprint Decisions Required

_List any decisions that must be made before tickets can be drafted. Each decision becomes an ADR. Include the recommendation if you have one._

- **Decision 1:** _Topic. Recommendation. ADR-NNNN._
- **Decision 2:** _Topic. Recommendation. ADR-NNNN._

## 3. Sprint Ticket Map

_Dependency-ordered ticket list. Show the dependency tree so the agent or developer knows what can parallelize._

- **T1:** _First prerequisite task_
- **T2:** _Depends on T1_
- **T3:** _Can run in parallel with T2_

## 4. Agent Execution Notes

_Use this section when an AI agent is contributing to the sprint. Keep the instructions operational, not aspirational._

- **Allowed scope:** _What the agent may draft directly_
- **Review-required paths:** _Auth, payments, migrations, compliance, or other trust-boundary files_
- **Verification required before done:** _What must be exercised in the running system_
- **Stop-and-ask triggers:** _Unknown contracts, missing env state, ambiguous acceptance criteria, or conflicting artifacts_
- **Artifacts to update:** _ADR, retro note, smoke plan, handoff, or changelog_

## 5. Acceptance Criteria

_Write criteria that can fail. Avoid vague success language._

- _Criterion 1_
- _Criterion 2_
- _Criterion 3_

## 6. Verification Plan

_List the checks that prove the sprint goal was met in reality, not just in code._

- _Smoke test 1_
- _Integration check 2_
- _Observed side effect 3_
