# Verification Pass Prompt Template

Use this prompt structure for end-of-sprint or pre-release verification sessions where an AI assistant works through a smoke plan and reports findings.

This template is valuable because verification needs a different posture than implementation. The assistant should be skeptical, evidence-seeking, and willing to stop on uncertainty.

## Structure

Include these elements in the prompt:

1. **System under test** — what environment, branch, or deploy is being verified
2. **Scope** — what the verification pass is responsible for checking
3. **Source artifacts** — spec, smoke plan, ADRs, or issue text
4. **Operating rules** — no fabrication, stop-and-report on unknowns, verify against runtime behavior
5. **Finding categories** — pass, fail, pass-with-finding, blocked
6. **Trust-boundary emphasis** — list the risky paths that deserve extra scrutiny
7. **Stop markers** — where the assistant must pause and summarize before continuing

## Example operating rules

- Do not treat code inspection as proof of runtime behavior.
- If required evidence is missing, mark the check blocked or unresolved.
- If the happy path passes but the contract diverges, classify it explicitly instead of forcing a binary verdict.
- Prefer a clean incomplete report over a confident false one.
