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
