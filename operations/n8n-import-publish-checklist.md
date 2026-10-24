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
