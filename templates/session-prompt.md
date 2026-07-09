# PARRUG Handoff Prompt

Use only when the user explicitly requests a copy-paste handoff or restart
prompt. The target session executes through PARRUG; producing this prompt does
not complete the task.

Normative rules: `SKILL.md`, `docs/protocol.md`, `docs/severity.md`.

## Mission

{{mission}}

## Non-Goals

{{non_goals}}

## Sources of Truth

{{sot_files}}

## Gate-Green Criteria

Supplied by the handoff author. You may add stricter gates after grounding; do
not weaken these.

{{gate_green_criteria}}

## Task Type Template

Pick one and fill placeholders:

- Code: `templates/code-task.md`
- Docs: `templates/doc-task.md`
- Research: `templates/research-task.md`

## Allowed Actions

{{allowed_actions}}

## Forbidden Actions

{{forbidden_actions}}

## Refinement Budget

{{refinement_budget}}

Default: two targeted attempts per blocking gate. Stop with
`BLOCKED_WITH_NEXT_ACTIONS` when exhausted.

## Loop Summary

```text
Plan -> Act -> Review -> Refine -> Until Gate-Green
```

- Plan: each step `Step N -> verify: <check>`
- Review: nine standard gates; severity per `docs/severity.md`
- Close: mandatory evidence block per `SKILL.md`

## Stop Rules

Stop on missing permissions, destructive VCS without approval, unclear scope,
missing required worker tooling, or verification failure after refinement budget.

## Output Contract

Return integrated output only — no raw worker prompts or internal plan fragments:

1. Current Decision
2. What Was Checked
3. Integrated Result
4. Evidence
5. Open Questions
6. Next Action or Blocker

Result: `GATE_GREEN`, `PARTIAL_GATE_GREEN`, or `BLOCKED_WITH_NEXT_ACTIONS`.
