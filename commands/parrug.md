---
description: Generate an English PARRUG prompt for an autonomous orchestrator session. The generated prompt runs internal subagent loops by default; output copy-paste worker prompt packs only when explicitly requested.
argument-hint: "<slug> -- <topic or task brief>"
---

# /parrug - Autonomous Loop Prompt Generator

Read the installed skill SoT first:

```text
SKILL.md
```

Use it as binding behavior. This command generates a new session prompt; it does
not execute the described target task.

## Invocation

```text
/parrug <slug> -- <topic or task brief>
```

Arguments from the user:

```text
$ARGUMENTS
```

Parse all text before `--` as the slug phrase and normalize it to kebab-case.
Parse everything after `--` as the brief. If the slug or brief is missing, ask
for the missing part and stop.

## Boundary While Generating

Follow `SKILL.md` exactly. In particular:

- default to one autonomous orchestrator-session prompt;
- include internal research, planning, execution, review, and refinement
  subagent loops in that generated prompt;
- do not output worker prompt packs unless the user explicitly asks for
  copy-paste worker prompts;
- do not execute the target task in this `/parrug` command.

## Output

Return one English, copy-paste-ready autonomous orchestrator prompt based on:

- `templates/session-prompt.md`;
- `references/foundations.md`;
- the user's slug and brief;
- verified local or current-source facts needed to make the prompt safe.

The generated prompt must include role boundary, scope, non-goals, context to
read, allowed and forbidden actions, internal subagent loop phases, gate-green
criteria, bounded refinement budget, stop rules, Manual-Burden Check, and final
output contract.
