# AGENTS.md: PARRUG

Tool-agnostisches Repo-Overlay für das öffentliche, runtime-neutrale
Gate-Green-Protokoll. In einem übergeordneten Workspace gelten zusätzlich die
dort gefundenen `AGENTS.md`; ein Standalone-Clone funktioniert ausschließlich
mit dieser Datei und enthält keine privaten Workspace-Annahmen.

<!-- CORE:start (eigenständig gepflegt, kein Generator) -->
# Workspace Core: gemeinsame Agentenregeln

Zweck: Ein tool-agnostischer Vertrag für autonome Arbeit. Repo- und
Tool-Overlays ergänzen nur konkrete Stack-, Daten-, Deployment- oder
Runtime-Fakten, widersprechen diesem Core nicht und fügen keine neue Rückfrage
hinzu.

## Autonomie und Planung

- Agenten erledigen Aufgaben vollständig: lesen, editieren, prüfen,
  begründete Dependencies samt Lockfile ändern, atomar committen, pushen,
  integrieren und eigene Branches/Worktrees aufräumen.
- Reversible Arbeit läuft ohne Rückfrage. Rückfrage nur bei unwiderruflicher
  Löschung, Kosten und Versand nach außen oder rechtsverbindlichen
  Handlungen.
- Der Orchestrator zerlegt Arbeit, koordiniert Abhängigkeiten und verifiziert
  den Abschluss; er ist nicht der einzige zulässige Editor oder Git-Akteur.
- Nicht-triviale Arbeit bekommt einen knappen internen Plan mit konkreten
  Checks. Reversible Arbeit braucht keine Planfreigabe.
- Spezialisierte Rollen dürfen ausdrücklich enger sein, etwa Review-only oder
  Draft-only. Diese Rollengrenze wird nicht zum globalen Default.

## Scope, Parallelität und Dirty State

- Parallele Writer arbeiten in unterschiedlichen Repos oder getrennten
  Worktrees desselben Repos. Pro Worktree gibt es genau einen Writer.
- Ein dirty Repo ist kein Stop. Fremde uncommitted Änderungen werden nicht
  überschrieben, gestaget, reformatiert oder verworfen; droht ihr Verlust,
  gilt das als unwiderrufliche Löschung und wird rückgefragt.
- Das Ergebnis nennt geänderte Pfade, gelaufene Checks und offene Risiken.

## Git und reversible Entwicklung

- Solo-Default: direkt auf `main`. Ein eigener Branch oder Worktree entsteht
  nur für einen echten parallelen zweiten Writer.
- Commit, Push, Merge und Deploy sind normale Arbeitsschritte.
- Aufräumen: eigene, saubere Worktrees entfernen und enthaltene Branches mit
  normalem `git branch -d` löschen.
- Force-Push, History-Rewrite, verlustbehafteter Hard-Reset und erzwungene
  Branch-Löschung sind unwiderrufliche Löschung und werden rückgefragt.

## Sicherheit und externe Wirkung

- Secrets werden nicht gelesen; Secret-Werte, Kunden-PII und private Rohdaten
  kommen nie in Git, Logs oder Ausgaben.
- Rückfrage nur in drei Fällen: unwiderrufliche Löschung, Geld und laufende
  Kosten, Versand an Menschen außerhalb oder rechtsverbindliche Handlungen.
  Alles andere, auch Provider-, Publish- und Deploy-Schreibvorgänge, ist
  reversible Arbeit ohne Rückfrage.
- Secret-Scan und Tests werden nicht umgangen oder geschwächt.
- Notwendige Dependencies sind mit Begründung, Manifest und geprüftem Lockfile
  erlaubt.

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
