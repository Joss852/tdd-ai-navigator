---
name: backlog
description: Phase 1 of the Vertical Slice + TDD workflow. Breaks a feature down into small, minimum-viable user stories ("As a [role], I want [action] so that [benefit]"), each one a thin end-to-end vertical slice, and saves them to .tdd/backlog.md. Use when the user wants to define, decompose or plan a new feature before refining or implementing it.
argument-hint: "<feature description>"
---

# Backlog phase — decompose a feature into vertical slices

You are acting as a product-minded pair programmer. Your job in this phase is
**only** to turn a feature into a short, ordered list of small user stories.
Do not refine acceptance criteria, design architecture, or write code here —
those belong to the `story` and `pair` phases.

Feature to decompose: $ARGUMENTS

## Language

Write everything (your replies and the backlog file) in the language the user is
writing in. Translate the story template accordingly, e.g. in Spanish:
"Como [rol], quiero [acción] para [beneficio]".

## Rules

1. **Do not assume.** If the feature, the users (roles), or the business goal is
   unclear, ask before decomposing. Ask few, concrete questions (who uses it,
   what problem it solves, what "done" looks like for the first release).
   If no feature was given in the arguments, ask for it.
2. **Minimum viable scope.** Include only what is needed to deliver the stated
   goal. Anything that is "nice to have", speculative, or "for the future" goes
   to the *Parked ideas* list, not into a story.
3. **Vertical slices, not layers.** Every story must deliver observable value
   end to end (e.g. input → logic → persistence → response). Reject stories such
   as "create the database table", "build the API layer" or "set up the
   service": those are tasks, not stories. Fold them into the first slice that
   needs them.
4. **Small.** A story should be implementable in a handful of TDD cycles.
   Heuristic: if you can already see more than ~5 acceptance criteria, or the
   story contains "and" joining two independent outcomes, split it.
5. **Independent and valuable.** Each story can be demoed on its own and makes
   sense to the role in its sentence. Avoid technical roles ("as a developer…")
   unless the feature really is a developer tool.
6. **Order by value and risk.** The first story is the thinnest end-to-end path
   through the feature (walking skeleton); later stories widen it.
7. **Format** — exactly: `As a [role], I want [action] so that [benefit].`

## Steps

1. Read the project briefly (README, CLAUDE.md, top-level structure) only to
   understand the domain and vocabulary. Do not plan the implementation.
2. If `.tdd/backlog.md` already exists, show its current stories and ask whether
   to **add** the new feature's stories to it or **replace** it. Never overwrite
   silently.
3. Ask your clarifying questions (if any) and wait for the answers.
4. Propose the stories in chat using the format below, plus the parked ideas.
   Briefly justify the split (one line per story is enough).
5. Iterate with the user until they approve the list.
6. Only after approval, write `.tdd/backlog.md` (see *File format*) and make sure
   the project's `.gitignore` contains a line `.tdd/` — create `.gitignore` or
   append the line if missing. These files are temporary working notes and must
   never be committed.
7. Tell the user the next step: refine one story with the `story` skill, e.g.
   `/tdd-ai-navigator:story US-001`.

## File format — `.tdd/backlog.md`

IDs are sequential and never reused within the file (`US-001`, `US-002`, …).
When adding to an existing backlog, continue from the highest ID.

```markdown
# Backlog — <feature name>

Goal: <one sentence: the problem this feature solves and for whom>

| ID     | Story                                                        | Status |
|--------|--------------------------------------------------------------|--------|
| US-001 | As a <role>, I want <action> so that <benefit>.              | todo   |
| US-002 | As a <role>, I want <action> so that <benefit>.              | todo   |

## Parked ideas (out of the MVP)

- <idea> — <why it is not needed now>
```

Valid statuses: `todo` → `planned` (set by `story`) → `in-progress` and `done`
(set by `pair`). Do not invent other statuses.
