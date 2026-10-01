---
cycle: 1
phase: setup
active_tracks: []
updated: 2026-10-01
---

# State

The cockpit card. Rewritten each session, archived each cycle to `docs/archive/`.
Capped at 200 lines by check E9.

## Now

Cycle 1: setup, smoke cycle and discovery. No product code this cycle.
The product is Shelf, a web app for a neighbourhood tool-lending library.

Bootstrap (`BOOTSTRAP.md` step 4) is done on branch `bootstrap`, not pushed:

- `CLAUDE.md`: mission, four draft principles, both human seats (PublicEnemage).
- `docs/roles.yml`: domain-core held by Product (author) and Verifier (challenger); domain
  area `tool-lending-operations` challenged by Verifier. Other charters unchanged.
- `appliance.yml`: grade light (proposed), merges human, data folders kept at defaults.
- `.github/CODEOWNERS`: data paths added.
- `docs/standards/data/`: all five standards adapted, still drafts. No Data Architect seat
  now; the decision and its reopen triggers are in the folder's README.
- `docs/dor/checklist.yml`: D11.1 to D11.5, one open row per data standard.

## Next

1. Step 5: a fresh session challenges the bootstrap against `docs/dor/floor.yml` and
   `docs/roles.yml`. Focus rows: C6, C8, D6, D11. The challenger must not be the Architect
   or Operator, the declared authors of the data standards; Verifier fits.
2. Step 6: the Engineering Lead reads the challenge and `DRYRUN-QUESTIONS.md`, then merges.
3. Step 7: `git config core.hooksPath .githooks`.
4. Step 8: smoke cycle. Not run.
5. Step 9: discovery track. Not started.

## Open decisions

All are logged with the assumption made in `DRYRUN-QUESTIONS.md`. For the Intent Owner:

- Mission wording and the four principles (Q7, Q8).
- Grade: light proposed; confirm in the business case (Q15, C6).
- Domain area name and the missing counter-perspective (Q13, C8).
- Money handling, reminder channel, jurisdiction (Q24 to Q26).

For the Engineering Lead:

- Seat charters, domain-core holders, and the template tests that read `roles.yml` (Q10 to Q12).
- Commit identity for agent sessions (Q4).
- Data standards, proposed classes and reference-data owners (Q20 to Q29).

## Left mid-task

None.
