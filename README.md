# PARRUG

A tool-neutral gate-green work protocol for non-trivial agent tasks.

```text
Plan -> Act -> Review -> Refine -> Until Gate-Green
```

**Beginner:** PARRUG keeps agents from claiming "done" too early — every
non-trivial task goes through plan, execution, review, refinement, and evidence
before it closes.

**Power user:** A bounded `Plan -> Act -> Review -> Refine -> Gate-Green` loop
with typed gates, mandatory evidence, severity-based findings, and explicit stop
rules — runtime-neutral, no special features required.

## When to Use

Use PARRUG for multi-step work, ambiguous scope, cross-file changes, audits,
refactors, migrations, or any task where a premature "done" would be expensive.

Do **not** use for trivial one-step answers or tiny mechanical edits with one
obvious check.

## 5-Minute Quickstart

### 1. Ground (30 s)

Read the task, relevant source-of-truth files, and current workspace state.
Mark assumptions separately from confirmed facts.

### 2. Plan (1 min)

Write 3–6 steps. Each step must include verification:

```text
Step 1 -> verify: README.md contains a 5-minute quickstart section
Step 2 -> verify: SKILL.md frontmatter parses without YAML errors
Step 3 -> verify: evidence block is present in final output
```

Define gate-green criteria before acting.

### 3. Act (variable)

Execute only in-scope work. Keep edits surgical.

### 4. Review (1 min)

Check the nine standard gates (`scope`, `grounding`, `plan`, `execution`,
`checks`, `risk`, `review`, `refinement`, `handoff`). Classify findings as
P0/P1/P2 (see `docs/severity.md`).

### 5. Refine (if needed, bounded)

Fix concrete failed gates only. Default: two attempts per blocking gate. Stop
with `BLOCKED_WITH_NEXT_ACTIONS` if budget is exhausted.

### 6. Close with Evidence

```text
Evidence:
- sources: README.md, SKILL.md, docs/protocol.md
- actions: updated README quickstart; shortened SKILL.md
- checks: frontmatter parse -> pass; internal links -> pass
- findings: none
- open_gates: none
- result: GATE_GREEN
```

### Mini Example (doc-only task)

**Task:** Add a severity scale to the protocol docs.

```text
Step 1 -> verify: docs/severity.md exists with P0/P1/P2 definitions
Step 2 -> verify: SKILL.md links to docs/severity.md
Step 3 -> verify: no duplicate severity text in README and protocol
```

If all gates pass and evidence is complete → `GATE_GREEN`.

## Result Status

| Status | Meaning |
|--------|---------|
| `GATE_GREEN` | All applicable gates green |
| `PARTIAL_GATE_GREEN` | In-scope gates green; only pre-declared out-of-scope gates remain |
| `BLOCKED_WITH_NEXT_ACTIONS` | Blocked gate, missing evidence, or exhausted refinement budget |

## Repository Layout

```text
SKILL.md                     # Runtime contract (normative, keep short)
docs/protocol.md             # Deep protocol specification
docs/severity.md             # P0/P1/P2 definitions
docs/metrics.md              # Adoption / completion / rework metrics
docs/adapters.md             # Optional runtime install (Codex, Claude, Cursor…)
docs/release-checklist.md    # Pre-release verification
commands/parrug.md           # Thin slash-command entry point
templates/code-task.md       # Code-change starter
templates/doc-task.md        # Documentation starter
templates/research-task.md   # Research starter
templates/session-prompt.md  # Handoff / restart prompt (explicit request only)
references/foundations.md    # Research basis and design rationale
```

## Install

Clone into your agent's skill directory. Paths vary by runtime — see
`docs/adapters.md` for Codex, Claude, Cursor, and Windsurf examples.

```bash
git clone https://github.com/xaversebastian/parrug.git ~/.codex/skills/parrug
```

## Usage

Normal work (agent executes through PARRUG):

```text
/parrug Clean up this repository, update stale docs, run checks, and stop gate-green.
```

Handoff prompt (only when explicitly requested):

```text
/parrug Create a restart prompt for a new session that will perform the cleanup through PARRUG.
```

## Deep Protocol

Full specification: [`docs/protocol.md`](docs/protocol.md)

## License

MIT.
