# Habify30 Knowledge Repository

**Version:** 1.1
**Status:** Active Development
**Repository Purpose:** Canonical Knowledge Base
**Last structural change:** 2026-09-03 — migrated from OneDrive to Git

---

# Purpose of this Repository

This repository is the canonical knowledge base for the Habify30 project.

It preserves the relevant knowledge required to understand, develop, improve and operate Habify30.

It is intentionally **not** a chat export.

It is **not** a meeting log.

It is **not** a collection of loose ideas.

Instead, it documents the current state of the project together with the reasoning that led to the current architecture.

Whenever contradictions exist between historical discussions and this repository, **this repository is considered the authoritative source.**

---

# Read This First

Before proposing product changes, writing content or building anything:

1. **`00_Index.md`** — topic index to the Decision Log. Look up which DL entries touch the topic at hand. Only then build. This step is not optional; the index exists because it was once skipped.
2. **`13_AI_Working_Context.md`** — judgement criteria for product proposals: behavioural hypothesis before feature, simplicity as a design goal, architecture versus implementation, evidence tiers.
3. **`09_Decision_Log.md`** — read selectively, never in one pass. At roughly 125,000 characters it is silently truncated when read whole. Access it through the index, section by section.

`CLAUDE.md` in the repository root holds the working rules for the repository itself — governance, what belongs here, what does not. It is loaded automatically by Claude Code; on other surfaces this README is the entry point.

---

# Where this Repository Lives

As of 2026-09-03, this repository is a Git repository in Azure DevOps:

`https://dev.azure.com/kado-org/kado/_git/habify`

It replaces the previous OneDrive folder as the authoritative location. The OneDrive folder still holds source material that does not belong in Git — Word documents, PDFs, spreadsheets, course materials and transient working documents. Those remain there by design; Git carries text only.

Product code lives in a separate repository, `habify-app`, not here.

---

# What is Habify30?

Habify30 is a B2B behaviour change product for organisations.

It helps employees translate learning, training and development experiences into sustained behavioural change in everyday work.

Habify30 starts where many learning interventions end:

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

Habify30 exists to close this gap.

---

# Core Philosophy

Knowledge rarely changes behaviour on its own.

Repeated behaviour changes behaviour.

Therefore Habify30 focuses on building sustainable work habits instead of providing more content.

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

Habify30 does not compete with learning platforms.

Habify30 complements them.

Existing platforms optimise learning.

Habify30 optimises transfer.

---

# Positioning

Habify30 is positioned as a transfer architecture rather than an e-learning platform.

The value proposition is not:

> Learn more.

The value proposition is:

> Make learning stick in everyday work.

---

# Target Market

Habify30 is currently designed as a B2B product for organisations that already invest in learning and development.

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

The organisation finances Habify30 because it benefits from improved transfer and greater return on learning investments.

Participants receive Habify30 as part of an organisational development journey.

The product is therefore designed to create value for both:

* the participant, through easier behavioural implementation
* the organisation, through stronger learning transfer

---

# Scientific Foundation

Habify30 draws upon established research in:

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

* Habify30 is a B2B product.
* The primary problem is transfer, not learning.
* The product supports behavioural implementation after learning interventions.
* The user journey is structured around a ~6-week transfer process, with a 30-day Momentum Phase at its core (Ready Check, Impulsphase, Veränderungswerkstatt, Momentum).
* Simplicity, reflection, repetition and peer support are core design principles.
* Ready Check is a free, unregistered qualification tool with no gate function (see DL-023).

Implementation details continue to evolve.

---

# Repository Structure

This repository is organised into independent knowledge domains.

## Entry Points

* `README.md`
  This document. Entry point and repository overview.
* `CLAUDE.md`
  Working rules for the repository: governance, boundaries, what does not belong here.
* `00_Index.md`
  Topic index to the Decision Log. Consult before building anything.
* `Glossary.md`
  Definitions of key terms.
* `Canon.md`
  Immutable product principles.

## Product Documentation

* `01_Project_summary.md`
  High-level overview of Habify30.
* `02_Product_Vision.md`
  Why Habify30 exists.
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
  Major product decisions and rationale. Read through the index, never whole.
* `10_Rejected_Ideas.md`
  Ideas intentionally not pursued.
* `11_Open_Questions.md`
  Known unresolved questions.
* `12_Backlog.md`
  Future opportunities and possible development items.
* `13_AI_Working_Context.md`
  Judgement criteria for AI systems working on Habify30.
* `14_Project_History.md`
  Evolution of the project.
* `15_Technical_Architecture.md`
  Current technical architecture status.
* `16_Programminhalte.md`
  Programme content detail (German), corrected against decisions through 2026-07-03.
* `17_Production_Asset_Architecture.md`
  Production asset structure.
* `Catalyst_Platform_Capabilities.md`
  Verified capabilities and limits of the Zoho Catalyst platform.
* `SCORMxFillout_ProjectID_UserID_Architecture.md`
  Pseudonymous identifier architecture (pid/user_id) for the combined module.
* `SCORMxFillout_Connector_Export.md`
  SCORM↔Fillout↔Zoho connector implementation reference.

---

# What Does Not Live Here

* **Code** — separate repository, `habify-app`.
* **Binary files and assets** — Word documents, PDFs, spreadsheets, course materials and design artefacts stay in OneDrive and Figma. Git carries text only.
* **Personal data** — no exception, on any surface.
* **Transient working documents** — handoff briefs, propagation notes, session states. These remain outside the repository, or under `.wip/`, which is gitignored.

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

It should become the single source of truth for Habify30.

---

# Confidence

## Established

* Habify30 focuses on behavioural transfer rather than knowledge delivery.
* Habify30 is currently scoped as a B2B product for organisations.
* The product complements existing learning and development interventions.
* Everyday work is the primary implementation environment.
* The ~6-week transfer journey, with its 30-day Momentum Phase, is the central product structure.
* The repository is maintained in Git; Azure DevOps `habify` is the authoritative location.

## Working Assumptions

* Technical implementation details may change.
* Client-specific adaptations may be required.
* Future product variants may emerge, but they are outside the current scope.
* The repository governance model is under empirical trial and not yet formalised.

## Open Questions

* Standard success metrics for client reporting.
* Long-term reporting and analytics architecture.
* Pricing and licensing model.
* Degree of customisation for different client programmes.
* Document language: the corpus is English, repository working rules are German. Not yet decided whether this stays.
