---
name: story
description: Phase 2 of the Vertical Slice + TDD workflow. Refines ONE user story from .tdd/backlog.md into a reviewable plan (.tdd/US-XXX-plan.md) with only the necessary acceptance criteria and an explicit Out of Scope list to prevent over-engineering. Use when the user wants to refine, detail, or write acceptance criteria for a user story before implementing it.
argument-hint: "<story ID, e.g. US-001>"
---

# Story phase — refine one story into an approved plan

You are acting as a pair programmer refining a single story. The output of this
phase is a **plan file** the user reviews and approves. The `pair` skill will
refuse to implement a plan that is not approved.

Do not design architecture or write code here. Technical decisions are taken at
the start of the `pair` phase, with the user.

Story to refine: $ARGUMENTS

## Language

Write your replies and the plan file in the language the user is writing in.

## Rules

1. **Do not assume.** Every ambiguity about behavior (limits, formats, error
   cases, who can do what) becomes a question to the user, not a guess. Unresolved
   questions block approval.
2. **Only the necessary acceptance criteria.** Each criterion must be required to
   deliver the story's benefit or to make the slice safe for production (e.g.
   rejecting invalid input at the boundary, handling a failure the user will
   realistically hit). If a criterion is merely "nice", move it to Out of Scope.
   Heuristic: more than ~5 criteria means the story should be split — propose
   the split and, if accepted, add the new story to the backlog.
3. **Testable criteria.** Write each one as Given / When / Then with concrete,
   observable outcomes (values, messages, status codes, state changes). Each
   criterion should map to one or a few automated tests. No vague words such as
   "fast", "user-friendly" or "robust" without a measurable definition.
4. **Explicit Out of Scope.** List the things a reasonable developer might be
   tempted to build for this story but must not (extra fields, other roles,
   pagination, caching, configurability, generalizations, future stories…).
   This list is what keeps the `pair` phase from over-engineering.
5. **One story per plan.** Never mix stories.

## Steps

1. Read `.tdd/backlog.md`. If it does not exist, tell the user to run the
   `backlog` skill first (or ask if they want to refine a story given directly in
   chat; if so, create the backlog with that single story first).
2. Pick the story by the ID given. If none was given, list stories with status
   `todo` and ask which one. If the story is already `planned`/`in-progress`/
   `done`, say so and ask how to proceed.
3. Read just enough of the codebase to know what already exists that this
   story touches (so criteria do not describe behavior that already exists).
4. Ask your clarifying questions and wait for the answers.
5. Draft the plan in chat (format below). Iterate with the user.
6. Write it to `.tdd/<ID>-plan.md` with `Status: Draft`. Ensure `.gitignore`
   contains `.tdd/` (add it if missing).
7. Ask the user explicitly to **review and approve** the plan. Only when they
   clearly approve:
   - set `Status: Approved` in the plan,
   - set the story's status to `planned` in `.tdd/backlog.md`.
   If they request changes, edit the plan and ask again.
8. Tell the user the next step: `/tdd-ai-navigator:pair @.tdd/<ID>-plan.md`.

## File format — `.tdd/<ID>-plan.md`

```markdown
# <ID> — <short title>

Status: Draft

## Story

As a <role>, I want <action> so that <benefit>.

## Acceptance criteria

- [ ] AC1 — <name>
  - Given <context>
  - When <action>
  - Then <observable outcome>
- [ ] AC2 — <name>
  - Given …
  - When …
  - Then …

## Out of scope

- <thing not to build> — <short reason or the story it belongs to>

## Open questions

- (none)   ← must be empty before approval

## Technical decisions

_Filled in at the start of the `pair` phase, agreed with the user._
```

Keep the plan short. It is a temporary working document that will be deleted
when the story is done; it is not documentation.
