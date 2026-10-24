# Shared-Server Deploy

How to deploy a containerized service onto a server that already hosts
other services and already runs its own reverse proxy.

## The core principle

On a shared server you are a guest. Change as little of the host as
possible, make every deploy reversible, and write down the exact commands
so the deploy does not depend on anyone's memory.

## Bind to loopback, reuse the existing vhost

- Do not publish container ports on all interfaces, and do not add a
  second reverse proxy. Use a compose override file that binds services to
  `127.0.0.1` only.
- Reuse the server's existing Nginx (or equivalent) vhost and proxy it to
  the loopback port, instead of adding a duplicate vhost for the same name.

## Name the exact command

Deploy docs must state the exact compose invocation, including the
override file. If it is left implicit, someone will eventually run plain
`docker compose up -d` and the override will silently not apply. Generic
example:

```bash
docker compose -f docker-compose.yml -f docker-compose.override.yml up -d
```

## Back up and tag before every deploy

1. Back up before **every** deploy, not just the risky-looking ones.
2. Tag every release with a per-project sequence (e.g. `<project>-v1`,
   `<project>-v2`).

Rollback is then a checkout of the previous tag plus a restore of the
backup taken immediately before the bad deploy, with nothing reconstructed
from memory. Backups also double as evidence: if an object never appears in
any backup, it was never deployed (see `principles/05`).

## Secrets

Entrypoints fail closed on missing required secrets. See
`principles/04-fail-closed.md` (corollary: required secrets). Docs and
commits refer to secrets by env var name only, never by value.

## Ownership boundary

When the server, DNS, or network belongs to someone else's operations team:

- **DNS** changes are requested, not made.
- **Firewall or SSH lockdown** on a shared server needs the owner's sign-off
  before it is applied, because it affects services that aren't yours.
- Record what was requested, from whom (by role), and when it was approved.

## When the agent can't reach the server

See `operations/human-run-command-blocks.md`.
