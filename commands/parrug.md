---
description: Apply the PARRUG gate-green work protocol to a non-trivial task. Produces a handoff or restart prompt only when explicitly requested.
argument-hint: "<task brief>"
---

# /parrug - Gate-Green Work Protocol

Read the installed skill SoT first:

```text
SKILL.md
```

Use it as binding behavior.

## Invocation

```text
/parrug <task brief>
```

Arguments from the user:

```text
$ARGUMENTS
```

Treat the arguments as the task brief unless the user explicitly asks for a
prompt, prompt pack, handoff, restart prompt, or copy-paste artifact.

## Default Behavior

Run the current task through PARRUG:

```text
Plan -> Act -> Review -> Refine -> Until Gate-Green
```

Use available Superpowers skills and real worker/subagent tooling when they fit
the task. Do not expose raw worker prompts or raw reviewer output. If the
runtime lacks a required worker/subagent capability, report the tooling blocker
instead of pretending workers ran.

## Handoff Prompt Behavior

Only when explicitly requested, produce a copy-paste-ready English handoff prompt
based on:

- `templates/session-prompt.md`;
- `references/foundations.md`;
- the user's task brief;
- verified local or current-source facts needed to make the prompt safe.

In that mode, output the prompt in chat by default and do not execute the target
task unless the user separately asks for execution.

## Output

For normal work, return the integrated result only:

1. **Current Decision**
2. **What Was Checked**
3. **Integrated Result**
4. **Evidence**
5. **Open Questions**
6. **Next Action or Blocker**
