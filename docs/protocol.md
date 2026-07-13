# PARRUG Deep Protocol

Normative runtime contract: `SKILL.md` (short). This document is the full
specification for power users, reviewers, and maintainers.

## 1. Purpose

PARRUG is a work protocol for non-trivial agent tasks. It interleaves planning,
action, observation, review, and bounded refinement until gates are green or a
blocker is reported honestly.

PARRUG is model- and runtime-neutral. It works with Codex, Claude Code, Cursor,
Windsurf, ChatGPT, or local LLMs — as long as the agent can read files, plan
steps, run checks, and report evidence.

## 2. Activation Rule

Activate when **any** of these apply:

- multiple steps or files;
- unclear scope or state drift;
- safety, privacy, legal, provider, billing, deploy, or production risk;
- independent subtasks suitable for workers;
- meaningful chance of false "done";
- user explicitly requests PARRUG, gate-green, loop, or orchestrator mode.

Do **not** activate for trivial one-step answers or tiny mechanical edits.

## 3. Core Loop

```text
Plan -> Act -> Review -> Refine -> Until Gate-Green
```

### 3.1 Plan

1. Confirm workspace and relevant repos.
2. Read source-of-truth files and inspect current state.
3. Separate confirmed facts from assumptions.
4. Write a plan where each step is `Step N -> verify: <concrete check>`.
5. For ambiguous design choices: compare at least two options, pick one with a
   one-line rationale.
6. Define gate-green criteria **before** execution.

### 3.2 Act

- Execute only in-scope work.
- Keep edits surgical; match local conventions.
- Use workers/subagents only when the runtime provides real tooling and scopes
  are independent.
- Each worker scope needs: task, allowed actions, forbidden actions, expected
  evidence, stop rules, output contract.
- Do not fake worker execution.

### 3.3 Review

Review against gates and evidence, not intuition.

Minimum questions:

- Mission and non-goals satisfied?
- Each planned step verified?
- Checks ran or honestly marked blocked?
- Risk gates (secrets, PII, legal, billing, provider, deploy, production)
  respected?
- Unrelated user changes untouched?
- Would the user need to run orchestration manually?

Apply severity rules from `docs/severity.md`.

### 3.4 Refine

- Refine only against concrete failed gates, findings, or missing evidence.
- Default budget: **two** targeted attempts per blocking gate.
- Lower budget for high side-effect risk; higher only with explicit user
  approval.
- No retry without new evidence or a new hypothesis.
- After budget exhausted: `BLOCKED_WITH_NEXT_ACTIONS`.

## 4. Standard Gates

Every PARRUG run evaluates these gates. A gate is either `pass`, `fail`,
`blocked`, or `not_applicable` (with one-line reason).

| Gate | Purpose | Minimum check | Blocker |
|------|---------|---------------|---------|
| `scope` | Stay within mission | Non-goals not violated | In-scope work skipped or scope widened without approval |
| `grounding` | Know current state | SoT files read; assumptions listed | Acting on unverified assumptions |
| `plan` | Executable steps | Each step has `verify` and a result | Missing verification or skipped steps |
| `execution` | Controlled action | Only allowed actions taken | Forbidden or out-of-scope actions |
| `checks` | Objective quality | Tests/builds/reviews ran or blocked honestly | Failed check hidden or unrun check claimed pass |
| `risk` | Safety boundaries | No secrets/PII/legal/prod violations | Any unapproved high-risk action |
| `review` | Finding resolution | P0/P1 resolved; P2 consciously decided | Open P0/P1 at close |
| `refinement` | Bounded retry | Budget respected | Unbounded or cosmetic retries |
| `handoff` | User burden | Integrated result, no manual orchestration | User must copy prompts or restart workers |

## 5. Severity Scale

Full definitions: `docs/severity.md`.

| Level | Blocks gate-green? | Examples |
|-------|-------------------|----------|
| P0 | Yes, immediately | Secrets exposed, PII leak, safety/correctness failure |
| P1 | Yes | Missing evidence, failed check, scope drift, open risk gate |
| P2 | Only if consciously deferred wrong | Style nit, nice-to-have, out-of-scope improvement |

## 6. Evidence Format

Mandatory at the end of every PARRUG run:

```text
Evidence:
- sources: <files/URLs/sections>
- actions: <read | changed | reviewed>
- checks: <check -> pass|fail|blocked + short reason>
- findings: <P0/P1/P2 + status>
- open_gates: <none | list>
- result: GATE_GREEN | PARTIAL_GATE_GREEN | BLOCKED_WITH_NEXT_ACTIONS
```

Rules:

- Every `checks` entry must name the check and its outcome.
- `blocked` requires the exact blocker (permission, tool, env, approval).
- `findings` must map to severity and resolution status.
- Research tasks may use qualitative checks (source quality, recency) but the
  `checks` field remains mandatory.

## 7. Result Status

### GATE_GREEN

All applicable gates are green. Evidence is complete.

### PARTIAL_GATE_GREEN

Every **in-scope** gate is green. The only open gates were explicitly declared
out-of-scope **before** work began. Name each open gate.

### BLOCKED_WITH_NEXT_ACTIONS

Any other state: failed gate, missing evidence, exhausted refinement budget,
or newly discovered in-scope blocker.

## 8. Stop Rules

Stop and report blocker when encountering:

- missing permission for PII, secrets, provider/admin writes, legal action,
  live billing, money movement, production deploy, or public-live claim;
- history-rewriting or destructive VCS operations without explicit approval;
  contained branch/worktree cleanup is allowed after integration proof;
- global toolchain/provider/runtime changes without explicit approval; a
  minimal justified repo dependency is allowed when manifest, lockfile and
  relevant checks are included;
- unclear ownership of deletion, migration, or irreversible cleanup;
- ambiguous scope unresolvable from local sources;
- missing worker tooling where autonomous workers are required;
- verification failure after refinement budget exhausted;
- evidence that work would modify unrelated user changes.

## 9. Output Contract

Do not expose raw worker prompts, raw worker outputs, raw reviewer comments, or
internal plan fragments.

Return:

1. **Current Decision**
2. **What Was Checked**
3. **Integrated Result**
4. **Evidence**
5. **Open Questions**
6. **Next Action or Blocker**

## 10. Handoff Prompt Mode

Secondary mode only. Activate when the user explicitly asks for a copy-paste
prompt, handoff, restart prompt, or reusable session prompt.

Use `templates/session-prompt.md`. Output in chat by default; write a file only
on explicit request. Producing a prompt does not mean the target task is done.

## 11. Task Templates

| Type | Template | When |
|------|----------|------|
| Code | `templates/code-task.md` | Code changes, tests, builds |
| Docs | `templates/doc-task.md` | Documentation, index updates |
| Research | `templates/research-task.md` | Investigation, comparison, audit |
| Handoff | `templates/session-prompt.md` | Cross-session restart |

## 12. Optional Adapters

Runtime-specific install and process-skill hints: `docs/adapters.md`.
Adapters are optional; the protocol works without them.

## 13. Research Basis

Design rationale and source map: `references/foundations.md`.
