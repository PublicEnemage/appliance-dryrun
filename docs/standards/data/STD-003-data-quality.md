---
id: STD-003
type: standard
title: Data quality
status: draft
author_seat: Architect
challenger_seat: Verifier
approver: Engineering Lead
parents: []
approved_at: null
---

# Data quality

- **Kind:** craft standard
- **Owning seat:** Architect, or the Data Architect once that seat is adopted. The
  Verifier challenges, and may not be the seat that designed the rules it applies.
- **Applies to:** every dataset that enters the system from outside: imports, feeds,
  third-party APIs, user uploads. For Shelf: a bulk import of an existing member or tool
  list, delivery results from the email or SMS provider, and photo uploads if the product
  takes them. Single-record form input is validated at the server boundary (STD-002
  clause 4) and needs no quality profile

Adapted for Shelf at bootstrap (2026-10-01). Still a draft: a challenger reviews it and
the Engineering Lead approves it (floor row D11). A rejection must cite a clause number.

## Clauses

1. **Every external dataset has a quality profile.** The profile names its validity rules,
   completeness threshold and freshness limit, in a file next to its contract.
2. **Provenance is recorded with the data.** Source, retrieval date, version or vintage,
   and licence travel with each dataset and are visible where its numbers are shown.
3. **Checks run at ingest.** Data that fails a rule is quarantined with the reason. It is
   never silently dropped, defaulted or passed through.
4. **Failures are loud.** A failed check raises an alert or fails the job. A missing or
   empty dataset is an error, not a valid state. (WorldSIM NM-060: an empty table produced
   a bare error with no diagnostic.)
5. **Quality is reported.** Each run writes a quality record: rows in, rows accepted, rows
   quarantined, and rules failed. The record is kept for the retention period of the data.

## Shelf notes

- A bulk import is the main risk. A library moving from a spreadsheet brings duplicate
  members, missing contact details and free-text tool names. Clause 3 applies: rows that
  fail are listed back to the coordinator, never dropped.
- A provider delivery result that never arrives is a missing input (clause 4). The
  reminder stays in a pending state that the coordinator can see.
