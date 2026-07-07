---
name: parrug
description: Use when a user asks for a PARRUG loop-engineering prompt, autonomous orchestrator session prompt, plan-act-review-refine-until-gate-green workflow, or subagent-driven execution prompt. Generates prompts; the default generated prompt runs autonomous internal subagent loops. Only output worker prompt packs when the user explicitly asks for copy-paste worker prompts. Not for directly executing the target task.
---

# PARRUG - Autonomous Loop Prompt Engineering

PARRUG means: **Plan -> Act -> Review -> Refine -> Until Gate-Green**.

This skill generates copy-paste-ready prompts for new sessions. It does not execute the target task itself. By default it generates **one autonomous
orchestrator-session prompt** for the current topic. That generated prompt must
tell the target session to dispatch research, planning, worker, reviewer, and
refinement subagents internally, then show the human only the integrated result.

## Default Behavior

Default to one autonomous orchestrator prompt per topic.

Do not output a bundle of worker prompts unless the user explicitly asks for
copy-paste prompts, for example:

- "erstelle Prompt-Pack";
- "gib mir Worker-Prompts";
- "ich will die Prompts copy-pasten";
- "prompt-only";
- "nur die Prompts ausgeben".

When the user says "Loop", "autonom", "Worker laufen lassen", "Orchestrator",
"Phase ausfuehren", "naechster Block", "PARRUG-first", or describes a large
task that should be handled end-to-end, generate one autonomous loop prompt, not
a manual prompt pack.

## Generator Boundary

`/parrug` is a prompt generator, not the target worker.

Allowed while generating the prompt:

- read local files needed to understand the target context;
- inspect workspace status with read-only commands such as `git status` and
  `git log`;
- search current primary sources when the prompt depends on unstable facts;
- ask one concise clarifying question if the brief is too ambiguous;
- synthesize the requested prompt artifact.

Forbidden while generating the prompt:

- execute the described target task;
- start target workers or subagents;
- edit, commit, push, deploy, migrate, import, send, cancel, purchase, buy, or
  contact providers;
- run builds, tests, or state-changing shell commands while grounding;
- install dependencies or change runtime configuration;
- process PII/secrets beyond naming the stop rule;
- claim the described target work is complete.

## Generated Prompt Contract

The default generated prompt must create an autonomous orchestrator session with
these properties:

- it checks whether the target runtime has real subagent, worker, or multi-agent
  tooling before claiming worker execution;
- it dispatches research/context subagents when useful for current facts,
  architecture, safety, legal/provider, or repo understanding;
- it turns the research into a plan and explicit review gates;
- it dispatches execution workers in parallel when their scopes are independent;
- it dispatches reviewer passes against the gates;
- it refines failed gates with a bounded retry budget chosen for the topic;
- it stops when gates are green, partially green with named out-of-scope gates,
  or blocked with exact next actions;
- it keeps raw worker prompts, raw worker outputs, raw reviewer comments, and
  internal plan fragments out of the human-visible final answer;
- it returns only integrated outcome, evidence, open decisions, blockers, and
  the next autonomous loop prompt or handoff.

If the target runtime does not provide real subagent/worker tooling, the
generated prompt must not fall back to dumping worker prompts. It should report
the tooling blocker and, at most, give one restart prompt for a runtime that can
run the loop.

## Manual-Burden Check

Every generated prompt must force the target session to run this check before
final output:

```text
Does my output require the user to copy multiple prompts, start workers
manually, or do the orchestration work themselves?
```

If yes, the target output is wrong unless the user explicitly asked for a
copy-paste prompt pack. The target session must either integrate the loop result
or report the tooling blocker.

## Inputs

Expected invocation:

```text
/parrug <slug> -- <topic or task brief>
```

Use the slug only as a short output label. The brief after `--` is the real
scope. If multiple tokens appear before `--`, join them as a kebab-case slug. If
either part is missing, ask for the missing part and stop.

## Prompt Construction Loop

1. **Ground**
   - Identify the target workspace, role, risk class, and likely gates.
   - Read mentioned files or obvious local source-of-truth files.
   - Use web/current-source checks for legal, provider, pricing, API, version,
     or news-dependent claims.

2. **Classify**
   - Decide whether the target prompt is read-only, docs-only, code-editing,
     provider-gated, admin-gated, production-gated, or mixed.
   - Choose the default target role: autonomous orchestrator.
   - Choose another target role only if the user explicitly asked for it.

3. **Draft**
   - Use `templates/session-prompt.md`.
   - Fill every required slot, especially `{{non_goals}}`, `{{sot_files}}`,
     `{{subagent_loop}}`, `{{gate_green_criteria}}`, and
     `{{refinement_budget}}`.
   - Make every major step verifiable.
   - For ambiguous design choices, include at least two options and choose one
     with a one-line rationale.
   - Define a topic-appropriate bounded retry budget. Use two refinement rounds
     per blocking gate as the default unless the task risk requires less.

4. **Review**
   - Check the generated prompt against the Gate-Green Checklist below.
   - If a gate is vague, rewrite it as concrete evidence or an explicit stop.
   - If the prompt would make the user manually run multiple workers, rewrite it
     as an autonomous orchestrator prompt.

5. **Output**
   - Return only the final English prompt in chat, plus short usage notes if
     needed.
   - Do not write prompt files unless the user explicitly asks for a file.
   - Do not include hidden reasoning or long source excerpts.

## Gate-Green Checklist

A generated prompt is gate-green only if it states:

- autonomous orchestrator role by default;
- target mission and non-goals;
- source-of-truth files and workspace/context-gathering requirements;
- allowed and forbidden actions;
- research/context subagent phase;
- plan and review-gate creation phase;
- parallel worker execution phase where scopes are independent;
- independent review phase against explicit gates;
- bounded refine loop with a topic-appropriate retry budget;
- Manual-Burden Check;
- stop rules for missing worker tooling, PII, secrets, legal/provider/admin
  writes, production/deploy actions, unclear scope, and failing verification;
- human-visible final output limited to integrated result, evidence, open
  questions, blockers, and next autonomous loop prompt or handoff.

## Research Basis

Use `references/foundations.md` for the design rationale. The core pattern draws
from ReAct, Reflexion, Self-Refine, Tree of Thoughts, Anthropic agent workflow
patterns, OpenAI Codex agent-loop notes, and eval-loop guidance.
If a generated prompt depends on facts that may have changed since the
foundation note was verified, refresh those facts from primary sources and mark
unverified source-dependent claims as open gates instead of treating them as
settled.

## Runtime Notes

- Claude: the real slash command is `commands/parrug.md`, symlinked globally as
  `~/.claude/commands/parrug.md`.
- Codex: use the global `parrug` skill. Treat `/parrug ...` as the prompt
  pattern, not as guaranteed native slash-command support.
- The generated prompt should require real worker/subagent tooling in the target
  runtime. If unavailable, the target session blocks instead of handing manual
  worker prompts back to the user.

## Acceptance Test

If the user says:

```text
arbeite PARRUG-first / loops sollen autonom laufen
```

the generated prompt must not instruct the target session to output five worker
prompts. It must instruct the target session to run an integrated autonomous
subagent loop, or to block if no worker/subagent tooling exists.

## Common Mistakes

- Turning a large autonomous-loop request into a manual worker prompt pack.
- Letting the target session expose raw worker prompts or raw reviewer output.
- Faking worker execution when no worker tool exists.
- Generating a prompt that says "continue until done" without gate definitions.
- Using one fixed retry budget for every topic instead of setting a bounded
  topic-appropriate budget.
- Treating provider, legal, billing, PII, or production actions as normal steps.
- Hiding review findings under "done with concerns".
