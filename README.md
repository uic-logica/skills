# LOGICA @ UIC — skills

Claude Code skills for how we actually work — not generic advice, they're built around [CONTRIBUTING.md](https://github.com/uic-logica/.github/blob/main/CONTRIBUTING.md) and [ROADMAP.md](https://github.com/uic-logica/.github/blob/main/ROADMAP.md). Install once, then type `/` in any LOGICA repo (or ask for the thing in plain language) and Claude follows our process instead of a generic one.

## Install (one time)

```
/plugin marketplace add uic-logica/skills
/plugin install logica-workflow@logica-skills
```

That's it — the skills are then available in every project, not just this one.

## What's in it

| Skill | Use when you want to... |
|---|---|
| [`logica-pr`](skills/logica-pr/SKILL.md) | Open a PR: branch off `main`, lint + typecheck locally, link the tracking issue, fill the PR template, push, `gh pr create`. |
| [`logica-review`](skills/logica-review/SKILL.md) | Review a diff or PR: role checks happen server-side, migrations are committed, no secrets, accessible frontend markup, scope matches the linked issue. |
| [`logica-test`](skills/logica-test/SKILL.md) | Write a test for something you changed — scoped to the change, using whatever runner the repo already has (asks before adding a new one). |
| [`logica-issue`](skills/logica-issue/SKILL.md) | File a new issue in the same `[Step N]` / `[Addition]` format as the rest of the roadmap tracker, with the right labels and a real "done when" line. |
| [`logica-lean`](skills/logica-lean/SKILL.md) | Cut a solution down to size for *our* stack specifically — Tailwind/Prisma/Auth.js already cover a lot, use that before reaching for a new dependency or abstraction. Same spirit as the general-purpose `ponytail` skill, just aimed at our exact tools. |

Say what you want in plain language ("open a PR for this", "review my branch", "is this over-engineered?") — Claude picks the matching skill on its own. You can also invoke one directly by name once it's installed.

## Why this exists

Anyone can install Claude Code and just start typing — but "start typing" doesn't know our branch-protection rules, our label conventions, or that role checks have to happen on the backend. These skills encode that once, here, so nobody has to re-explain it in every PR review.

## Adding a new skill

1. `mkdir skills/<name>` and write `skills/<name>/SKILL.md` — frontmatter needs `name` and a `description` that states what it does and when to use it (the description is what Claude matches against, so be specific).
2. Validate before pushing: `claude plugin validate .` from the repo root.
3. Add a row to the table above.
4. PR it like anything else — see [CONTRIBUTING.md](https://github.com/uic-logica/.github/blob/main/CONTRIBUTING.md).

Anyone with the plugin installed picks up new/updated skills automatically the next time Claude Code checks for marketplace updates (or immediately via `/plugin marketplace update logica-skills`).
