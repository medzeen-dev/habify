# habify30 Knowledge Repository

**Version:** 1.0
**Status:** Active Development
**Repository Purpose:** Canonical Knowledge Base
**Primary Consumer:** Claude Projects
**Secondary Consumers:** GPT, Gemini, Cursor, Humans

---

# Purpose of this Repository

This repository is the canonical knowledge base for the habify30 project.

It preserves the relevant knowledge required to understand, develop, improve and operate habify30.

It is intentionally **not** a chat export.

It is **not** a meeting log.

It is **not** a collection of loose ideas.

Instead, it documents the current state of the project together with the reasoning that led to the current architecture.

Whenever contradictions exist between historical discussions and this repository, **this repository is considered the authoritative source.**

---

# Where this Repository Lives

As of 2026-07-07, this repository is maintained locally at:

`C:\Users\MatthiasNitsche\OneDrive - K-A-D-O\04_Product_Capabilities\habify30`

Claude accesses this folder directly via a filesystem connector. The Claude Project's file storage intentionally holds only this README as a pointer — the 21 canonical documents are not duplicated there. This avoids the two-system divergence risk that existed when both a project upload and a local copy were maintained in parallel.

---

# Repository Scope

This repository is the single source of truth for habify30's product
rationale, decisions, and production material (content drafts,
phase-specific assets referenced by filename, Fillout form text) — see
17_Production_Asset_Architecture.md for the full structure (decided
2026-07-08, DL-024).

It is deliberately **not** the source of truth for:

- Marketing and sales material, which lives at
  `C:\Users\MatthiasNitsche\OneDrive - K-A-D-O\05_Marketing_Sales\05_Materials\habify30\`,
  alongside other Kado products' marketing material.
- Binary design assets (images, illustrations, icons), which live at
  `C:\Users\MatthiasNitsche\OneDrive - K-A-D-O\03_Resources\01_Design\`,
  managed via Magnific project lists. Production documents in this
  repository reference these assets by filename only.

Where contradictions exist between historical discussions and this
repository regarding product rationale and decisions, this repository
remains the authoritative source, per the Repository Purpose section above.

---

# What is habify30?

habify30 is a B2B behaviour change product for organisations.

It helps employees translate learning, training and development experiences into sustained behavioural change in everyday work.

habify30 starts where many learning interventions end:

After the workshop.

After the training.

After the insight.

Its purpose is not to teach people more.

Its purpose is to help people actually behave differently.

---

# Core Problem

Organisations invest heavily in:

* leadership development
* training programmes
* workshops
* culture initiatives
* organisational development
* professional learning

Participants often leave these interventions motivated and inspired.

However, everyday work quickly returns.

Meetings, deadlines, habits, pressure and existing routines take over.

As a result, much of the intended behavioural change never becomes part of daily work.

This is the transfer gap.

habify30 exists to close this gap.

---

# Core Philosophy

Knowledge rarely changes behaviour on its own.

Repeated behaviour changes behaviour.

Therefore habify30 focuses on building sustainable work habits instead of providing more content.

Learning is treated as a catalyst.

Behaviour is treated as the outcome.

---

# Vision

Make sustainable behavioural transfer a normal outcome of organisational learning.

---

# Mission

Create a simple, evidence-informed transfer architecture that helps employees turn learning into everyday behaviour.

---

# Product Principle

habify30 does not compete with learning platforms.

habify30 complements them.

Existing platforms optimise learning.

habify30 optimises transfer.

---

# Positioning

habify30 is positioned as a transfer architecture rather than an e-learning platform.

The value proposition is not:

> Learn more.

The value proposition is:

> Make learning stick in everyday work.

---

# Target Market

habify30 is currently designed as a B2B product for organisations that already invest in learning and development.

Typical buyers include:

* Learning & Development
* HR
* People Development
* Leadership Development
* Organisational Development
* Internal Academies

Typical use cases include:

* leadership programmes
* feedback training
* communication training
* psychological safety initiatives
* culture change programmes
* new manager programmes
* collaboration development
* change management initiatives

---

# Primary Users

The primary users are employees participating in organisational learning or development programmes.

They may include:

* managers
* team leads
* project leads
* specialists
* graduates
* cross-functional teams
* programme participants

---

# Business Principle

The organisation finances habify30 because it benefits from improved transfer and greater return on learning investments.

Participants receive habify30 as part of an organisational development journey.

The product is therefore designed to create value for both:

* the participant, through easier behavioural implementation
* the organisation, through stronger learning transfer

---

# Scientific Foundation

habify30 draws upon established research in:

* behaviour change
* habit formation
* transfer of training
* self-determination theory
* behavioural psychology
* coaching psychology
* reflective practice
* implementation intentions
* identity-based behaviour change

The product intentionally avoids relying on motivation as the primary change mechanism.

---

# Current Product Status

The overall product architecture has been defined.

Major building blocks have been specified.

Several core product decisions are considered stable:

* habify30 is a B2B product.
* The primary problem is transfer, not learning.
* The product supports behavioural implementation after learning interventions.
* The user journey is structured around a ~6-week transfer process, with a 30-day Momentum Phase at its core (Ready Check, Impulsphase, Veränderungswerkstatt, Momentum).
* Simplicity, reflection, repetition and peer support are core design principles.
* Ready Check is a free, unregistered qualification tool with no gate function (see DL-023).

Implementation details continue to evolve.

---

# Repository Structure

This repository is organised into independent knowledge domains.

## Core Documents

* `README.md`
  Entry point and repository overview.

* `Glossary.md`
  Definitions of key terms.

* `Working with Matthias.txt`
  Working style, rationale and decision patterns of the primary product designer.

## Product Documentation

* `01_Project_summary.md`
  High-level overview of habify30.

* `02_Product_Vision.md`
  Why habify30 exists.

* `03_Product_Architecture.md`
  Overall product structure and transfer journey.

* `04_Business_Model.md`
  Commercial model and B2B positioning.

* `05_User_Journey.md`
  Participant experience and psychological journey.

* `06_Transfer_Architecture.md`
  Conceptual core and behavioural mechanisms.

* `07_Content_Architecture.md`
  Content principles and interaction design.

* `08_Research.md`
  Scientific foundations.

* `09_Decision_Log.md`
  Major product decisions and rationale.

* `10_Rejected_Ideas.md`
  Ideas intentionally not pursued.

* `11_Open_Questions.md`
  Known unresolved questions.

* `12_Backlog_md.txt`
  Future opportunities and possible development items.

* `13_AI_Working_Context.md`
  Guidance for AI systems working on habify30.

* `14_Project_History.md`
  Evolution of the project.

* `15_Technical_Architecture.md`
  The current technical architecture status of habify30.

* `16_Programminhalte.md`
  Programme content detail (German), corrected against decisions through 2026-07-03.

* `17_Production_Asset_Architecture.md`
  Production asset architecture: where production material lives relative to canonical rationale, and what this repository deliberately does not store.

* `SCORMxFillout_ProjectID_UserID_Architecture.md`
  Pseudonymous identifier architecture (pid/user_id) for the combined module.

* `SCORMxFillout_Connector_Export.md`
  SCORM↔Fillout↔Zoho connector implementation reference.

* `Catalyst_Platform_Capabilities.md`
  Empirical measurements of Zoho Catalyst capabilities relevant to habify30: Slate frontend hosting (Cluster A), ZCQL / Data Store query capabilities (Cluster B), Backup/DR (Cluster C). Updated as new measurements are taken. Referenced by DL-068 and DL-069.

* `Canon.md`
  Immutable product principles.

---

# Repository Principles

Every document follows the same rules.

## Facts over assumptions

Whenever possible, documented knowledge represents confirmed project decisions.

---

## Decisions over opinions

Important decisions include their rationale.

The reasoning behind a decision is often more valuable than the decision itself.

---

## No redundancy

Knowledge should exist exactly once.

Documents reference each other instead of duplicating content.

---

## Transparency

Unresolved questions remain unresolved.

Unknown information is never invented.

---

## Living Documentation

This repository is intended to evolve together with the project.

It should become the single source of truth for habify30.

---

# Confidence

## Established

* habify30 focuses on behavioural transfer rather than knowledge delivery.
* habify30 is currently scoped as a B2B product for organisations.
* The product complements existing learning and development interventions.
* Everyday work is the primary implementation environment.
* The ~6-week transfer journey, with its 30-day Momentum Phase, is the central product structure.
* This repository is maintained locally; the Claude Project file store holds only this README as a pointer.
* Repository scope covers product rationale and production material; marketing material and binary design assets are deliberately stored outside the repository (see DL-024, 17_Production_Asset_Architecture.md).

## Working Assumptions

* Technical implementation details may change.
* Client-specific adaptations may be required.
* Future product variants may emerge, but they are outside the current scope.

## Open Questions

* Standard success metrics for client reporting.
* Long-term reporting and analytics architecture.
* Pricing and licensing model.
* Degree of customisation for different client programmes.
