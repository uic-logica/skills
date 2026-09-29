---
name: logica-review
description: >
  Review a diff or PR against LOGICA @ UIC's specific standards, not just
  generic code quality: server-side role checks, committed Prisma migrations,
  no secrets, accessible frontend markup, and scope matching the linked
  team issue. Use when the user says "review this PR", "review my diff",
  "review this branch", or "/logica-review".
---

# LOGICA Review

A focused checklist for this project's two repos — on top of normal correctness review, check the things that are easy to miss and specific to how we've built this.

## Step 1 — Get the diff

```bash
git diff main...HEAD
```

Or for a PR: `gh pr diff <number>`.

## Step 2 — Universal checks (both repos)

- `npm run lint` and `npx tsc --noEmit` pass.
- No secrets: nothing that looks like a token, API key, or connection string; `.env*` files aren't staged (only `.env.example` should ever be committed).
- The PR is linked to a team-labeled issue (`team: …`) and doesn't quietly do more than that issue describes — scope creep belongs in its own issue.

## Step 3 — Backend-specific checks (`backend` repo)

- **Role checks happen on the server**, not just hidden in a frontend button. A raw request from a `MEMBER` should never be able to do what only `BOARD`/`EXEC_BOARD` can.
- **Every schema change has a migration** committed alongside it (`prisma/migrations/...`) — no hand-edited database assumptions.
- **Prisma client comes from `lib/prisma.ts`**, never `new PrismaClient()` inline — that's what avoids exhausting connections in dev.
- Auth/session logic isn't touched unless the PR is specifically about auth — that code is shared infrastructure, changes there affect every feature.

## Step 4 — Frontend-specific checks (`frontend` repo)

- Loading, empty, and error states are handled, not just the happy path.
- Forms have labeled inputs and are keyboard-navigable; nothing interactive is mouse-only.
- Data comes from the backend API (`NEXT_PUBLIC_API_URL`) — no hardcoded fixture data shipping to production, no client-side storage of anything that should live server-side.

## Step 5 — Report

List findings by file, each as: what's wrong, why it matters, and the concrete fix. Note anything genuinely good, too — a review isn't only a defect list. If everything's clean, say so plainly instead of manufacturing nitpicks.
