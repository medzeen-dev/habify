# Arbeitsregeln habify

Wissens-Repo für Habify30. Produkt unter Kado, so geführt, dass es als eigene Brand
abtrennbar bleibt: dieses Repo ist per Clone vollständig portabel (§9).

**Einstieg: `README.md`.** Dort steht, was das Repo enthält und in welcher Reihenfolge zu
lesen ist. Diese Datei regelt ausschließlich die Repo-Mechanik — kein Duplikat.

## Governance — leicht, bewusst

`main` hat keine Push-Sperre, keine Pfad-Policy, keinen Reviewer-Zwang. Direkt-Commit auf
`main` ist der Normalweg. Kein PR erforderlich.

Begründung: Produktentscheidungen betreffen das Produkt, nicht die Geschäftsarchitektur.
Die Gate-Mechanik aus Verfassung §10 gilt für `kado` — hier nicht. Muster: `kado-os`
(B.1, Governance je Repo).

Dieser Zustand ist noch nicht kanonisiert. Der Norm-Akt folgt nach der Erprobungsphase
und beschreibt, was funktioniert hat.

## Eigene Normreihe

Habify30 führt seine eigene Decision-Log-Serie (DL-001 ff.) und seinen eigenen
Themen-Index. Diese Nummerierung bleibt unverändert. Sie ist NICHT die Kado-Serie
(DL-2026-xxx) und wird nicht angeglichen.

Kein Schema-Frontmatter-Zwang. Die Kado-`typ`-Werteliste gilt hier nicht.

## Was hier nicht liegt

- **Code** — eigenes Repo `habify-app` (§13).
- **Binärdateien und Assets** — nie in Git (§6). Quelldokumente, Kursmaterial und
  Design-Artefakte bleiben in OneDrive bzw. Figma; das Repo trägt nur den Verweis.
- **Personenbezogene Daten** — harte Grenze, keine Ausnahme (§15.1, „Git ist
  personenbezugsfrei"). Gilt hier genauso wie in `kado`.
- **Awaris-Material** — Partner-Sphäre, nie in einem Kado-Repo (DL-2026-001 ff.).
- **Nicht-kanonische Arbeitszustände** — unter `.wip/`, gitignored.

## Bindung an Kado-Normen

Werkzeug-Verträge und -Konventionen leben in `kado` und gelten hier per Verweis, nicht
per Kopie. Ein Werkzeug, ein Vertrag — auch wenn es in mehreren Sphären genutzt wird.

Relevant und in Kraft: `VTR-figma`, `KONV-figma`, `KONV-visuelle-zugaenglichkeit` und der
Ausführungs-Skill `figma-bauen` (alle in `kado`), gelten hier per Verweis. Siehe DL-077.

## Oberflächen und ihre Einstiege

Diese Datei wird von Claude Code über den Pfad geladen, nicht über Retrieval
(schema.md, DL-2026-011). Auf anderen Oberflächen greift sie nicht automatisch:

- **Claude Code** → `CLAUDE.md` (automatisch)
- **Claude-Projekt** → Projekt-Instruktion im UI-Feld; sie verweist auf `README.md`
- **Mensch, GitHub, andere KI** → `README.md`

Ändert sich die Leseordnung, ist sie in `README.md` zu ändern — nicht hier.

## Skills

habify-Skills leben versioniert im Repo unter `skills/<name>/SKILL.md` (Quelle der Wahrheit;
UI-Upload ist nur Deployment). Nur technisches Frontmatter, kein Kado-Schema. Siehe DL-079.

## Skripte

`kanon-pr.py` wird für dieses Repo nicht verwendet — es gibt kein Gate, das einen
PR-Zyklus rechtfertigt. Achtung: das Präfix `habify_` löst laut `KONV-git-pr-nutzung` §8
auf dieses Repo auf. Wird eine Datei mit diesem Präfix versehentlich durch `kanon-pr.py`
geschickt, sucht das Skript `schema/schema.md` und bricht mit Halt(4) ab. Das ist die
gewünschte Fehlerrichtung.
