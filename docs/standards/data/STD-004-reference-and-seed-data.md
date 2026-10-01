---
id: STD-004
type: standard
title: Reference and seed data
status: draft
author_seat: Architect
challenger_seat: Verifier
approver: Engineering Lead
parents: []
approved_at: null
---

# Reference and seed data

- **Kind:** craft standard
- **Owning seat:** Architect owns this standard; each reference dataset names its owner
- **Applies to:** lookup tables, code lists, configuration data, and the seed data each
  environment starts with. For Shelf: tool categories, the default loan period, the
  reminder schedule (days before and after the due date), and library settings

Adapted for Shelf at bootstrap (2026-10-01). Still a draft: a challenger reviews it and
the Engineering Lead approves it (floor row D11). A rejection must cite a clause number.

## Clauses

1. **Every reference dataset has a named owner seat.** The owner approves every change.
2. **Reference data is versioned in the repository.** Changes go through a pull request,
   never a manual edit to an environment.
3. **Seeding is a script, and the script is idempotent.** Running it twice leaves the same
   state. Each environment records the seed version it holds.
4. **Seed presence is checked, not assumed.** Startup, the health endpoint (floor row P8)
   and tests check that required seed data is present, and fail loudly when it is not.
   A test never skips because data is missing. (WorldSIM NM-097: tests skipped on a
   connection check while the database was under-seeded.)
5. **Test seeds are synthetic.** No production personal data is used as seed data in any
   non-production environment.

## Shelf notes

- Proposed owners, confirmed when the datasets are named in the architecture: tool
  categories and the default loan period, Product; the reminder schedule, Product, with
  Operator approving any change that alters send volume or cost.
- Clause 5 matters more than usual. A library's first members are neighbours, and a
  test seed copied from a real sign-up sheet would expose them. Seeds use invented
  names, invented addresses and reserved test email domains.
