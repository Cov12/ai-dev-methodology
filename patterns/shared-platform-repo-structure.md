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
