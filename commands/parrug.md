---
description: Apply the PARRUG gate-green work protocol to a non-trivial task.
argument-hint: "<task brief>"
---

# /parrug

Read and follow the installed skill SoT:

```text
SKILL.md
```

Deep spec (if needed): `docs/protocol.md`

## Arguments

```text
$ARGUMENTS
```

Treat as the task brief unless the user explicitly asks for a handoff or
restart prompt.

## Behavior

1. Run `Plan -> Act -> Review -> Refine -> Until Gate-Green`.
2. Close with mandatory evidence block (see `SKILL.md`).
3. Use task templates from `templates/` when they fit.
4. Optional runtime setup: `docs/adapters.md`.

Handoff prompt mode: only on explicit request → `templates/session-prompt.md`.

## Output

Integrated result only (no raw worker output):

1. Current Decision
2. What Was Checked
3. Integrated Result
4. Evidence
5. Open Questions
6. Next Action or Blocker
