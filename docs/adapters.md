# Runtime Adapters (Optional)

PARRUG is tool-neutral. This file covers **optional** install paths. The
protocol itself requires only: read files, plan steps, run checks, report
evidence.

## Codex

```bash
git clone https://github.com/xaversebastian/parrug.git ~/.codex/skills/parrug
```

Skill is picked up from `SKILL.md` in the skill directory.

## Claude Code

```bash
git clone https://github.com/xaversebastian/parrug.git ~/.claude/skills/parrug
```

Slash command (optional):

```bash
ln -sf "$HOME/.claude/skills/parrug/commands/parrug.md" \
  "$HOME/.claude/commands/parrug.md"
```

## Cursor

Copy or symlink the repo into your project's skills/rules area, or reference
`SKILL.md` from `.cursor/rules/` with a short pointer rule. No Cursor-specific
runtime feature is required.

Example project rule pointer:

```markdown
For non-trivial tasks, follow the PARRUG protocol in skills/parrug/SKILL.md.
```

## Windsurf

Same as Cursor: place `SKILL.md` where your Windsurf workflow reads agent
instructions. No Windsurf-specific API required.

## Process Skills

If your runtime has optional process skills (e.g. Superpowers), use them as
helpers — not as prerequisites:

| Situation | Optional helper |
|-----------|----------------|
| Unclear goals or design tradeoffs | brainstorming |
| Independent parallel tracks | subagent-driven development |
| New testable behavior | test-driven development |
| Failures or unknown root cause | systematic debugging |
| Before claiming done | verification before completion |
| Commit / push / PR work | finishing a development branch |

If a named skill is unavailable, continue with the same PARRUG loop using
available tools. Do not pretend unavailable skills ran.
