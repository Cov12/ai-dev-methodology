# Human-Run Command Blocks

How to run a deploy or diagnosis when the AI agent cannot reach the target
machine directly (no SSH, no credentials, policy forbids it).

## The core principle

The human runs the commands and pastes the output back. The agent's job is
to make that loop safe, low-friction, and driven by observed output rather
than expected output.

## Rules

- Give **one copy-paste block per step**, not scattered fragments the human
  has to assemble.
- Make blocks safe to re-run. Guard edits so they are idempotent and
  reversible:
  - check before appending, e.g. `grep -q '^KEY=' file || echo 'KEY=...' >> file`
  - edit in place with a backup, e.g. `sed -i.bak 's/old/new/' file`
- End each block with a check that prints the resulting state (a status
  code, a config line, a container status) so the pasted output is evidence.
- Ask for the output to be pasted back, and base the next step on that
  output, not on what the block was expected to do
  (`principles/05-evidence-over-assumption.md`).
- Never ask the human to paste secret values back into the chat. Print
  whether a secret is set (e.g. its length), not what it is.

## Files referenced

| File | What it is / does | When to use or change it | Key contents and gotchas |
|---|---|---|---|
| `file` in the guard examples (typically `.env` or a config file) | The file a block edits. | Any block that appends or substitutes config. | `sed -i.bak` leaves a `.bak` copy for undo; the `grep -q` guard makes a re-run a no-op. |
| `.env` / `.env.example` | `.env` holds the deployment's env values, secrets included, and is never committed; `.env.example` is the committed names-only template. | Edit `.env` on the server when a value changes; edit `.env.example` when a variable is added. | Refer to variables by name only. Add keys with a guarded `grep -q '^KEY=' .env \|\| echo ...`. |
| `principles/05-evidence-over-assumption.md` | Principle: end-to-end evidence beats "looks right". | When deciding whether a step or test actually worked. | Corollary "verify in the system of record": find the new execution/row, check the object is stored, cross-check backups. |
