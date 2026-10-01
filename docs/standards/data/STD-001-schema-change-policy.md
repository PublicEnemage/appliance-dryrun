---
id: STD-001
type: standard
title: Schema change policy
status: draft
author_seat: Architect
challenger_seat: Verifier
approver: Engineering Lead
parents: []
approved_at: null
---

# Schema change policy

- **Kind:** craft standard
- **Owning seat:** Architect, or the Data Architect once that seat is adopted
- **Applies to:** every persistent schema: database tables, document shapes, file formats, event payloads at rest.
  For Shelf: members, tools, loans, reminders and their delivery records, and library settings

Adapted for Shelf at bootstrap (2026-10-01). Still a draft: a challenger reviews it and
the Engineering Lead approves it (floor row D11). A rejection must cite a clause number.

## Clauses

1. **One source of truth.** The schema lives in the repository as code. No environment is
   changed by hand. (WorldSIM NM-003, NM-011: agents guessed field names because no
   owned schema existed.)
2. **Every change is a migration.** Each migration is a new file in the migrations folder
   named in `appliance.yml`. Migrations are append-only: a merged migration is never edited
   or deleted. A fix is a new migration. Check E15 refuses edits and deletions.
3. **Expand, migrate, contract.** A breaking change ships in three steps, in separate
   deploys: add the new shape, move the data and readers, then remove the old shape. The
   running version and the previous version must both work against the schema at every step.
4. **Destructive changes need an ADR.** Dropping a column, table or field, or narrowing a
   type, needs an ADR that names the data lost and the rollback path.
5. **Every write path is tested against the full schema.** Copy and branch paths are write
   paths. A NOT NULL or required field added later must be covered in every path that
   writes the record. (WorldSIM NM-036: a copy path missed a required column.)
6. **Migrations run before the code that needs them, and are checked.** Startup and the
   post-deploy check (E13) fail loudly when a migration is pending. (WorldSIM NM-049: a
   migration was never applied to the stack where validation ran.)
7. **Fixtures follow the schema.** Test fixtures are generated from, or validated against,
   the schema in CI (floor row D10). (WorldSIM NM-051, NM-086.)

## Shelf notes

- Clause 3 holds even with one running instance. A loan that is out when a deploy lands
  must still be returnable, and its reminder still sent, on both versions.
- Clause 4 covers any change that drops loan history. Loan history is how the coordinator
  settles a disputed return.
- The migrations folder is set in `appliance.yml`. If the chosen framework keeps
  migrations elsewhere, the stack ADR moves it before the first migration merges.
