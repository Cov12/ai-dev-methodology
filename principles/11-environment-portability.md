# Principle 11: Environment Portability

## Principle

Version control carries code state. It does not carry environment state. Identify the machine-bound state your project depends on, and commit an explicit reproduction path for it alongside the code.

## Why this matters

When environment state is undocumented, every new machine rediscovers the same gaps at the worst possible time: when trying to run a real deliverable.

## Where this came from

This principle came from a cross-machine provisioning failure. One machine had the necessary runtimes, CLIs, and authenticated state. The second machine had the code, but not the environment required to run it. The repo was intact. The environment was not.

The lesson is not “remember to check your setup.” The lesson is: if the project does not have a committed reproduction path, environment checks will always be improvised.

## What belongs in a reproduction path

At minimum, commit a setup runbook that covers:

- runtimes and package managers
- required CLI tools
- how to acquire credentials without storing them in git
- required environment variables and where to set them
- a smoke command that proves the environment works

## Corollary: your schema-apply mechanism is part of the environment contract

Environment state includes *how the deploy applies schema changes* — and that
mechanism has a failure mode you must know before you design the change, not
after the deploy fails.

A push/auto-sync apply (many migration tools offer one: `prisma db push`,
`drizzle-kit push`, etc.) that runs **without** an explicit data-loss flag will
happily make **additive** changes but will **refuse or fail** on anything
destructive — dropping a model/table, narrowing a column, removing an enum
value. The build then fails at deploy time, or silently no-ops the drop.

Consequences for design:

- **Keep schema changes additive** unless you have deliberately planned a
  destructive step. Additive (new nullable columns, new tables) is safe under
  push-apply; destructive is not.
- **A destructive change needs a coordinated step** — an explicit
  data-loss-accepting migration run, sequenced with the deploy, not a plain
  schema edit that rides the normal build.
- **Dead code can't always just be deleted.** Removing a model that still has a
  live consumer, or that maps to a table with data, is a multi-step ordered
  change (migrate consumers off → then drop), not a one-line cleanup.

### Where this came from

A cleanup task set out to delete two dead ORM models. The deploy applied schema
through a push-style command with no data-loss flag, so the deletion would have
failed the production build — and one model still had a live consumer. What
looked like a one-line cleanup was a sequenced, coordinated migration, and was
correctly deferred.

### How to apply

Before any schema change, answer: **what does my deploy's apply step do with a
destructive diff?** If it refuses data loss, keep the change additive, or plan
the destructive step explicitly (ordered consumer migration + a
data-loss-accepting apply, coordinated with the deploy).
