---
name: parrug
description: Use when a task is non-trivial: multi-step, ambiguous, risky, cross-file, cross-system, or likely to suffer from premature done claims, missing review gates, manual prompt handoffs, unverified work, or unbounded retries. Also use when the user says PARRUG, gate-green, loop, autonom, orchestrator, next block, or larger task. Do not use for trivial one-step answers.
---

# PARRUG - Gate-Green Work Protocol

PARRUG means: **Plan -> Act -> Review -> Refine -> Until Gate-Green**.

This skill is a work protocol for non-trivial tasks. Its default behavior is to
help the current agent complete the assigned task through a bounded loop with
explicit gates, evidence, review, and refinement. It is **not primarily a prompt
generator**.

Prompt or handoff generation is a secondary mode. Use that mode only when the
user explicitly asks for a copy-paste prompt, prompt pack, handoff, restart
prompt, or reusable session prompt.

## Activation

Use PARRUG when the task has any of these traits:

- multiple steps or files;
- unclear scope, current-state drift, or competing options;
- safety, privacy, legal, provider, billing, deployment, or production risk;
- independent subtasks that could use workers or subagents;
- a meaningful chance of false "done" without tests, review, or evidence;
- the user asks for a loop, orchestrator, autonomous run, gate-green result,
  next block, cleanup, migration, audit, refactor, setup, or larger task.

Do not use PARRUG for a trivial one-step answer, a direct fact the agent can
answer without tools, or a tiny mechanical edit with a single obvious check.

## Core Rule

The agent must carry the work through the loop itself when tools and permissions
allow it:

```text
Plan -> Act -> Review -> Refine -> Until Gate-Green
```

Do not turn a normal work request into a stack of prompts for the user to copy
unless the user explicitly requested prompt output.

## First Pass

1. **Ground**
   - Confirm the active workspace and relevant repos.
   - Read source-of-truth files, mentioned files, and local instructions.
   - Inspect current state and unrelated changes.
   - Separate confirmed facts from assumptions.
   - Verify current legal, provider, pricing, API, version, or news-sensitive
     claims from primary sources before relying on them.

2. **Classify**
   - Identify the task type: read-only, docs-only, code-editing, data-moving,
     provider-gated, admin-gated, production-gated, or mixed.
   - Identify human approval gates before acting.
   - Decide whether subagents, parallel workers, tests, or external research are
     useful.

3. **Plan**
   - Write a short plan for non-trivial work.
   - Each step must include `Step -> verify: <concrete check>`.
   - For ambiguous design choices, compare at least two options and select one
     with a one-line rationale.
   - Define gate-green criteria before execution.

## Working With Superpowers

When Superpowers skills are available, use them as process helpers instead of
recreating their workflow:

- Use `superpowers:brainstorming` for unclear goals, design tradeoffs, or
  options that should be explored before acting.
- Use `superpowers:subagent-driven-development` when the plan contains
  independent implementation, research, or review tracks.
- Use `superpowers:test-driven-development` for new behavior where tests can
  define the target before implementation.
- Use `superpowers:systematic-debugging` for failures, regressions, or unknown
  root causes.
- Use `superpowers:verification-before-completion` before claiming the task is
  done.
- Use `superpowers:finishing-a-development-branch` when the user has asked for
  commit, push, PR, or branch completion.

If a named Superpowers skill is not available in the runtime, continue with the
same PARRUG principle using the available tools. Do not pretend that unavailable
subagents or tools ran.

## Act

Execute only the in-scope work. Keep edits surgical and aligned with the local
codebase or document structure.

Use subagents or parallel workers only when the runtime provides real tooling and
the scopes are independent. Worker scopes must have:

- a concrete task;
- allowed actions;
- forbidden actions;
- expected evidence;
- stop rules;
- a reviewable output contract.

Raw worker prompts, raw worker outputs, raw reviewer comments, and internal plan
fragments are not user-facing output.

## Review

Review against the gates, not against vibes.

Minimum review questions:

- Did the work satisfy the mission and non-goals?
- Did each planned step get its stated verification?
- Did tests, builds, linters, scans, or evidence checks run or get honestly
  marked blocked?
- Are secrets, PII, provider/admin writes, legal/billing, deploy, and production
  gates still respected?
- Were unrelated user changes left untouched?
- Would the final answer make the user do the orchestration manually?

Treat P0/P1 findings as blocking. P2 findings require a conscious decision:
fix, defer as out of scope, or block with next action. Do not hide findings under
"done with concerns".

## Refine

Refine only against concrete failed gates, findings, or missing evidence.

Use a bounded retry budget:

- default: two targeted refinement attempts per blocking gate;
- lower budget for risky work where repeated attempts may cause side effects;
- higher budget only when the user explicitly asks and the work is safe.

If the same gate still fails after the budget is exhausted, stop and report
`BLOCKED_WITH_NEXT_ACTIONS`.

## Gate-Green Definition

A task is gate-green only when all applicable items are true:

- scope and non-goals were honored;
- every major step has a matching verification result;
- required tests/builds/checks ran and passed, or are honestly blocked with the
  exact blocker;
- P0/P1 findings are resolved;
- PII, secrets, legal, billing, provider, deploy, and production gates remain
  closed unless the user explicitly approved that exact action;
- git/workspace status is accounted for where relevant;
- final output names touched files, evidence, open gates, and residual risk.

Use `PARTIAL_GATE_GREEN` only when every in-scope gate is green and the only open
gates were explicitly out of scope before work began. Name those open gates.
Otherwise use `BLOCKED_WITH_NEXT_ACTIONS`.

## Stop Rules

Stop and ask or report a blocker when you hit:

- missing permission for PII, secrets, provider/admin writes, legal/billing,
  payment, cancellation, campaign send, production deploy, or public-live claim;
- destructive VCS operations such as force-push, hard reset, branch deletion, or
  worktree deletion without explicit human approval;
- dependency installation or runtime configuration changes without explicit
  human approval;
- unclear ownership of a deletion, migration, or irreversible cleanup;
- ambiguous scope that cannot be resolved from local source-of-truth files;
- missing worker/subagent tooling where autonomous workers are required;
- repeated verification failure after the refinement budget is exhausted;
- evidence that the requested action would modify unrelated user work.

## Human-Visible Output

For normal PARRUG work, do not output:

- individual worker prompts;
- raw worker results;
- raw reviewer comments;
- internal plan fragments;
- long source excerpts.

Return concise integrated output:

1. **Current Decision**
2. **What Was Checked**
3. **Integrated Result**
4. **Evidence**
5. **Open Questions**
6. **Next Action or Blocker**

## Handoff Prompt Mode

Use `templates/session-prompt.md` only when the user explicitly asks for a
prompt, prompt pack, handoff, restart prompt, copy-paste artifact, or reusable
session prompt.

In handoff prompt mode:

- output the prompt in chat by default;
- write a file only on explicit user request;
- include mission, non-goals, sources of truth, allowed actions, forbidden
  actions, gate-green criteria, stop rules, evidence requirements, and bounded
  refinement;
- make the generated prompt run PARRUG in the target session;
- do not claim that the target task is done merely because the prompt was
  produced.

## Research Basis

Use `references/foundations.md` for the design rationale. The protocol draws
from ReAct, Reflexion, Self-Refine, Tree of Thoughts, Anthropic agent workflow
patterns, OpenAI Codex agent-loop notes, and eval-loop guidance.

## Common Mistakes

- Treating PARRUG as "generate prompts only" for normal work requests.
- Handing orchestration work back to the user as multiple manual prompts.
- Faking worker execution when no worker tool exists.
- Starting implementation before defining gates.
- Running unbounded retries.
- Treating provider, legal, billing, PII, or production actions as ordinary
  implementation steps.
- Calling work done without verification evidence.
