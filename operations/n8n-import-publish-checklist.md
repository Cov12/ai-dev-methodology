# n8n 2.x: Import, Credential, Publish Checklist

How to make an imported webhook workflow actually live in n8n 2.x.

## The core principle

`n8n import:workflow` stores the workflow. It does not make a webhook
workflow live with working auth. In n8n 2.x, saving only updates the
draft; the **published** version is what serves webhooks.

## Checklist

After importing:

1. Create the **Header Auth** credential in the UI. Header name e.g.
   `X-Webhook-Secret`; value = the `WEBHOOK_SECRET` env value. Refer to the
   credential and env var **by name only**; never paste the value into
   docs, commits, or chat.
2. Open the workflow and attach that credential to the Webhook node.
3. **Save**, then **Publish again**.
4. Restart n8n.
5. Send a test request and **verify a new run under Executions**, not
   just the HTTP response (`principles/05`, corollary: verify in the system
   of record).

## Symptoms

| Symptom | Meaning |
|---|---|
| "webhook not registered" | The workflow is not live (not published/active), or was never stored at all. |
| HTTP 500 with "No authentication data defined on node!" | The published version lacks the credential. Attach it, Save, Publish again. |

When the caller is another container, probe from inside it to see these
errors directly (`operations/probe-from-calling-container.md`).

## Files referenced

| File | What it is / does | When to use or change it | Key contents and gotchas |
|---|---|---|---|
| `workflows/<workflow-name>.json` (placeholder) | Exported n8n workflow definition, without credentials. | Import to deploy; re-export and commit after UI edits. | Import stores the workflow only: no credential attached, not published. |
| `WEBHOOK_SECRET` (env var in `.env`) | Shared secret the caller sends and the Header Auth credential checks. | Set once; rotate by updating `.env` and the credential, then recreate the containers that read it. | Refer to it by name; never paste or echo the value. |
| Header Auth credential (n8n UI, not a file) | Holds the webhook header the Webhook node checks. | Create after import; update when the secret rotates. | Header name e.g. `X-Webhook-Secret`. Missing from the published version → HTTP 500 "No authentication data defined on node!". |
| n8n Executions list (n8n UI, not a file) | n8n's record of each workflow run. | The proof step after any test request. | Look for a **new** run of the workflow under test. |
| `principles/05-evidence-over-assumption.md` | Principle: end-to-end evidence beats "looks right". | When deciding whether a step or test actually worked. | Corollary "verify in the system of record": find the new execution/row, check the object is stored, cross-check backups. |
| `operations/probe-from-calling-container.md` | How to debug a failing call between two containers. | When service A's call to service B fails or A shows only a generic error. | Exec into A, call B exactly as A does, print status and body, reference secrets as env vars. |
