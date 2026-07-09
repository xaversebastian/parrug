# Release Checklist

Run before tagging or announcing a PARRUG release. No special runtime required.

## Structure

- [ ] `SKILL.md` exists and is the normative runtime contract (short).
- [ ] `docs/protocol.md` is the deep specification.
- [ ] `docs/severity.md` defines P0/P1/P2.
- [ ] `README.md` has 5-minute quickstart at the top.
- [ ] Tool-specific install is only in `docs/adapters.md`.

## Frontmatter

- [ ] `SKILL.md` YAML frontmatter parses (no unquoted colons in `description`).
- [ ] `commands/parrug.md` frontmatter parses if present.

## Consistency

- [ ] Gate set matches between `SKILL.md` and `docs/protocol.md`.
- [ ] Evidence format matches between `SKILL.md` and `docs/protocol.md`.
- [ ] Severity rules in `docs/severity.md`; no conflicting definitions elsewhere.
- [ ] `commands/parrug.md` delegates to `SKILL.md` without divergent rules.
- [ ] `templates/session-prompt.md` references SoT, does not duplicate full spec.
- [ ] `references/foundations.md` is rationale only, not a second normative spec.

## Templates

- [ ] `templates/code-task.md` exists.
- [ ] `templates/doc-task.md` exists.
- [ ] `templates/research-task.md` exists.
- [ ] `templates/session-prompt.md` exists for handoff mode only.

## Links

- [ ] All internal `docs/` links in README resolve.
- [ ] GitHub URL in README matches repository.

## Example

- [ ] README quickstart example is complete (plan steps with `verify`, evidence block).

## Manual Smoke Test

1. Give an agent a small doc-only task with PARRUG.
2. Confirm it produces plan steps with `verify`.
3. Confirm it closes with an evidence block and a valid result status.
4. Confirm it does not expose raw worker output.

## Changelog

- [ ] Note breaking vs non-breaking changes.
- [ ] Migration notes for moved install paths or renamed sections.
