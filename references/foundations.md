# PARRUG Foundations

This note is the research basis for the `parrug` skill. It is intentionally
model-independent: the pattern should work in Claude, Codex, ChatGPT, local LLMs,
or any tool-using agent harness with equivalent capabilities.

## Verification Status

Last live-verified: 2026-07-07.

All links in the source map opened successfully on the verification date. If a
future `/parrug` prompt depends on current legal, provider, pricing, API, or
version facts, verify those facts again from primary sources before using them.

## Source Map

| Source | Link | PARRUG lesson |
|---|---|---|
| ReAct: Synergizing Reasoning and Acting in Language Models | https://arxiv.org/abs/2210.03629 | Reasoning and actions should be interleaved with observations, not split into blind planning and blind execution. |
| Reflexion: Language Agents with Verbal Reinforcement Learning | https://arxiv.org/abs/2303.11366 | Feedback can be converted into explicit reflection that improves the next attempt without retraining. |
| Self-Refine: Iterative Refinement with Self-Feedback | https://arxiv.org/abs/2303.17651 | Generate, critique, and refine as separate phases so quality improves at test time. |
| Tree of Thoughts | https://arxiv.org/abs/2305.10601 | For ambiguous tasks, deliberate over alternatives and self-evaluate before committing. |
| Anthropic: Building Effective Agents | https://www.anthropic.com/engineering/building-effective-agents | Use simple workflows first, evaluator-optimizer and orchestrator-worker patterns where useful, and agents only where step count is not predictable. Ground each step in environment feedback and include stopping conditions. |
| OpenAI: Unrolling the Codex agent loop | https://openai.com/index/unrolling-the-codex-agent-loop/ | Agent loops repeatedly combine model inference, tool actions, observations, context management, and termination. A prompt must define when the loop stops. |
| OpenAI Cookbook: Agent Improvement Loop | https://developers.openai.com/cookbook/examples/agents_sdk/agent_improvement_loop | Traces, feedback, evals, and handoffs make improvements reusable instead of one-off prompt tweaking. |
| OpenAI: Working with evals | https://developers.openai.com/api/docs/guides/evals | Reliable agent behavior needs explicit tests or criteria, then iteration based on observed results. |

## Applying The Sources

- Treat PARRUG itself as a predictable prompt-generation workflow: gather
  context, draft, review, and output one prompt. Do not let the generator become
  the target worker.
- Use agent-style flexibility inside the generated target prompt: internal
  subagents may research, execute, review, and refine, but only with explicit
  gates, stop rules, and final evidence requirements.
- Use evaluator-optimizer only as a bounded review/refine phase: critique the
  prompt or work against concrete criteria, then refine no more than the stated
  budget.
- Use orchestrator-worker structure when the target role is `orchestrator`: the
  generated prompt must define dispatch, worker output contracts, and review
  aggregation rather than asking the orchestrator to do worker tasks inline.
- Convert review lessons into reusable checks. When the same failure mode recurs,
  make it part of the generated prompt's gate-green criteria.

## Design Principles

1. **Prompt generator, autonomous target prompt**
   The generator can research the target domain, but it must not perform the
   target task. By default, it should produce one autonomous orchestrator prompt
   that runs subagents internally. This keeps prompt construction separate from
   target execution and prevents accidental provider writes, commits, deploys,
   or PII handling in the generator session.

2. **Ground before planning**
   The target prompt should require a local grounding pass: read source-of-truth
   files, inspect status, and separate repo facts from assumptions.

3. **One loop, explicit gates**
   "Work until done" is too vague. PARRUG defines completion as gate-green:
   required checks pass, review findings are resolved or explicitly blocked, and
   open admin/legal/provider gates are not misreported as complete.

4. **Review is a phase, not a vibe**
   Review must compare output against the mission, non-goals, constraints, and
   evidence. For high-risk work, the generated prompt should require independent
   review when the runtime supports it.

5. **Refine from evidence**
   Refinement should fix concrete findings, failed checks, or unclear gates. It
   should not be an unbounded retry loop.

6. **Model independence**
   Use role, scope, gates, evidence, and output contracts rather than relying on
   a specific model's hidden reasoning style or a single tool's command syntax.

## Gate-Green Definition

A target session is gate-green only when all applicable items are true:

- scope and non-goals were honored;
- every planned step has a matching verification result;
- tests/builds/checks named in the prompt ran or are honestly blocked;
- P0/P1 review findings are resolved;
- PII, secrets, legal, billing, provider, deploy, and production gates remain
  closed unless the prompt explicitly includes a human approval format;
- git/workspace status is accounted for;
- final output states touched files, evidence, open gates, and residual risk.

## Anti-Patterns

- Prompt asks the current `/parrug` generator session to implement the target
  task.
- Generated prompt hands the user multiple worker prompts to copy manually when
  they asked for an autonomous loop.
- Prompt lacks a hard stop for PII, secrets, provider writes, or production
  activation.
- Prompt uses "done" without naming tests, evidence, or review state.
- Prompt asks for unlimited retries without budget or stop conditions.
- Prompt bundles unrelated roles: worker, reviewer, and approver all in one
  unchecked authority.
- Prompt overfits to one model or one runtime and cannot be pasted elsewhere.
