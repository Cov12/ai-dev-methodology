# Pattern: Webhook Side Effects and Visitor Privacy

## When to use

A user-facing endpoint (a contact form, a signup, a webhook receiver)
triggers downstream side effects: notifications, logging, CRM pushes,
workflow engines.

## Rules

- **Side effects never change the user-facing response.** They run after
  (or alongside) the response; their success or failure does not alter what
  the visitor sees. Failures are logged and alerted on, not surfaced as a
  different response.
- **Send no visitor IP downstream.** Do not forward the visitor's IP
  address to third-party or downstream services. Forward only the fields
  the downstream actually needs.
- **Still fail closed at the boundary.** Validate and authenticate the
  inbound request before any side effect fires (`principles/04`).

## Why

Coupling the response to side effects leaks internal state, makes a
downstream outage look like a broken form, and invites retries that
duplicate side effects. Forwarding visitor IPs widens the privacy surface
for no product benefit.

Related: `principles/02-trust-boundaries.md`, `principles/04-fail-closed.md`.

## Files referenced

| File | What it is / does | When to use or change it | Key contents and gotchas |
|---|---|---|---|
| User-facing endpoint handler (placeholder, e.g. a contact-form service) | Code that answers the visitor and then fires side effects. | Change when forwarded fields or the response contract change. | Response must not depend on side-effect results; forward only needed fields, no visitor IP. |
| `principles/02-trust-boundaries.md` | Principle: trust-boundary code needs explicit human review. | When reviewing code that accepts inbound requests or forwards data. | A webhook receiver and its downstream calls sit on a trust boundary. |
| `principles/04-fail-closed.md` | Principle: refuse by default on uncertainty or failure. | When designing auth, error handling, secrets or startup checks. | Corollary "required secrets": entrypoints exit `FATAL` on an empty required secret; compose uses `${VAR:-}`. |
