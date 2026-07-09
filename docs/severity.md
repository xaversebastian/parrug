# Severity Scale (P0 / P1 / P2)

Normative definitions for PARRUG review findings. Referenced by `SKILL.md` and
`docs/protocol.md`.

## P0 — Critical (blocks immediately)

A finding that must stop work now. Never defer to gate-green close.

**Criteria (any one triggers P0):**

- exposed or committed secrets, tokens, credentials;
- customer PII in repo, logs, or output;
- safety or correctness failure with real harm potential;
- unapproved production deploy, billing, payment, or legal action;
- destructive VCS operation executed without explicit approval.

**Resolution:** fix before close, or stop with `BLOCKED_WITH_NEXT_ACTIONS`.

## P1 — Blocking (prevents gate-green)

A finding that blocks gate-green but is not an immediate safety incident.

**Criteria (examples):**

- required test, build, lint, or scan failed;
- planned step missing its `verify` result;
- scope drift (in-scope work skipped or widened);
- risk gate touched without approval;
- evidence block incomplete or inconsistent;
- open finding that affects correctness or deliverable quality.

**Resolution:** fix, or stop with `BLOCKED_WITH_NEXT_ACTIONS` and name the
blocker.

## P2 — Minor (conscious decision required)

A finding that does not automatically block gate-green.

**Criteria (examples):**

- style or formatting outside task scope;
- nice-to-have improvement not in mission;
- low-risk documentation nit;
- deferred optimization with explicit out-of-scope declaration.

**Resolution (pick one, document in evidence):**

- `fixed` — addressed in this run;
- `deferred_out_of_scope` — explicitly not part of mission;
- `blocked_next_action` — should be fixed but needs separate approval or scope.

Do not hide P2 concerns under "done with minor issues" without naming the
decision.

## Mapping to Gates

| Severity | Typical gate |
|----------|-------------|
| P0 | `risk`, `checks`, `review` |
| P1 | `scope`, `plan`, `checks`, `review`, `evidence` |
| P2 | `review` (conscious defer/fix decision) |

## Evidence Format

```text
findings:
- P0: <description> -> fixed|blocked
- P1: <description> -> fixed|open
- P2: <description> -> fixed|deferred_out_of_scope|blocked_next_action
```
