# Pattern: Shared Platform, Per-Deployment Config, Per-Deployment Content

## When to use

Delivering the same platform to multiple customers or environments, where
each needs its own deployment config and its own automation content (for
example, workflow definitions).

## The pattern

- **One shared codebase.** If it is a fork or mirror of an upstream
  project, isolate custom work in a dedicated directory so upstream syncs
  stay clean (see `operations/mirrored-upstream-workflows.md`).
- **One deploy folder per deployment**: compose files, overrides, env
  templates (never env values), and deploy docs with the exact commands
  (`operations/shared-server-deploy.md`).
- **One separate content repo per deployment**: the deployment-specific
  content, versioned independently of the platform.

## Why

Platform fixes land once. Config and content can't leak across
deployments. Each layer has its own history, review, and rollback.

## Files referenced

| File | What it is / does | When to use or change it | Key contents and gotchas |
|---|---|---|---|
| `custom/` (placeholder for the dedicated custom-work directory) | Holds all your changes to a mirrored upstream codebase. | Put every customization here instead of editing upstream files. | Keeps upstream syncs conflict-free. |
| `deploy/<deployment>/` (placeholder) | One folder per deployment: compose files, overrides, env templates, deploy doc. | Change when that deployment changes. | Holds `.env.example`, never `.env` values. |
| `<deployment>-workflows` repo (placeholder) | The deployment-specific content (e.g. workflow JSON), versioned separately. | Commit after exporting changed content. | Never commit credentials, `.env` or encryption keys. |
| `operations/mirrored-upstream-workflows.md` | How to handle upstream CI after forking or mirroring. | Right after mirroring and after each upstream sync. | Disable workflows that need the upstream's private GitHub App or secrets; record what was disabled. |
| `operations/shared-server-deploy.md` | Runbook for deploying containers onto a server that already hosts other services and runs its own reverse proxy. | Read before any deploy to a shared host. | Loopback-only override, reuse the existing vhost, exact compose command, back up and tag, ownership boundary. |
