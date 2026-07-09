# PARRUG Foundations

Research basis and design rationale for the `parrug` skill. **Not** a second
normative spec — gates, evidence, and severity live in `SKILL.md` and
`docs/protocol.md`.

## Verification Status

Last live-verified: 2026-07-07.

If a task depends on current legal, provider, pricing, API, or version facts,
verify from primary sources before relying on them.

## Source Map

| Source | Link | PARRUG lesson |
|---|---|---|
| ReAct | https://arxiv.org/abs/2210.03629 | Interleave reasoning and action with observations. |
| Reflexion | https://arxiv.org/abs/2303.11366 | Convert feedback into explicit reflection for the next attempt. |
| Self-Refine | https://arxiv.org/abs/2303.17651 | Separate generate, critique, and refine phases. |
| Tree of Thoughts | https://arxiv.org/abs/2305.10601 | Deliberate over alternatives before committing. |
| Anthropic: Building Effective Agents | https://www.anthropic.com/engineering/building-effective-agents | Simple workflows first; explicit stopping conditions. |
| OpenAI: Codex agent loop | https://openai.com/index/unrolling-the-codex-agent-loop/ | Loops need defined termination. |
| OpenAI: Agent Improvement Loop | https://developers.openai.com/cookbook/examples/agents_sdk/agent_improvement_loop | Traces and evals make improvement reusable. |
| OpenAI: Working with evals | https://developers.openai.com/api/docs/guides/evals | Explicit criteria, then iterate on results. |

## Design Principles

1. **Work protocol first** — execute the task; prompt output only for explicit
   handoff requests.
2. **Ground before planning** — read SoT files and inspect state first.
3. **One loop, explicit gates** — completion means gate-green, not "feels done".
4. **Review is a phase** — compare against mission, constraints, and evidence.
5. **Refine from evidence** — bounded retries on concrete failures only.
6. **Model independence** — roles, gates, and contracts over tool syntax.
7. **Human effort at approval gates** — user approves risk; agent runs the loop.

## Anti-Patterns

- Treating normal work as "prompt output only".
- Handing the user multiple worker prompts to copy manually.
- Faking worker execution when no worker tool exists.
- Starting implementation before defining gates.
- Claiming "done" without evidence.
- Unbounded retries without budget.
- Overfitting to one model or runtime.

## Where Normative Rules Live

| Topic | File |
|-------|------|
| Runtime contract | `SKILL.md` |
| Full protocol | `docs/protocol.md` |
| P0/P1/P2 | `docs/severity.md` |
| Metrics | `docs/metrics.md` |
| Install adapters | `docs/adapters.md` |
