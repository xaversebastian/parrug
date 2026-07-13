---
name: parrug
description: >-
  Use when a task is non-trivial (multi-step, ambiguous, risky, cross-file,
  cross-system, or likely to suffer from premature done claims). Also use when
  the user says PARRUG, gate-green, loop, orchestrator, or larger task. Do not
  use for trivial one-step answers.
---

# PARRUG — Gate-Green Work Protocol

PARRUG means: **Plan -> Act -> Review -> Refine -> Until Gate-Green**.

Default behavior: complete the assigned task through a bounded loop with
explicit gates, evidence, review, and refinement. PARRUG is **not** primarily a
prompt generator. Handoff/prompt output only when the user explicitly asks for
a copy-paste prompt, handoff, restart prompt, or reusable session prompt.

Full specification: `docs/protocol.md`. Severity scale: `docs/severity.md`.

## Activation

Use PARRUG when the task has any of these traits:

- multiple steps or files;
- unclear scope, drift, or competing options;
- safety, privacy, legal, provider, billing, deployment, or production risk;
- independent subtasks that could use workers or subagents;
- meaningful chance of false "done" without tests, review, or evidence;
- user asks for loop, orchestrator, gate-green, cleanup, migration, audit,
  refactor, or larger task.

Do **not** use for a trivial one-step answer, a direct fact without tools, or a
tiny mechanical edit with one obvious check.

## Core Loop

```text
Plan -> Act -> Review -> Refine -> Until Gate-Green
```

Carry the work through the loop yourself when tools and permissions allow. Do
not hand orchestration back to the user unless they explicitly requested a
handoff prompt.

### 1. Plan

- Ground in workspace, source-of-truth files, and current state.
- Separate confirmed facts from assumptions.
- Write a short plan; each step: `Step N -> verify: <concrete check>`.
- Define gate-green criteria before execution.

### 2. Act

- Execute only in-scope work; keep edits surgical.
- Use workers/subagents only when the runtime provides real tooling and scopes
  are independent.
- Do not fake worker execution.

### 3. Review

Review against gates, not vibes. See `docs/protocol.md` for the standard gate set.

Severity rules (`docs/severity.md`):

- **P0** — blocks immediately (safety, correctness, secrets, PII).
- **P1** — blocks gate-green (missing evidence, failed checks, scope drift).
- **P2** — conscious decision: fix, defer as out-of-scope, or block with next
  action.

### 4. Refine

- Refine only concrete failed gates, findings, or missing evidence.
- Default budget: **two** targeted attempts per blocking gate.
- No retry without new evidence or a new hypothesis.
- After budget exhausted: `BLOCKED_WITH_NEXT_ACTIONS`.
- Higher budget only with explicit user approval and low side-effect risk.

## Standard Gates

| Gate | Question |
|------|----------|
| `scope` | Mission and non-goals honored? |
| `grounding` | Sources read, assumptions marked? |
| `plan` | Each step has `verify` and a result? |
| `execution` | Only allowed, in-scope actions taken? |
| `checks` | Tests/builds/reviews ran or honestly blocked? |
| `risk` | Secrets, PII, legal, billing, provider, deploy, production untouched? |
| `review` | P0/P1 resolved; P2 consciously decided? |
| `refinement` | Retry budget respected? |
| `handoff` | User does not need to run internal orchestration manually? |

A gate may be `not_applicable` only with a one-line reason in evidence.

## Evidence (required)

Every PARRUG run ends with an evidence block:

```text
Evidence:
- sources: <files/URLs/sections read>
- actions: <read | changed | reviewed>
- checks: <check -> pass|fail|blocked + short reason>
- findings: <P0/P1/P2 + status>
- open_gates: <none | list>
- result: GATE_GREEN | PARTIAL_GATE_GREEN | BLOCKED_WITH_NEXT_ACTIONS
```

## Result Status

- **GATE_GREEN** — all applicable gates green.
- **PARTIAL_GATE_GREEN** — every in-scope gate green; only gates explicitly
  declared out-of-scope before work remain open. Name them.
- **BLOCKED_WITH_NEXT_ACTIONS** — any other state.

## Stop Rules

Stop and report blocker when you hit:

- missing permission for PII, secrets, provider/admin writes, legal action,
  live billing, money movement, production deploy, or public-live claim;
- history-rewriting or destructive VCS ops without explicit approval; contained
  branch/worktree cleanup is allowed after integration proof;
- global toolchain/provider/runtime changes without explicit approval; a
  minimal justified repo dependency is allowed when manifest, lockfile and
  relevant checks are included;
- unclear ownership of deletion, migration, or irreversible cleanup;
- ambiguous scope unresolvable from local sources;
- verification failure after refinement budget exhausted;
- evidence that work would touch unrelated user changes.

## Output Contract

Do not expose raw worker prompts, raw worker outputs, raw reviewer comments, or
internal plan fragments.

Return integrated output:

1. **Current Decision**
2. **What Was Checked**
3. **Integrated Result**
4. **Evidence**
5. **Open Questions**
6. **Next Action or Blocker**

## Task Templates

| Task type | Template |
|-----------|----------|
| Code change | `templates/code-task.md` |
| Documentation | `templates/doc-task.md` |
| Research | `templates/research-task.md` |
| Handoff prompt | `templates/session-prompt.md` |

## Optional Adapters

Runtime-specific install and slash-command setup: `docs/adapters.md`.
Optional process-skill hints (Superpowers etc.): `docs/adapters.md#process-skills`.

## Research Basis

Design rationale: `references/foundations.md`.
