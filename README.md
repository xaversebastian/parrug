# PARRUG

PARRUG is a reusable prompt-engineering skill for autonomous agent sessions.

PARRUG means:

```text
Plan -> Act -> Review -> Refine -> Until Gate-Green
```

The skill generates one copy-paste-ready autonomous orchestrator prompt for a
topic. That generated prompt tells the target session to run internal research,
planning, worker, reviewer, and refinement subagent loops, then report only the
integrated result.

It is not a task runner by itself. The generator should not execute the target
task, contact providers, deploy, commit, or process private data. It writes a
prompt for a new session that can do the work under its own runtime rules.

## What It Produces

By default, `/parrug <slug> -- <brief>` produces one autonomous orchestrator
session prompt.

The generated prompt includes:

- scope and non-goals;
- source-of-truth files to read first;
- research/context subagent phase;
- plan and review-gate phase;
- parallel worker phase where scopes are independent;
- independent review phase;
- bounded refinement loop;
- manual-burden check;
- stop rules for missing worker tooling, secrets, PII, legal/provider/admin
  writes, production/deploy actions, unclear scope, and failing verification;
- final output contract focused on integrated result and evidence.

It outputs worker prompt packs only when the user explicitly asks for copy-paste
worker prompts.

## Files

```text
SKILL.md
commands/parrug.md
templates/session-prompt.md
references/foundations.md
```

## Install

Clone the repository and link or copy it into your agent's skill directory.

Example:

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

```text
/parrug repo-cleanup -- Clean up this repository, remove obsolete files, update stale docs, run checks, and commit if gate-green.
```

If your runtime does not provide real worker/subagent tooling, the generated
prompt should block instead of dumping a manual worker-prompt pack back to the
user.

## License

MIT.
