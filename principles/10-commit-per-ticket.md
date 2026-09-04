# Principle 10: Commit Per Ticket, Not Per Sprint

## Principle

Every completed ticket gets its own commit, with the ticket ID or equivalent scope marker referenced in the commit message, before the developer or assistant moves to the next ticket.

Readable git history is institutional memory.

## Why

Git history is the most durable record of what changed and when. A clean history with per-ticket commits lets you:

- revert a specific unit of work if it caused a regression
- understand what actually shipped
- bisect to find when a bug was introduced
- generate changelogs and release notes
- onboard contributors without replaying the whole project orally

A history that batches a sprint into one giant “done” commit loses all of this.

## Where this came from

This principle became obvious after a verification cycle found that a large batch of completed work was still sitting uncommitted in a working tree. There was no clean attribution from change to ticket, no easy rollback point, and unnecessary risk that unrelated work would be bundled together or lost.

The rescue required a manual pass to separate the work into deliberate commits after the fact. That is exactly the cleanup cost this rule is meant to avoid.

## How to apply

1. Work on a scoped branch or a clearly bounded sprint branch.
2. Commit as each ticket or logical unit completes.
3. Use one commit when the change is cohesive, or several commits when the ticket contains multiple meaningful steps.
4. Push regularly enough that the work is preserved remotely.
5. Treat clean commit structure as part of definition-of-done, not post-hoc housekeeping.

## Pattern references

- `operations/commit-hygiene.md`
