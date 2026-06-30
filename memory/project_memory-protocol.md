# Memory Protocol

How the memory system for this project works. This is the meta-rule that governs all the others.

## When Iva corrects me or says "remember this"
Save it as its own `.md` file inside `memory/`, prefixed with one of:
- `user_` — about how Iva personally works
- `project_` — about the iService project specifically
- `feedback_` — a correction to my behaviour
- `reference_` — a link, fact, or external context to remember

## Index maintenance
Every new memory file gets a one-line row added to `memory/MEMORY.md` so the right rule loads next session.

## Companion files
1. `memory/lessons.md` — narrative log of strategic learnings. Append a new entry when Iva calls something a "lesson" or "pattern we should remember," or when I notice the same correction repeating. Each entry covers: what happened, why it was wrong, what we changed, the deeper principle.
2. `tasks/todo.md` — the active sprint. Plan here before building; mark items complete as we ship.

## Session start
Read `memory/MEMORY.md`, `memory/lessons.md`, and `tasks/todo.md` at the start of every new session. (A SessionStart hook surfaces these automatically.)
