# 專案管理 — Workflow

## Read order at session start

1. **`tasks/lessons.md`** — past corrections + rules for this project. Read first.
2. **`CHANGELOG.md`** — what shipped recently (brief).
3. **`tasks/V*_status.md`** — detailed history of completed versions.
4. **`PRD.md`** — single source of truth for what's next + product spec.
5. **`tasks/V*_todo.md`** — current in-progress work (if a version is being built).
6. **`docs/v*-*.md`** — per-version implementation deep-dives. Read when debugging or tracing how a version evolved.

## File responsibilities

| File / dir | Purpose | Style |
|-----------|---------|-------|
| `PRD.md` | Product spec + roadmap. Forward-looking. | Comprehensive |
| `CHANGELOG.md` | Per-version release notes. One file. | Brief |
| `tasks/lessons.md` | Corrections + rules learned over time. | Append-only |
| `tasks/V{N}_status.md` | Completed version — module-level status. | Detailed |
| `tasks/V{N}_todo.md` | In-progress version — task breakdown. | Detailed |
| `docs/v{N}-*.md` | Architecture / pipeline deep-dives. | Detailed |

## Conventions

- **One CHANGELOG, brief.** Don't split by version.
- **PRD is forward-looking.** Mark shipped versions with one-liner + link out — don't duplicate detail in PRD.
- **Per-version task file, detailed.** Write `V{N}_todo.md` while building; finalize as `V{N}_status.md` on ship.
- **Implementation deep-dives go in `docs/`**, not PRD.
- **Update `tasks/lessons.md` after every correction.** Stop hook blocks if you don't.

## Enforcement

Project-level hooks at `.claude/settings.json`:
- `Stop` → blocks if correction detected this session but `tasks/lessons.md` not updated
- `UserPromptSubmit` → injects rotating rule reminder per turn
- `SessionStart` → loads `tasks/lessons.md` into context

Scripts live at `~/.claude/hooks/` (reusable across projects).
