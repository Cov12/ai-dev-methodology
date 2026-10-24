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
