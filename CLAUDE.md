# habify — Arbeitsregeln

Wissens-Repo für Habify30. Produkt unter Kado, so geführt, dass es als eigene Brand
abtrennbar bleibt: dieses Repo ist per Clone vollständig portabel (§9).

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

- **Code** — eigenes Repo (§13).
- **Binärdateien und Assets** — nie in Git (§6). Figma bleibt autoritativ für
  Design-Artefakte, das Repo trägt nur den Verweis.
- **Personenbezogene Daten** — harte Grenze, keine Ausnahme (§15.1, „Git ist
  personenbezugsfrei"). Gilt hier genauso wie in `kado`.

## Bindung an Kado-Normen

Werkzeug-Verträge und -Konventionen leben in `kado` und gelten hier per Verweis, nicht
per Kopie. Ein Werkzeug, ein Vertrag — auch wenn es in mehreren Sphären genutzt wird.

Relevant, sobald geschrieben: Figma-Vertrag und -Konvention (existieren noch nicht).

## Skripte

`kanon-pr.py` wird für dieses Repo nicht verwendet — es gibt kein Gate, das einen
PR-Zyklus rechtfertigt. Achtung: das Präfix `habify_` löst laut `KONV-git-pr-nutzung` §8
auf dieses Repo auf. Wird eine Datei mit diesem Präfix versehentlich durch `kanon-pr.py`
geschickt, sucht das Skript `schema/schema.md` und bricht mit Halt(4) ab. Das ist die
gewünschte Fehlerrichtung.
