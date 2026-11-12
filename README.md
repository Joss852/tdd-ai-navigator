# tdd-ai-navigator

A Claude Code plugin that makes Claude follow a **Vertical Slices + TDD + AI pair
programming** workflow to build production-ready code without over-engineering.

You are the navigator; Claude drives. Three skills, one per phase:

| Phase | Skill | Output |
|-------|-------|--------|
| 1. Backlog (definition) | `/tdd-ai-navigator:backlog <feature>` | `.tdd/backlog.md` — small user stories, each a thin end-to-end slice |
| 2. Story (refinement) | `/tdd-ai-navigator:story US-001` | `.tdd/US-001-plan.md` — minimal acceptance criteria + Out of Scope, reviewed and **approved** by you |
| 3. Pair programming (TDD) | `/tdd-ai-navigator:pair @.tdd/US-001-plan.md` | Code + tests, Red → Green → Refactor, one commit per acceptance criterion |

Claude can also pick the right skill automatically when you ask for something
related (e.g. "break this feature into stories").

## Workflow

```
feature ──/backlog──▶ backlog.md ──/story──▶ US-XXX-plan.md ──(you approve)──▶ /pair
                                                                                │
        ◀── next story ◀── plan deleted, story marked done ◀── all ACs green ◀──┘
```

- **Backlog** — stories in the form *As a [role], I want [action] so that
  [benefit]*, minimum viable scope, ordered by value/risk. Ideas outside the MVP
  are parked, not planned.
- **Story** — only the acceptance criteria needed (Given/When/Then, testable),
  an explicit *Out of scope* list, and no open questions before approval.
- **Pair** — before any code, Claude proposes where the code goes and asks every
  open architecture question. Then, per acceptance criterion:
  **Red** (failing test, for the right reason) → **Green** (minimum code) →
  **Refactor** (no behavior change) → checkpoint with you → commit on your OK.
  Claude stops and proposes a refactor whenever a file exceeds ~200 lines.

### Working files

`.tdd/` holds temporary notes, not documentation. The skills add `.tdd/` to the
project's `.gitignore`; plans are deleted when their story is closed, and the
backlog can be deleted once every story is done. Nothing about stories is
versioned in git — the code and tests are the record.

### Language

The skills are written in English, but Claude replies and writes the backlog and
plans in the language you use (e.g. *Como [rol], quiero [acción] para
[beneficio]*).

### Stack

Stack-agnostic. Claude detects the language and test command from the project;
if there is no test setup or it is ambiguous, it asks you.

## Install

Install it once for your user and it is available in every project:

```bash
claude plugin marketplace add Joss852/tdd-ai-navigator
claude plugin install tdd-ai-navigator@tdd-ai-navigator --scope user
```

Or from inside Claude Code:

```
/plugin marketplace add Joss852/tdd-ai-navigator
/plugin install tdd-ai-navigator@tdd-ai-navigator
```

Update to the latest version with `claude plugin marketplace update tdd-ai-navigator`.

To try it without installing:

```bash
claude --plugin-dir /path/to/tdd-ai-navigator
```

## Structure

```
.claude-plugin/
  plugin.json        # plugin manifest
  marketplace.json   # lets this repo act as its own marketplace
skills/
  backlog/SKILL.md
  story/SKILL.md
  pair/SKILL.md
```

## License

[MIT](LICENSE)
