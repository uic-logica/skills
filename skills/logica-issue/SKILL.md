---
name: logica-issue
description: >
  File a new GitHub issue on the frontend or backend repo in LOGICA @ UIC's
  tracking format — titled and labeled to match ROADMAP.md, linked back to
  the roadmap section it's part of, with a concrete "done when" line. Use
  when the user says "file an issue", "open an issue for this", "track this",
  or "/logica-issue".
---

# LOGICA Issue

Keeps every issue in the same shape as the existing roadmap tracker, so `gh issue list --label roadmap` stays a real project board and not a pile of inconsistent tickets.

## Step 1 — Figure out where it belongs

- Which repo: `frontend` or `backend` (or both — file one in each if the work has parts on both sides).
- Does it belong to a numbered step in [ROADMAP.md](https://github.com/uic-logica/.github/blob/main/ROADMAP.md), or is it an unordered item from the **Additions** list? If it's neither, ask whether it should be added to the roadmap first, or whether it's a plain `bug`/`enhancement` outside the roadmap (those don't need the `roadmap` label or the step-number title).

## Step 2 — Title

- Roadmap step: `[Step N] <short description>`
- Addition: `[Addition] <short description>`
- Anything else (bug, small fix): plain descriptive title, no prefix.

## Step 3 — Labels

Reuse the labels that already exist — don't invent new ones ad hoc:

```bash
gh label list -R uic-logica/<repo>
```

Roadmap issues get `roadmap` + the repo's own label (`frontend`/`backend`) + `enhancement` (unless it's foundational/onboarding work, which skips `enhancement`). Add `bug`, `accessibility`, `security`, `design`, or `docs` on top when they genuinely apply.

## Step 4 — Body

Match the existing format:

```markdown
Part of [Step N — <name>](https://github.com/uic-logica/.github/blob/main/ROADMAP.md#step-n--<slug>).

**What:** <what this issue covers, one or two sentences>

**Depends on:** <another issue/step, or omit this line if nothing blocks it>

**Done when:** <one concrete, checkable statement of completion>
```

For an Addition, link `#additions` instead of a step anchor.

## Step 5 — Create it

```bash
gh issue create -R uic-logica/<repo> \
  --title "<title>" \
  --label "<labels, comma-separated>" \
  --body "<body>"
```

Read the body back before creating — vague "done when" lines are the most common way these trackers rot into noise.
