---
name: logica-issue
description: >
  File a new GitHub issue on the frontend or backend repo in LOGICA @ UIC's
  tracking format — labeled with its product team, linked to that team's page,
  with a concrete "done when" line. Use when the user says "file an issue",
  "open an issue for this", "track this", or "/logica-issue".
---

# LOGICA Issue

Every issue belongs to one product team, so `gh issue list --label "team: <name>"` is that team's board.

## Step 1 — Where it belongs

- Repo: `frontend` or `backend` (file one in each if the work touches both).
- Team — exactly one of: `team: site`, `team: opportunity-board`, `team: resume-builder`, `team: event-replays`, `team: mock-interviewer`. Team pages: https://github.com/uic-logica/.github/tree/main/projects

## Step 2 — Title

A plain, specific title of what changes ("Feed filters roles by class year"). No `[Step N]` prefixes — that format is retired.

## Step 3 — Labels

The team label, plus `bug`, `enhancement`, `accessibility`, `security`, `design` or `docs` when they genuinely apply. Reuse existing labels (`gh label list -R uic-logica/<repo>`); don't invent new ones.

## Step 4 — Body

```markdown
Team: [<team>](https://github.com/uic-logica/.github/tree/main/projects/<team>) · Milestone: <GitHub milestone, e.g. "Opportunity board · MVP">

**What:** <one or two sentences>

**Depends on:** <issue, or omit>

**Done when:** <one concrete, checkable statement>
```

## Step 5 — Create it

```bash
gh issue create -R uic-logica/<repo> --title "<title>" --label "team: <name>,<type>" --milestone "<milestone>" --body "<body>"
```

Read the body back first — vague "done when" lines are how trackers rot.
