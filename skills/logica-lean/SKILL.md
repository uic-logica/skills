---
name: logica-lean
description: >
  Push back on over-engineering in LOGICA @ UIC's specific stack (Next.js 16,
  Tailwind, Prisma, Auth.js) — the smallest working solution that uses what
  the stack already gives you, instead of a new abstraction or dependency.
  Use when the user says "make this simpler", "is this over-engineered",
  "minimize this", "too many lines", or "/logica-lean".
---

# LOGICA Lean

Our version of "does this really need to exist" — aimed at the exact stack we already picked, so it doesn't just repeat generic advice. Stop at the first rung that holds:

1. **Does this need to exist at all?** If it's for a milestone nobody's started yet, skip it and say so — don't build ahead of the step you're on.
2. **Does Tailwind or plain HTML already do it?** A native `<input type="date">`, `<dialog>`, or CSS behavior beats a component library import, every time.
3. **Does Prisma already model this?** Check `prisma/schema.prisma` before adding a parallel data structure, a second source of truth, or an in-memory cache of something the database already answers directly.
4. **Does something already in `lib/` do this?** Especially `lib/prisma.ts`'s shared client — never `new PrismaClient()` in a route.
5. **Can it be one line?** One line.
6. **Only then:** the minimum new code that works.

## What this catches

- A new npm dependency for something a few lines of Tailwind/Prisma/vanilla JS already covers.
- An interface or config option with exactly one implementation and no second one planned.
- A generic "form builder" abstraction hand-built before Step 7 actually calls for one.
- Custom validation/auth logic duplicating what Auth.js's `signIn`/`session` callbacks already do.
- Boilerplate "for later" — a milestone that hasn't started yet doesn't need scaffolding today.

## What never gets simplified away

Role checks, input validation at API boundaries, accessibility basics, and anything the user explicitly asked for. Lazy means less code, not fewer guarantees.

## Marking a deliberate shortcut

If you take a shortcut with a known ceiling (skip pagination because the table's tiny today, skip retries because this endpoint isn't called yet), leave a one-line comment naming the ceiling and what would trigger revisiting it:

```
// logica-lean: no pagination — fine under ~50 rows, revisit if the member list grows past that
```

## Output

Show the simplified code, then one line: what got cut and when to add it back. No essay defending the simplification — if the explanation is longer than the change, that's a sign to just show the code.
