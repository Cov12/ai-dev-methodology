# Promote Verification Gaps into Reusable Browser Smoke Tests

## Trigger

Use this pattern when a fix claims that user-visible persistence, routing, tenant isolation, authentication, or workflow continuity is resolved.

If the behavior matters to the person using the product, do not stop at utility tests or API checks. Add a repeatable browser smoke that exercises the actual UI path.

## Core principle

Code that looks correct is not the same as behavior that has been observed.

When a verification gap is discovered, promote it into repo-owned test infrastructure instead of leaving it as a one-off manual check or chat-thread note.

## Anti-pattern this prevents

A team can falsely feel done because:

- unit tests pass
- adapter tests pass
- direct API checks pass
- formatting and linting pass
- an implementation looks consistent with the intended architecture

Those checks are useful, but they do not prove the browser flow works. The UI can still fail because of routing, auth bootstrap, service workers, CORS, first-run modals, stale local state, selector ambiguity, or rehydration behavior.

## Required upgrade

Add a stable command that any engineer, agent, or CI job can run:

```bash
npm run smoke:<feature>
```

The command should exercise the real browser path while keeping unrelated dependencies deterministic.

## Implementation shape

A durable browser smoke usually includes:

- Playwright or an equivalent browser runner
- a repo-owned config file
- a focused smoke spec under `tests/smoke/`
- a stable package script
- deterministic API mocks or seeded records
- stable UI selectors such as `data-testid`
- failure artifacts: screenshots, videos, traces, and console/error context

Prefer selectors that describe product behavior rather than CSS implementation:

```html
<div data-testid="feature-page">
<input data-testid="feature-search" />
<button data-testid="feature-recent-item" data-item-id="...">
<div data-testid="feature-message" data-role="user">
<div data-testid="feature-message" data-role="assistant">
<button data-testid="feature-new-item">
<textarea data-testid="feature-input"></textarea>
<button data-testid="feature-submit">
```

## Deterministic mock and seed rules

The smoke should verify the feature under test, not every external service around it.

When downstream systems are irrelevant to the claim, mock them with deterministic responses. Seed setup data through APIs where possible so browser time stays focused on the user-visible path.

A strong persistence smoke should usually seed:

1. an in-scope record that should appear
2. another valid variant of the same feature, if one exists
3. an out-of-scope negative-control record that must not appear

For tenant-aware systems, include tenant or scope metadata in the seed payload and assert that neighboring data stays hidden.

## Browser smoke assertions

For a user-visible persistence fix, the smoke should prove:

1. The page renders.
2. Existing in-scope records appear in the UI.
3. Out-of-scope records do not appear.
4. A new user action creates the expected persisted record.
5. A follow-up action updates or reuses the correct record rather than creating an accidental duplicate.
6. The persisted payload includes the expected feature metadata.
7. Reloading the page preserves the visible state.
8. Reopening a recent or saved item rehydrates the full user-visible history/context.
9. The test does not depend on live behavior from unrelated downstream systems.

## Hardening checklist

Before trusting a browser-smoke failure, harden the harness:

- [ ] Use the repo's scripted entrypoint, not a bare test-runner command that bypasses config.
- [ ] Install required browser binaries in setup documentation or CI bootstrap.
- [ ] Block or control service workers when route-mocking backend APIs.
- [ ] Return credential-compatible CORS headers when mocked requests use credentials.
- [ ] Suppress or defensively dismiss onboarding, changelog, cookie, or first-run modals.
- [ ] Use stable `data-testid` selectors instead of CSS classes or visual text alone.
- [ ] Scope locators when the same text can appear in multiple roles or containers.
- [ ] Capture screenshot/video/trace artifacts on failure.
- [ ] Clean generated test artifacts before committing unless they are intentionally part of a report.

## Verification commands

A practical verification set looks like:

```bash
npm run test:frontend -- --run
npm run smoke:<feature> -- --reporter=line
npx prettier --check <touched-files>
git diff --check
```

If backend behavior changed, add the relevant backend test slice before the browser smoke.

If full-repo checks are blocked by pre-existing debt, report that separately from the focused checks that prove the change.

## Definition of done

A user-visible persistence or workflow fix is done when:

- the browser smoke exists as a repo-owned command
- it exercises the real UI path
- it verifies create/update/list/reload/rehydration behavior as applicable
- it includes negative controls for scope or permissions where relevant
- focused unit/API tests still pass
- formatting and whitespace checks pass
- unrelated repo-wide failures are named honestly rather than blurred into the result

## Example: persistence verification gap

In one UI persistence fix, adapter tests and direct API checks passed, but the team still could not honestly claim browser-level verification. The missing proof was the actual user path: open the page, create a record, update it, reload, and confirm the recent item restored the same history.

The resolution was to add a first-class browser smoke with deterministic API mocks, stable selectors, seeded in-scope and out-of-scope records, failure artifacts, and a package-script entrypoint. The smoke then exposed harness issues that simpler tests missed: browser binary setup, credentialed CORS behavior, service-worker interception risk, a first-run modal blocking clicks, and ambiguous locators.

Each blocker made the test stronger. The lesson was not only "add an end-to-end test." The lesson was: **when verification uncertainty survives a fix, turn that uncertainty into reusable infrastructure.**