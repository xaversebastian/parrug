# PARRUG

PARRUG is a reusable gate-green work protocol for agentic coding and research
sessions.

PARRUG means:

```text
Plan -> Act -> Review -> Refine -> Until Gate-Green
```

The skill is meant for non-trivial tasks: multi-step work, ambiguous cleanup,
cross-file changes, provider or privacy risk, migrations, audits, refactors,
or any task where a premature "done" claim would be expensive.

It is not primarily a prompt generator. The default behavior is that the current
agent uses PARRUG to do the assigned work with explicit gates, review, evidence,
and bounded refinement. Prompt or handoff generation is only for cases where the
user explicitly asks for a copy-paste prompt, prompt pack, handoff, restart
prompt, or reusable session prompt.

## What It Does

For normal work, PARRUG guides the agent to:

- ground in the real workspace and source-of-truth files;
- classify the risk and approval gates before acting;
- create a plan with concrete verification for each major step;
- use real worker/subagent tooling when available and useful;
- review the result against explicit gates;
- refine only concrete failed gates with a bounded retry budget;
- stop as `GATE_GREEN`, `PARTIAL_GATE_GREEN`, or
  `BLOCKED_WITH_NEXT_ACTIONS`;
- return an integrated result with evidence instead of raw worker chatter.

When Superpowers-style process skills are installed, PARRUG is designed to work
with them:

- brainstorming for unclear tradeoffs;
- subagent-driven development for independent tracks;
- test-driven development for new testable behavior;
- systematic debugging for failures;
- verification before completion before claiming done;
- finishing a development branch for commit, push, or PR work.

If those skills are not available, PARRUG still applies the same loop with the
runtime's available tools.

## Files

```text
SKILL.md
commands/parrug.md
templates/session-prompt.md
references/foundations.md
```

## Install

Clone the repository into your agent's skill directory:

```bash
git clone https://github.com/xaversebastian/parrug.git ~/.codex/skills/parrug
```

For Claude slash-command support, link the command file into your commands
directory:

```bash
ln -sf "$HOME/.codex/skills/parrug/commands/parrug.md" "$HOME/.claude/commands/parrug.md"
```

Adjust paths for your runtime.

## Usage

Use the skill for larger tasks:

```text
/parrug Clean up this repository, remove obsolete files, update stale docs, run checks, and commit if gate-green.
```

Ask for a handoff prompt only when that is what you want:

```text
/parrug Create a restart prompt for a new session that will perform the repository cleanup through PARRUG.
```

## Output Shape

For normal work, the final answer should be integrated and concise:

1. **Current Decision**
2. **What Was Checked**
3. **Integrated Result**
4. **Evidence**
5. **Open Questions**
6. **Next Action or Blocker**

## License

MIT.
