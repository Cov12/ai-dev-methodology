# [Project] — Agent Review Boundaries v1

**Purpose:** This file defines which code in this project is agent-draftable versus which requires explicit human review. The boundary is intentional and documented.

**Status:** Active. Changes to this file require deliberate review.

---

## Heavy review required

Code in these categories requires human review before merging. An AI assistant may draft, but a human should read the affected logic carefully before it lands.

### Authentication and session management

_File paths or path globs. Examples:_
- _src/lib/auth/*_
- _src/middleware.ts_

### Payment processing

- _src/lib/payments/*_
- _src/app/api/webhooks/payments/*_

### Attestation and consent

- _src/lib/compliance/attestation*_
- _Any route or action handling consent capture_

### Audit logging

- _src/lib/audit/*_

### AI safety guardrails

- _src/lib/ai/guardrails/*_
- _src/lib/ai/system-prompts.ts (the prompt logic itself)_

### Cryptographic operations

- _src/lib/*/signed-url.ts_
- _Anything calling crypto.* directly_

### Compliance enforcement

- _src/lib/compliance/geographic-restrictions.ts_
- _src/lib/compliance/age-gate.ts_

### Schema and migrations

- _packages/db/prisma/schema.prisma_
- _packages/db/prisma/migrations/*_

---

## Agent-draftable

Code in these categories can usually be drafted first with AI, with normal spot-checking and integration testing still applied.

### UI components and styling

- _src/components/*_
- _src/app/**/page.tsx (rendering paths, not trust-boundary actions)_

### Static content

- _content/*_
- _public-facing copy_
- _reference docs_

### Tests

- _**/__tests__/*_
- _**/*.test.ts_

### Build configuration

- _next.config.ts_
- _tailwind.config.ts_
- _package.json_

### Documentation

- _docs/**/*.md_
- _README.md_

---

## Middle ground (case by case)

### Server actions
_The action body is often middle ground. Trust-boundary inputs require review; routine validation and non-sensitive persistence often do not._

### Database queries
_Plain CRUD against non-sensitive tables is often AI-draftable. Queries touching payments, attestation, compliance, or audit data deserve review._

### Background jobs
_Job structure and queue setup may be AI-draftable. Side effects within jobs should be reviewed according to their actual risk._

---

## Enforcement

Examples:
- _PRs touching heavy-review paths require explicit reviewer signoff_
- _Agent instructions reference this file_
- _CI can list changed paths and flag heavy-review areas_

---

## Updates

When new code categories emerge, update this file deliberately. Changes to review boundaries are themselves trust-boundary decisions.
