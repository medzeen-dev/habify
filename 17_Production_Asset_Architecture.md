# 17_Production_Asset_Architecture.md

**Document Version:** 1.0
**Status:** Living Document
**Purpose:** Define where structured production material lives relative to the canonical Rationale documents, and what this repository deliberately does not store.

---

# Purpose

This document specifies the repository's production asset architecture: how folders under this repository relate to each other, which material belongs here and which does not, and the naming and rendering conventions that apply once production material accumulates.

It exists because content production has begun (SCORM, Fillout, handouts, presentations, website, marketing), and without an explicit per-folder ownership model, production material and canonical rationale drift into the same undifferentiated pile as volume grows — a divergence problem already observed twice in practice (docx vs. .md content drift; duplicate files across folders). See DL-024 for the decision this document implements.

---

# 01_Framework — Rendering, Not Retelling

`01_Framework/` holds a single Word document intended for newcomers to the project. This document is a **rendering** of the canonical root documents — `01_Project_summary.md`, `02_Product_Vision.md`, `03_Product_Architecture.md` and `06_Transfer_Architecture.md` — assembled into one onboarding-friendly read. It is not an independent restatement of the product. Its content is derived from those four documents; if the framework document and a root document ever disagree, the root document is authoritative, and the framework document should be regenerated rather than edited to resolve the conflict.

`01_Framework/Theorie_Konzepte/` holds raw source material underneath this: books, articles and links, including where in the source they were found. This is distinct from `08_Research.md`, which holds the distilled, applied-principles layer actually used in product decisions. `Theorie_Konzepte/` is the library; `08_Research.md` is what was taken from that library and put to work. New source material goes into `Theorie_Konzepte/`; only material that has been distilled into an applied principle earns a place in `08_Research.md`.

---

# 03_Contents — Phase Folders as Authoring Structure

`03_Contents/` has four phase folders: `0_Ready Check`, `1_Impulsphase`, `2_Veränderungswerkstatt` and `3_Momentumphase`. Each contains a `Fillout/` subfolder plus whatever other material belongs to that phase — text building blocks, handouts, audio, video, and phase-specific graphics.

This four-folder structure is an authoring-time grouping only. It exists to keep the people writing content organised by phase while they work. It does **not** imply four separate SCORM packages. Delivery remains exactly as decided in DL-022: two SCORM packages — Ready Check standalone, and Impulsphase, Veränderungswerkstatt and Momentum combined into one package with multiple internal lessons. The authoring folders and the delivery packages are two different groupings of the same material, and only the latter is a technical/commercial boundary. This resolves OQ-021: the four-folder authoring structure was never a case for merging Ready Check into the combined package, and the two-package split from DL-022 stands.

---

# 02_Assessments — Dissolved

`02_Assessments/` is dissolved. Its function is now fully covered by `03_Contents/<Phase>/Fillout/` — assessment material lives with the phase it belongs to rather than in a separate cross-phase folder.

---

# 04_Compliance_GDPR_Ethics — Out of Scope Here

`04_Compliance_GDPR_Ethics/` holds all data protection documents. Its current content is reviewed separately and is explicitly not addressed by this document or by the reorganisation this document accompanies.

---

# What This Repository Does Not Store

Two categories of material are deliberately kept outside this repository, even though they relate directly to habify30 production.

## Binary Design Assets

Images, illustrations and icons are not stored in this repository. They live centrally at `C:\Users\MatthiasNitsche\OneDrive - K-A-D-O\03_Resources\01_Design\` (under `Images`, `Illustrations` and `Icons`), managed through Magnific project lists. Production documents in this repository reference these assets by filename only, never by embedding or duplicating the binary itself.

## Marketing and Sales Material

Marketing and sales material is not stored in this repository. It lives at `C:\Users\MatthiasNitsche\OneDrive - K-A-D-O\05_Marketing_Sales\05_Materials\habify30\`, alongside the marketing material of other Kado products.

---

# Naming Convention for Binary and Production Assets

Binary and production assets (everything covered by this document — not the canonical root `.md` documents, which keep their existing Living Document convention) follow the pattern:

`<name>_V<n>_<YYMMDD>.<ext>`

There is no separate version history to maintain alongside this — OneDrive already versions files — so the version number and date in the filename are simply updated in place at every revision.

---

# Optional Word Renderings of Canonical Documents

Any canonical root `.md` document may optionally get a readable Word copy: same filename, `.docx` extension, sitting at root level next to the source `.md`. The `.md` file remains the sole canonical source. The `.docx` is always generated from it and is never hand-edited — any change belongs in the `.md`, followed by regenerating the `.docx`. The first application of this convention is `16_Programminhalte.docx`, generated from `16_Programminhalte.md`.

---

# Content Voice and Skill Scope

The `kado-content-voice` skill applies to everything under `03_Contents/`, to the `01_Framework/` onboarding document, and to future handout and presentation material. It does not apply to the canonical root documents — those are internal specification and rationale, not participant- or customer-facing text, and keep their existing plain, precise documentation register.

---

# Confidence

## Established

* Production material and canonical rationale are governed by different rules and live in different places, per DL-024.
* The four `03_Contents/` phase folders are an authoring-time grouping; delivery remains two SCORM packages per DL-022 (OQ-021 resolved).
* `02_Assessments/` is dissolved; its function moved into `03_Contents/<Phase>/Fillout/`.
* Binary design assets and marketing/sales material are intentionally stored outside this repository, at the OneDrive locations specified above.

## Working Assumptions

* The naming convention for binary/production assets (`<name>_V<n>_<YYMMDD>.<ext>`) may need adjustment once more production material of different types accumulates.
* The optional `.docx` rendering convention is untested beyond its first application (`16_Programminhalte.docx`); it may need refinement once used for other documents.

## Open Questions

* Whether other root documents besides `16_Programminhalte.md` will need a `.docx` rendering, and on what cadence renderings get regenerated as the source `.md` changes.
