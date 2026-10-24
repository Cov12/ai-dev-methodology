# Mirrored Upstream Repos: Disable Upstream-Only CI

How to handle CI that comes along when you fork or mirror an upstream
project.

## The core principle

An upstream's GitHub Actions workflows arrive with the code. Some depend on
the upstream's private GitHub App, secrets, or release process, and will
fail or misbehave in your copy.

## How to apply

- Inventory `.github/workflows/` right after mirroring.
- Disable workflows that require the upstream's private GitHub App or
  secrets. Typical examples: backport bots, security-fix publishing,
  release-branch cleanup.
- Record which ones were disabled and why, so the next upstream sync does
  not silently re-enable them.
- Keep custom work in a dedicated directory so syncs stay clean (see
  `patterns/shared-platform-repo-structure.md`).

## Files referenced

| File | What it is / does | When to use or change it | Key contents and gotchas |
|---|---|---|---|
| `.github/workflows/*.yml` | GitHub Actions workflows inherited from upstream. | Inventory right after mirroring and after each sync. | Disable ones needing the upstream's private GitHub App or secrets (e.g. backport bots, security-fix publishing, release-branch cleanup); write down which and why. |
| `custom/` (placeholder for the dedicated custom-work directory) | Holds all your changes to a mirrored upstream codebase. | Put every customization here instead of editing upstream files. | Keeps upstream syncs conflict-free. |
| `patterns/shared-platform-repo-structure.md` | Pattern: one shared codebase, a deploy folder per deployment, a content repo per deployment. | When one platform serves several customers or environments. | Custom work in a dedicated directory; deploy folders hold env templates, never values. |
