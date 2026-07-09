# PARRUG Metrics

Simple, manual metrics for adoption quality. No special runtime required.

## Definitions

### Adoption Rate

Share of **eligible** non-trivial tasks where PARRUG was correctly activated.

```text
adoption_rate = parrug_activated_eligible / eligible_non_trivial_tasks
```

Eligible = multi-step, risky, cross-file, audit, refactor, migration, or any
task where premature "done" would be expensive.

Not counted: trivial one-step answers the agent correctly skipped.

### Completion Rate

Share of started PARRUG runs that close cleanly.

```text
completion_rate = clean_closes / parrug_runs_started
```

Clean close = `GATE_GREEN` or honest `BLOCKED_WITH_NEXT_ACTIONS` with complete
evidence and named next action.

`PARTIAL_GATE_GREEN` counts as clean only when out-of-scope gates were
pre-declared.

### Rework Rate

Share of closed runs reopened because of protocol failure.

```text
rework_rate = reopened_runs / closed_runs
```

Reopen reasons (examples):

- missing or inconsistent evidence;
- false gate-green (checks not actually run);
- user had to redo orchestration manually;
- severity misclassification (P1 treated as P2);
- drift between SKILL and docs caused wrong behavior.

Target: rework_rate < 10% after onboarding.

## Per-Run Log Template

Copy into session notes or `AGENT_HANDOFF.md`:

```text
PARRUG Run Log
- date:
- task_type: code | doc | research | mixed
- activated: yes | no (why skipped)
- result: GATE_GREEN | PARTIAL_GATE_GREEN | BLOCKED_WITH_NEXT_ACTIONS
- open_gates:
- findings: P0= / P1= / P2=
- rework: yes | no
- rework_reason:
```

## Monthly Rollup Template

```text
Period:
Eligible tasks:
PARRUG activated:
Adoption rate:
Runs started:
Clean closes:
Completion rate:
Reopened runs:
Rework rate:
Top rework reasons:
```

## Release Health Checks

Before tagging a release, verify:

1. `SKILL.md` frontmatter parses.
2. Internal doc links resolve.
3. Gate and evidence formats match between `SKILL.md` and `docs/protocol.md`.
4. No duplicate normative gate-green text outside SoT files.
5. At least one worked quickstart example in `README.md`.

See `docs/release-checklist.md` for the full checklist.
