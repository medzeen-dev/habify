---
dl: 20
title: "Standardised naming: `pid` = customer/cohort/programme run; `user_id` = individual participant."
status: active
supersedes: []
superseded_by: []
---
# DL-020

## Decision

Standardised naming: `pid` = customer/cohort/programme run; `user_id` = individual participant.

## Context

Three inconsistent names existed across code and documentation for the same two concepts (`project_id`/`pid` in code, `user_id` in one architecture doc, `participant_id` in another).

## Decision

`pid` identifies the customer/cohort/programme run (replaces the former `project_id`). `user_id` identifies the individual participant (replaces the former `pid` in the shipped code). Applies to URL parameters, Fillout hidden fields, Zoho field mapping, and all documentation.

## Consequences

Existing shipped code and both SCORM×Fillout architecture documents require renaming to match.
