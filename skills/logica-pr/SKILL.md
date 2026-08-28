---
name: logica-pr
description: >
  Open a pull request the way LOGICA @ UIC does it: branch off main, lint and
  typecheck locally, link the tracking issue, fill out the PR template, push,
  and open the PR with gh. Use when the user says "open a PR", "make a PR",
  "ship this", "create a pull request", or "/logica-pr". Never pushes directly
  to main — this repo's branch protection blocks that anyway.
---

# LOGICA PR

Opens a pull request that matches [CONTRIBUTING.md](https://github.com/uic-logica/.github/blob/main/CONTRIBUTING.md)'s workflow. Nobody pushes straight to `main`, even leads — every change goes through a PR with a passing lint check and one approval.

## Step 1 — Make sure you're not on `main`

```bash
git branch --show-current
```

If it's `main`, create a branch first: `git checkout -b <yourname>/<short-description>` (e.g. `maria/feed-endpoint`). Use the git config name if you have one (`git config user.name`), otherwise ask.

## Step 2 — Run the same checks CI runs

```bash
npm run lint
npx tsc --noEmit
npm ci
```

Fix anything that fails before continuing — CI will block the merge otherwise, so catching it now saves a review round-trip.

If you added or updated a package, `npm ci` above will catch a `package-lock.json` that's out of sync with `package.json` (CI uses `npm ci`, which refuses to install when they don't match — it won't just update the lock file like `npm install` does). If that happens, run `npm install` and commit the regenerated `package-lock.json` alongside your change.

## Step 3 — Find the tracking issue

Every roadmap piece of work has an issue (`[Step N] ...` or `[Addition] ...`, labeled `roadmap`) on the repo you're in. Look it up:

```bash
gh issue list -R uic-logica/<frontend-or-backend> --label roadmap
```

If there genuinely isn't one yet, use the `logica-issue` skill to file it first — don't ship untracked work.

## Step 4 — Commit and push

Only commit what belongs to this change. Push the branch:

```bash
git push -u origin <branch-name>
```

## Step 5 — Open the PR

```bash
gh pr create --title "<short description>" --body "$(cat <<'EOF'
## Summary
<1-3 bullets on what changed and why>

Closes #<issue-number>

## Test plan
<what you ran / checked>
EOF
)"
```

The org's PR template auto-applies — keep the summary short and let the linked issue carry the "why." Don't self-merge: branch protection requires one approval and a passing `lint` check before a maintainer merges it.
