# Probe From Inside the Calling Container

How to debug a failing call between two services on an internal container
network.

## The core principle

When service A calls service B, test the call **from inside A's
container**. That reproduces the same network path, name resolution, and
headers A uses, and shows B's real status code and error body, which an
external curl or A's own sanitized user-facing error can hide.

## How to apply

- Exec into A's container and call B the way A does (same hostname, port,
  path, headers).
- Print the status code and the response body. Do not rely on A's
  user-facing message; it is often deliberately generic.
- Send any auth header by referencing the env var already present in the
  container (e.g. `"$WEBHOOK_SECRET"`); never echo or paste the value.
- Then confirm the effect in B's own records
  (`principles/05`, corollary: verify in the system of record).
