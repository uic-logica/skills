---
name: logica-test
description: >
  Write a focused test for changed code in LOGICA @ UIC's frontend or backend
  repo — scoped to what changed, using whatever test runner the repo already
  has (and asking before adding one if it doesn't). Use when the user says
  "write tests for this", "add test coverage", "test this function", or
  "/logica-test".
---

# LOGICA Test

Writes the smallest test that actually verifies the change — not a full suite nobody asked for.

## Step 1 — Check what's already there

```bash
cat package.json | grep -A3 '"scripts"'
find . -name "*.test.*" -o -name "*.spec.*" | grep -v node_modules
```

If a test runner is already configured (a `test` script, an existing `*.test.*` file), use it and match its existing style.

If there's genuinely nothing set up yet: **stop and ask before adding a new dependency.** Which test runner to standardize on is a team decision, not something to decide mid-PR. Say what you'd recommend (a lightweight option that fits Next.js 16, e.g. Vitest) and why, then wait.

## Step 2 — Scope the test to the change

One function fixed → one test for that function. One new endpoint → one test for its main path plus its one real edge case (bad input, missing auth, whatever actually matters here) — not exhaustive branch coverage for a small utility.

## Step 3 — Repo-specific shape

**Backend** (`backend` repo — API route handlers, Prisma):
- Test the handler function directly (import it, call it with a constructed request) rather than spinning up a real HTTP server, unless the PR is specifically about request/response wiring.
- If the code touches the database, test against the real schema shape — don't hand-write a mock object that could silently drift from `prisma/schema.prisma`. Prefer a real (test) database or Prisma's mock client over a hand-rolled fake.
- If the change is a role check, the test should include a case where the wrong role gets rejected — that's the whole point of the check.

**Frontend** (`frontend` repo — pages, components):
- Test what a user actually sees/does: renders the right content, the button does the thing, the error state shows on failure. Don't test implementation details (internal state, private helpers) that could change without the user-facing behavior changing.
- Mock the backend API call at the network boundary, not deeper — keeps the test honest about what's actually being exercised.

## Step 4 — Run it

Run the test and confirm it fails without your fix and passes with it, when that's easy to check. A test that was never seen to fail is not proof of anything.
