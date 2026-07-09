# Changelog

## Unreleased — Doku-Relaunch (Execution Spec v1)

### Added

- `docs/protocol.md` — deep protocol specification
- `docs/severity.md` — P0/P1/P2 definitions
- `docs/metrics.md` — adoption, completion, rework metrics
- `docs/adapters.md` — optional Codex/Claude/Cursor/Windsurf install
- `docs/release-checklist.md` — pre-release verification
- `templates/code-task.md`, `templates/doc-task.md`, `templates/research-task.md`

### Changed

- `README.md` — 5-minute quickstart, clearer when-to-use, repo layout
- `SKILL.md` — shortened runtime contract; YAML frontmatter hardened
- `commands/parrug.md` — thin entry point delegating to `SKILL.md`
- `templates/session-prompt.md` — handoff-only, references SoT
- `references/foundations.md` — rationale only; normative rules moved to docs

### Migration (non-breaking)

- **Install paths unchanged** — same `git clone` URL and skill directory layout.
- **Slash commands** — `commands/parrug.md` still works; behavior unchanged, less duplication.
- **Superpowers** — moved to optional `docs/adapters.md#process-skills`; not required.
- **Gate-green definition** — now in `docs/protocol.md` §4; `SKILL.md` has summary table.
- **New readers** — start with README quickstart; power users read `docs/protocol.md`.

### Breaking

None intended. If you inlined old gate-green text from `foundations.md`, update
references to `docs/protocol.md`.
