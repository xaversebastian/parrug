# PARRUG Foundations

This note is the research basis for the `parrug` skill. It is intentionally
model-independent: the pattern should work in Claude, Codex, ChatGPT, local LLMs,
or any tool-using agent harness with equivalent capabilities.

## Verification Status

Last live-verified: 2026-07-07.

All links in the source map opened successfully on the verification date. If a
future PARRUG task depends on current legal, provider, pricing, API, or version
facts, verify those facts again from primary sources before using them.

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

- Treat PARRUG as a work protocol for the assigned task. Prompt output is only a
  handoff mode when explicitly requested.
- Interleave planning, action, observation, and review. Do not make a long plan
  that never reconnects with the environment.
- Use evaluator-optimizer only as a bounded review/refine phase: critique the
  work against concrete criteria, then refine no more than the stated budget.
- Use orchestrator-worker structure when the runtime has real worker or
  subagent tooling and the work naturally splits into independent tracks.
- Convert review lessons into reusable checks. When the same failure mode recurs,
  make it part of the gate-green criteria.
- Keep the final output integrated. Raw worker prompts, raw worker outputs, and
  raw reviewer comments are internal coordination artifacts, not the user-facing
  result.

## Design Principles

1. **Work protocol first**
   PARRUG should normally execute the assigned task through a controlled loop.
   Producing a prompt instead of doing the work is correct only when the user
   explicitly asks for a handoff or restart artifact.

2. **Ground before planning**
   The agent should read source-of-truth files, inspect status, and separate
   repo facts from assumptions before choosing a plan.

3. **One loop, explicit gates**
   "Work until done" is too vague. PARRUG defines completion as gate-green:
   required checks pass, review findings are resolved or explicitly blocked, and
   open admin/legal/provider gates are not misreported as complete.

4. **Review is a phase, not a vibe**
   Review must compare output against the mission, non-goals, constraints, and
   evidence. For high-risk work, use independent review when the runtime
   supports it.

5. **Refine from evidence**
   Refinement should fix concrete findings, failed checks, or unclear gates. It
   should not be an unbounded retry loop.

6. **Model independence**
   Use role, scope, gates, evidence, and output contracts rather than relying on
   a specific model's hidden reasoning style or a single tool's command syntax.

7. **Human effort stays at approval gates**
   The user should approve real decisions and risky actions, not manually run
   the agent's internal orchestration.

## Gate-Green Definition

A PARRUG task is gate-green only when all applicable items are true:

- scope and non-goals were honored;
- every planned step has a matching verification result;
- tests/builds/checks named in the plan ran or are honestly blocked;
- P0/P1 review findings are resolved;
- PII, secrets, legal, billing, provider, deploy, and production gates remain
  closed unless the user explicitly approved that exact action;
- git/workspace status is accounted for;
- final output states touched files, evidence, open gates, and residual risk.

## Anti-Patterns

- Treating a normal work request as "prompt output only".
- Handing the user multiple worker prompts to copy manually when the task should
  be handled by the current agent or real subagents.
- Faking worker execution when no worker tool exists.
- Starting implementation before defining gates.
- Lacking a hard stop for PII, secrets, provider writes, or production
  activation.
- Using "done" without naming tests, evidence, or review state.
- Running unlimited retries without budget or stop conditions.
- Bundling unrelated roles: worker, reviewer, and approver all in one unchecked
  authority.
- Overfitting to one model or one runtime.
