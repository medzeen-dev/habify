---
dl: 22
title: "Module architecture: Ready Check as its own SCORM package; Impulsphase + Veränderungswerkstatt + Momentum combined into a single SCORM package with multiple internal lessons."
status: active
supersedes: []
superseded_by: []
---
# DL-022

> **Correction note (2026-07-10, DL-030):** The "combined module" packaged as a single SCORM/Web-Export unit is superseded for the MVP Web Export path — each phase (Impulsphase, Veränderungswerkstatt, Momentum) now ships as its own separate Rise Web Export, orchestrated by a persistent Shell. See DL-030. The Ready-Check-separate framing below is unaffected.

## Decision

Module architecture: Ready Check as its own SCORM package; Impulsphase + Veränderungswerkstatt + Momentum combined into a single SCORM package with multiple internal lessons.

## Context

Ready Check and the combined Impulsphase/Veränderungswerkstatt/Momentum module are delivered as two separate SCORM packages. Originally this split was justified by Ready Check's function as a technical prerequisite gate (see former DL-021, superseded by DL-023). Following DL-023, Ready Check no longer serves a gate function.

## Decision

Two packages: (1) Ready Check, standalone, free and unregistered; (2) the combined Impulsphase/Veränderungswerkstatt/Momentum package, the paid, seat-licensed product.

## Consequences

The two-package split is retained, but the rationale is now commercial rather than technical: Ready Check is a free, unregistered qualification and marketing tool; the combined module is the paid, seat-licensed product. This is a product/licence boundary, not a technical dependency. The `user_id` persistence risk that the original gate design created (across the Ready Check → combined-module boundary) no longer applies, since no participant-level data needs to cross that boundary — see DL-023 and 15_Technical_Architecture.md. Re-entry into a new Momentum cycle does not require this persistence either (new identifier per cycle, no cross-cycle correlation — see PB-038, 12_Backlog.md).
