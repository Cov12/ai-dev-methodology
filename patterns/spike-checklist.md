# Spike Checklist

A spike is a time-boxed investigation to answer a question or prove viability. This checklist helps ensure spikes answer the operational question, not just the “does it work once?” question.

## General Spike Checklist

Before declaring a spike complete, verify:

- [ ] **Happy path works** — the thing you're testing functions at least once
- [ ] **Failure path documented** — what happens when it fails? How do you know?
- [ ] **Operational path tested** — can this run continuously, or only once?
- [ ] **Dependencies documented** — what must be true for this to work?
- [ ] **Gap list explicit** — what did the spike not test that the design assumes?

## OAuth Integration Spikes

When spiking third-party OAuth integrations, explicitly validate:

1. **Token acquisition** — interactive flow works, tokens are stored
2. **Token validity** — measure actual lifetime; don't trust docs blindly
3. **Refresh behavior** — does the tool or library auto-refresh? Test this explicitly
4. **Re-auth path** — what happens when the refresh token expires? Who does it? How?

A spike that proves “it works once” has **not** proven operational viability.

### Example: OAuth integration spike

The spike validated:
- ✅ Token acquisition
- ✅ Token validity
- ✅ Direct API calls with the stored token

The spike did **not** validate:
- ❌ Automatic token refresh under real expiry conditions
- ❌ The exact behavior of the surrounding runtime when refresh is needed

That gap should be documented before the spike is treated as production-ready.

## API and Webhook Spikes

When spiking external APIs or webhooks:

1. **Auth works** — credentials accepted, basic call succeeds
2. **Error handling** — what does a 4xx or 5xx look like? Is the error parseable?
3. **Rate limits** — are there limits? What happens when hit?
4. **Idempotency** — can you safely retry?
5. **Callback verification** — if webhooks are involved, can you validate signatures?
