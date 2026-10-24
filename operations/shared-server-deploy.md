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

## Files referenced

| File | What it is / does | When to use or change it | Key contents and gotchas |
|---|---|---|---|
| `docker-compose.yml` | Base compose file defining the services. | Change when adding or reconfiguring a service. | Reference secrets as `${VAR:-}` so services that don't use a secret still start. |
| `docker-compose.override.yml` (placeholder name for the loopback override) | Extra compose file that publishes service ports on `127.0.0.1` only, so the host's existing proxy can reach them. | Use only on a host that already runs its own proxy; change when a published port changes. | Pass it explicitly with `-f` and write that exact command in the deploy doc. Gotcha: plain `docker compose up -d` only auto-loads a file literally named `docker-compose.override.yml`; an override with any other name is silently skipped. |
| `DEPLOY.md` (placeholder for the project's deploy doc) | The project's step-by-step deploy runbook. | Follow it for each deploy; update it whenever a command changes. | Must state the exact compose command, including the override file. |
| `.env` / `.env.example` | `.env` holds the deployment's env values, secrets included, and is never committed; `.env.example` is the committed names-only template. | Edit `.env` on the server when a value changes; edit `.env.example` when a variable is added. | Refer to variables by name only. Add keys with a guarded `grep -q '^KEY=' .env \|\| echo ...`. |
| `entrypoint.sh` (placeholder for each container's startup script) | Runs when the container starts. | Change when a service gains or loses a required secret. | Exit with a `FATAL` message if a required secret is empty, instead of starting degraded. |
| Existing reverse-proxy vhost (host config, not in the repo) | The shared host's proxy config for the public hostname. | Point it at the loopback port instead of adding a duplicate vhost. | Owned by the host's operator; changes go through them. |
| Git release tags (`<project>-v1`, `<project>-v2`, ...) | Immutable markers of each deployed release. | Create one before each deploy; check out the previous one to roll back. | Pair each tag with the backup taken just before that deploy. |
| Backups (database dumps or equivalent) | Pre-deploy snapshots. | Take one before every deploy; restore one on rollback. | Also evidence: an object missing from every backup was never deployed. |
| `principles/04-fail-closed.md` | Principle: refuse by default on uncertainty or failure. | When designing auth, error handling, secrets or startup checks. | Corollary "required secrets": entrypoints exit `FATAL` on an empty required secret; compose uses `${VAR:-}`. |
| `principles/05-evidence-over-assumption.md` | Principle: end-to-end evidence beats "looks right". | When deciding whether a step or test actually worked. | Corollary "verify in the system of record": find the new execution/row, check the object is stored, cross-check backups. |
| `operations/human-run-command-blocks.md` | How to run a deploy or diagnosis when the agent can't reach the machine and a human runs the commands. | Whenever an agent writes commands for a human to paste. | One guarded, re-runnable block per step, ending with a state check; next step based on pasted output; never secrets in chat. |
