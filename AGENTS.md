# AGENTS.md — PARRUG

Tool-agnostisches Repo-Overlay für das öffentliche, runtime-neutrale
Gate-Green-Protokoll. In einem übergeordneten Workspace gelten zusätzlich die
dort gefundenen `AGENTS.md`; ein Standalone-Clone funktioniert ausschließlich
mit dieser Datei und enthält keine privaten Workspace-Annahmen.

<!-- CORE:start (generiert via sync-instructions.mjs, NICHT editieren) -->
# Workspace Core: gemeinsame Agentenregeln

Zweck: Ein tool-agnostischer Vertrag für autonome, scope-sichere Arbeit. Repo-
und Tool-Overlays ergänzen nur konkrete Stack-, Brand-, Daten-, Deployment- oder
Runtime-Grenzen und widersprechen diesem Core nicht.

## Autonomie und Planung

- Native Agenten dürfen klar zugewiesene Aufgaben vollständig erledigen:
  lesen, editieren, prüfen, begründete Dependencies samt Lockfile ändern,
  atomar committen, normale nicht-produktive Pushes ausführen, integrieren und
  nach bewiesener Integration eigene Branches/Worktrees aufräumen.
- Der Orchestrator zerlegt Arbeit, koordiniert Abhängigkeiten und verifiziert
  den Abschluss; er ist nicht der einzige zulässige Editor oder Git-Akteur.
- Nicht-triviale Arbeit bekommt einen knappen internen Plan mit konkreten
  Checks. Reversible lokale Arbeit benötigt keine zusätzliche Planfreigabe.
- Spezialisierte Rollen dürfen ausdrücklich enger sein, etwa Review-only oder
  Draft-only. Diese Rollengrenze wird nicht zum globalen Native-Default.

## Scope, Parallelität und Dirty State

- Parallele Writer arbeiten in unterschiedlichen Repos oder getrennten
  Worktrees desselben Repos. Pro Worktree gibt es genau einen Writer.
- Jede schreibende Aufgabe hat Baseline, Read-/Owned-/Forbidden-Paths,
  erlaubte Aktionen, Verifikationsplan und strukturiertes Ergebnis-Receipt.
- Ein dirty Repo ist kein pauschales Stop-Gate. Gestoppt wird nur bei
  Schreibdrift, Owned-Path-Überlappung, ungeklärter Eigentümerschaft oder
  drohendem Verlust fremder Änderungen.
- Disjunkte fremde Änderungen bleiben unangetastet. Bei fremdem Dirt ist ein
  isolierter Worktree der Standard, sofern der bestehende Worktree nicht
  ausdrücklich der Aufgabe gehört.

## Git und reversible Entwicklung

- Topologie folgt Scope und Risiko: seriell im zugewiesenen Checkout, parallel
  im eigenen Branch/Worktree. Es gibt keinen pauschalen Main- oder Branch-Zwang.
- Normale Commits, nicht-produktive Pushes und konfliktfreie Integration sind
  erlaubt, wenn sie zum Auftrag gehören und der Zielstand verifiziert ist.
- Cleanup ist nur für eigene, saubere Worktrees und nachweislich enthaltene
  Branches erlaubt: Ziel-Commit verifizieren, Worktree entfernen, Branch mit
  normalem `git branch -d` löschen.
- Kein Force-Push, `--no-verify`, verlustbehafteter Hard-Reset, History-Rewrite
  oder erzwungene Branch-Löschung ohne explizite menschliche Freigabe.
- Ein Push mit nachgewiesenem automatischem Production-Deploy ist ein Deploy.
  Unbekannte Push-Wirkung wird vor dem Push klassifiziert.

## Sicherheit und externe Wirkung

- Secrets, Kunden-PII, private Rohdaten und Cross-Brand-Daten werden weder
  unautorisiert gelesen noch ausgegeben, übertragen oder committed.
- Menschliche Gates bleiben für irreversible Datenlöschung, produktive DB-
  Migrationen, produktive Auth-/RLS-/Payment-Rollouts, Live-Billing oder
  Geldbewegungen, rechtsverbindliche Handlungen, Kunden-/öffentliche
  Kommunikation sowie Production-/Provider-/DNS-Writes bestehen.
- Lokales Schreiben und Testen von Auth-, RLS-, Payment- und Migrationscode ist
  erlaubt. Der Gate betrifft die produktive oder irreversible Wirkung.
- Notwendige Dependencies sind mit Begründung, Owned-Manifest und geprüftem
  Lockfile erlaubt. Publish oder externe Registry-/Provider-Writes bleiben
  gesondert gegatet.

## Risikoproportionale Verifikation

- Docs/Instructions: betroffene Struktur-, Drift-, Syntax- und Contract-Checks.
- Code: passende Typechecks, Lints, Unit-/Integrationstests und Builds für den
  veränderten Scope.
- UI: relevanter Build plus passende UI-/E2E- und nötige manuelle Abnahme.
- Auth/RLS/Payment/Migration: lokale Security- und Integrationstests.
- Vollständige Produkt-Gates nur, wenn Risiko, Repo-Vertrag oder Pre-Push-Hook
  sie für den betroffenen Scope verlangt. Checks dürfen Fehler nicht schlucken.

## Doku-Hygiene

- `PROJECT_STRUCTURE.md` nur bei relevanter Datei-, API-, Hook-, Skill- oder
  Topologieänderung aktualisieren.
- `AGENT_HANDOFF.md` nur bei fortzusetzendem Cross-Session-Stand, offenem Risiko
  oder echter Übergabe ergänzen. Ein vollständiges Task-Receipt genügt sonst.
- Historische Handoffs bleiben historisch; aktive Regeln leben in kanonischen
  Instruction-Quellen, generierten Blöcken und dünnen Overlays.
<!-- CORE:end -->

## Session-Start

1. `PROJECT_STRUCTURE.md` lesen.
2. `AGENT_HANDOFF.md` lesen.
3. Diese Datei und danach `SKILL.md` lesen.
4. `git status --short --branch` und den exakten Doku-/Skill-Scope prüfen.

## Repo-Grenzen

- PARRUG bleibt runtime-, modell- und provider-neutral.
- Normative Protokolländerungen müssen in `SKILL.md` und `docs/protocol.md`
  konsistent sein; Adapter bleiben dünn.
- Keine privaten Workspace-Pfade, Kundendaten oder MFC-Brandregeln in das
  öffentliche Paket übernehmen.

## Verifikation

- Docs/Skill: Frontmatter, interne Links, Shell-Syntax der Adapter und
  `git diff --check` für den betroffenen Scope.
- Release-nahe Änderungen: zusätzlich `docs/release-checklist.md` abarbeiten.
