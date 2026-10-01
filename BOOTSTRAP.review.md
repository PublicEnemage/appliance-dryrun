---
artifact: BOOTSTRAP
challenger_seat: Verifier
session: "separate session, 2026-10-01, BOOTSTRAP.md step 5"
open_findings: 22
---

# Review of the Shelf bootstrap (BOOTSTRAP.md step 4)

Scope: commits b455712 to 6c939e9 on branch `bootstrap`, compared with the template at
b6e52ab. Judged against `docs/dor/floor.yml`, `docs/roles.yml`, `CLAUDE.md`, `BOOTSTRAP.md`,
`docs/artifact-types.yml`, the checks, and the grade table in `docs/method/appliance-design.md`.

## Where this file sits, and why

The constitution says findings go "in the review file beside the artifact"
(`CLAUDE.md`, The chain), and `docs/templates/review.md` names that file
`PREFIX-NNN-slug.review.md`. The bootstrap is not one artifact. Its output spans `CLAUDE.md`,
`docs/roles.yml`, `appliance.yml`, `.github/CODEOWNERS`, `docs/dor/checklist.yml` and five
standards, and it has no ID. The step that defines that output is `BOOTSTRAP.md`, so this
file sits beside it as `BOOTSTRAP.review.md`, next to `DRYRUN-QUESTIONS.md`. The repository
root is outside every artifact folder, so check E10 ignores this file. This file does not
serve as the review file of STD-001 to STD-005 (see finding 14).

The seat is Verifier, as `STATE.md` proposes. Finding 10 questions that the author chose it.

## Checks run

- `python3 tools/checks/run_all.py`: 9/9 pass.
- `python3 -m pytest -q tests`: 169 passed.
- `python3 tools/checks/check_dor.py --gate case`: blocked on C1 to C9, all open. Expected.

Every check passes. No check covers the defects below, so a green run is no evidence here.

## Summary

| # | Finding | Severity | Answer | Closed? |
| --- | --- | --- | --- | --- |
| 1 | Grade light contradicts the grade table for a product with real users | high | | no |
| 2 | Verifier declared qualified for `tool-lending-operations` with no basis | high | | no |
| 3 | domain-core assigned to seats with no domain knowledge, bypassing the crew review | high | | no |
| 4 | Data standards declare authors who did not write them | high | | no |
| 5 | Mission leaves the lending model undefined: shared library or member to member | medium | | no |
| 6 | STD-005 classes and access rules are inconsistent with each other and the mission | medium | | no |
| 7 | "Nothing is regulated" asserted while jurisdiction is open | medium | | no |
| 8 | Data Architect decision sits outside the chain and fixes D6 seats early | medium | | no |
| 9 | No `qualified_layers` narrowed, though step 4 requires it | medium | | no |
| 10 | The author chose its own challenger and the rows to focus on | medium | | no |
| 11 | Agent filled the human seats; step 2 unverified and not listed as open | medium | | no |
| 12 | Commits use the agent identity, not the human's | medium | | no |
| 13 | Verifier carries almost every independent check | medium | | no |
| 14 | STD-005 cannot be reviewed by this challenge; no standard has a review file | medium | | no |
| 15 | STD-003 carve-out cites the wrong clause | medium | | no |
| 16 | `appliance.yml` header reads as a recorded approval | medium | | no |
| 17 | D11.1 to D11.5 restate the parent and add no evidence path | low | | no |
| 18 | CODEOWNERS paths are not tied to `appliance.yml` data folders | low | | no |
| 19 | Principles overlap, and STD-002 cites one by number | low | | no |
| 20 | Repository still introduces itself as the template | low | | no |
| 21 | STD-004 gives one dataset two approvers | low | | no |
| 22 | STD-005 access log has no minimum, so its content will be guessed | low | | no |

## Findings

### 1. Grade light contradicts the grade table (high)

- **Where:** `appliance.yml`, `grade` and its comment. Floor row C6.
- **Problem:** The grade table in `docs/method/appliance-design.md` (Grades of rigor) gives
  Light to a "spike, internal tool, throwaway prototype" and Standard as the "default for
  anything with users". Shelf has members and stores neighbours' names, contact details and
  loan history (STD-005 notes). The comment says light holds "while the idea is tested with
  one library" and standard comes "before real members' personal data is stored in
  production". A test with one library uses real members. No phase exists where Shelf has
  users and no real personal data. Light also lets the challenger share a session and folds
  each phase into one artifact. STD-005 relies on review depth that light removes.
- **Resolve:** Propose standard. If the Intent Owner keeps light, the business case states
  the reason, and the raise trigger becomes a measurable event: the first real member
  record in any environment.

### 2. Verifier declared qualified for `tool-lending-operations` with no basis (high)

- **Where:** `docs/roles.yml`, `qualified_domains` and the comment above it. Floor row C8.
- **Problem:** The header of `docs/roles.yml` says to narrow each list to what the holder can
  judge, and that a domain no seat lists makes the check fail as the signal for a crew review.
  The comment beside the entry says "No agent seat has first-hand tool-library practice."
  The entry declares the Verifier qualified anyway. The C8 check will pass on a declaration
  the same file contradicts. C8 also asks for a counter-perspective where judgment is
  contested. Discovery evidence (C9) is an input, not a seat. It cannot challenge a draft.
  The Verifier already challenges the business case as a whole, so the domain challenge adds
  no second view.
- **Resolve:** Remove the entry so C8 refuses when the case names the area. Open a role
  proposal (`docs/templates/role-proposal.md`), for example an advisory seat held by a
  practising volunteer coordinator. Or the Intent Owner signs the gap as an accepted risk in
  the business case.

### 3. domain-core assigned to seats with no domain knowledge (high)

- **Where:** `docs/roles.yml`, Product and Verifier `qualified_layers`; `DRYRUN-QUESTIONS.md`
  Q11 and Q12. Floor rows D2, D6.
- **Problem:** The template left domain-core unassigned on purpose, so D6 would fail and a crew
  review would open. `tests/test_check_seats.py` (around line 123) shows the intended remedy: a
  seat such as "Domain Advisor". The bootstrap gave domain-core to Product and Verifier. Q12
  admits neither knows tool lending. Q11 says the choice was partly made to keep the
  template's tests green. Product writes the use cases and would then write the domain core
  from them. The Verifier challenges both. Nobody outside that pair looks at the loan
  lifecycle.
- **Resolve:** Revert both additions and run the crew review the template expects. If Shelf's
  core is thin CRUD, mark domain-core not-applicable in the architecture with a reason and an
  approver. Fix the test coupling separately (see the list at the end).

### 4. Data standards declare authors who did not write them (high)

- **Where:** front matter of STD-001 to STD-004 (`author_seat: Architect`) and STD-005
  (`author_seat: Operator`); `DRYRUN-QUESTIONS.md` Q2. `CLAUDE.md`, The chain.
- **Problem:** An unseated bootstrap session wrote all five adaptations. The front matter says
  the Architect and the Operator did, on two different holders. `docs/enforcement.yml` E1
  says the seat checks catch inconsistent declarations, not false ones. This is a false one.
  The "Shelf notes" also state product facts no upstream artifact holds: loan history settles
  disputed returns, libraries arrive from a spreadsheet, the reminder schedule drives send cost.
  The constitution lets an author write only from named upstream artifacts.
- **Resolve:** Have an Architect session and an Operator session adopt or rewrite their
  standards, and say so in each file. Or name a bootstrap seat in `BOOTSTRAP.md` and record it
  as author. Mark each Shelf note as an assumption, naming the artifact that confirms it (C4,
  C5 or the architecture).

### 5. Mission leaves the lending model undefined (medium)

- **Where:** `CLAUDE.md`, Mission; `DRYRUN-QUESTIONS.md` Q7.
- **Problem:** "Members list the tools they will lend, borrow from each other" describes member
  to member lending. "A neighbourhood tool-lending library" and a coordinator who sees "what is
  out" suggest a shared inventory. The two models differ in where a tool is between loans, who
  confirms a return, whose address a borrower sees, and whom the coordinator chases. The
  domain core, the data model and STD-005 all depend on the answer. Q7 does not log it.
- **Resolve:** Log it as an Intent Owner question. The mission states who holds a tool between
  loans and who confirms a return.

### 6. STD-005 classes and access rules are inconsistent (medium)

- **Where:** `docs/standards/data/STD-005-data-governance.md`, Shelf notes table and clause 5
  note.
- **Problem:** Loan history is confidential because it "shows ... what they own". Tool listings
  are internal, yet a listing tied to a lender shows exactly what that member owns, and in
  member to member lending, where it is. The clause 5 note logs coordinator access to contact
  details. Member to member lending needs members to see each other's contact details. No rule
  covers that access.
- **Resolve:** Class listings by the same test as loan history, or state why they differ. Add
  an access rule for member to member contact, or record that the model in finding 5 removes
  the need.

### 7. "Nothing is regulated" asserted while jurisdiction is open (medium)

- **Where:** STD-005 Shelf notes; `docs/standards/data/README.md`, Shelf decision trigger 2;
  `DRYRUN-QUESTIONS.md` Q24, Q25.
- **Problem:** Both files treat money handling as the only route to regulated data. The
  library's jurisdiction is unknown. Personal information of private people may fall under
  privacy law there. The Data Architect decision and the grade both lean on "not regulated".
- **Resolve:** Record the class as unknown until C5 names a jurisdiction. Add "jurisdiction
  decided" as a reopen trigger in the README decision and in the grade note.

### 8. Data Architect decision sits outside the chain (medium)

- **Where:** `docs/standards/data/README.md`, "Shelf decision at bootstrap"; Q23.
- **Problem:** The decision has no ID, author, challenger or approver. E10 ignores README files.
  `CLAUDE.md` session protocol 3 counts a decision only once it is in a named artifact. The
  section also fixes the D6 data-layer seats now: Architect authors, Builder challenges.
  Trigger 5 is "not met" only because the Builder is qualified. The Builder then builds the
  design it challenged.
- **Resolve:** Keep the reasoning, and list the decision under Open decisions in `STATE.md` for
  the Engineering Lead. Drop the seat assignment; the architecture's front matter decides it.
  State in trigger 5 that the only challenger is the implementer.

### 9. No `qualified_layers` narrowed (medium)

- **Where:** `docs/roles.yml`; `BOOTSTRAP.md` step 4; Q10.
- **Problem:** Step 4 says to narrow each seat's layers to what its holder can judge. The
  bootstrap narrowed none and only added. `STATE.md` still reports step 4 as done.
- **Resolve:** Narrow, or write one line per seat on why its full list stands, approved by the
  Engineering Lead. Report step 4 as partly done until then.

### 10. The author chose its own challenger and focus rows (medium)

- **Where:** `STATE.md`, Next item 1; Q2, Q5.
- **Problem:** The bootstrap names the seat that reviews it and tells it to focus on C6, C8, D6
  and D11. An author steering the challenge narrows it. D6 is a design-gate row, and no
  architecture exists to check it against.
- **Resolve:** The Engineering Lead or the Steward picks the challenger. `STATE.md` lists the
  files changed and leaves scope to the challenger.

### 11. Agent filled the human seats; step 2 unverified (medium)

- **Where:** `CLAUDE.md`, Seats; Q3; `BOOTSTRAP.md` steps 2 and 3.
- **Problem:** Step 3 belongs to the human. The agent filled both seats from a briefing. Branch
  protection and required checks (step 2) were "assumed done". Neither appears under Open
  decisions in `STATE.md`.
- **Resolve:** Add both to `STATE.md` Open decisions: the Engineering Lead confirms the seats
  and confirms step 2 against the repository settings.

### 12. Commits use the agent identity (medium)

- **Where:** `git log b6e52ab..HEAD` (author `Claude <noreply@anthropic.com>`); `BOOTSTRAP.md`
  step 2; Q4.
- **Problem:** Step 2 says agents commit under the human's GitHub identity until E1 ships. All
  four commits use another identity.
- **Resolve:** The Engineering Lead decides before merge: rewrite authorship, or amend step 2 to
  say the push, not the commit, carries the human's authorization.

### 13. Verifier carries almost every independent check (medium)

- **Where:** `docs/roles.yml`; `docs/dor/floor.yml`; Q14.
- **Problem:** Agent D now challenges the business case, the domain area, domain-core and
  STD-001 to STD-004, and judges C7, D7, D8, D12, I1 and I4 to I6. It also judges D7 and D8 on
  the domain core it challenges. Q14 says this was "accepted at the light grade". No one with
  authority accepted it.
- **Resolve:** Move at least the domain challenge off the Verifier (findings 2 and 3). Record
  the remaining load as an Engineering Lead decision.

### 14. STD-005 cannot be reviewed by this challenge (medium)

- **Where:** STD-005 front matter (`challenger_seat: Architect`);
  `tools/checks/check_artifacts.py` (review file `challenger_seat` must match the artifact's).
- **Problem:** `STATE.md` routes the whole bootstrap to the Verifier, but STD-005 names the
  Architect. E10 will refuse a Verifier review as STD-005's review file. None of the five
  standards has a review file. D11 cannot close on this challenge.
- **Resolve:** Each standard gets its own review file from its declared challenger. Or change
  STD-005's challenger, with a reason.

### 15. STD-003 carve-out cites the wrong clause (medium)

- **Where:** `docs/standards/data/STD-003-data-quality.md`, Applies to.
- **Problem:** The note exempts single-record form input because it is "validated at the server
  boundary (STD-002 clause 4)". STD-002 clause 4 is about producer and consumer tests against
  a contract. No clause requires server-boundary validation. Form input is Shelf's main way
  data enters, and the exemption rests on nothing.
- **Resolve:** Cite a real clause or add one, for example to STD-002, stating server-side
  validation of every form field against the schema.

### 16. `appliance.yml` header reads as a recorded approval (medium)

- **Where:** `appliance.yml`, line 2.
- **Problem:** "Filled at bootstrap (2026-10-01), approved by the Engineering Lead." With a date,
  this reads as a record. No approval has happened.
- **Resolve:** "Proposed at bootstrap (2026-10-01); awaiting Engineering Lead approval."

### 17. D11 child rows restate the parent (low)

- **Where:** `docs/dor/checklist.yml`, D11.1 to D11.5.
- **Problem:** Each child repeats D11 for one standard. None names its evidence path or
  approver. The file does not say whether D11 closes when all children close.
- **Resolve:** Add the standard's path as the expected evidence and state that D11 follows its
  children.

### 18. CODEOWNERS paths not tied to data folders (low)

- **Where:** `.github/CODEOWNERS`; `appliance.yml`, `data`.
- **Problem:** `/migrations/` and `/contracts/` are hard-coded. `appliance.yml` expects an ADR may
  move `migrations_dir`. Nothing keeps the two in step. The v0.2 example lines also omit data
  paths.
- **Resolve:** Add a comment in `appliance.yml` that a folder move updates CODEOWNERS in the
  same pull request.

### 19. Principles overlap; one is cited by number (low)

- **Where:** `CLAUDE.md`, Principles 1 and 4; STD-002 Shelf notes ("principle 3").
- **Problem:** Principles 1 and 4 both favour simple, cheap and easy to hand over, so they decide
  the same trade-offs. Principle 3 ranks reliable delivery above more channels, yet a second
  channel is a common way to make delivery reliable. STD-002 cites a principle by number, and
  the principles are drafts that may be renumbered.
- **Resolve:** Merge or separate 1 and 4. Say whether a fallback channel counts as reliability.
  Cite principles by name.

### 20. Repository still introduces itself as the template (low)

- **Where:** `README.md`; Q6.
- **Problem:** A new human or agent landing on the repository reads a template description, not
  Shelf.
- **Resolve:** One line at the top pointing to `CLAUDE.md` for Shelf.

### 21. STD-004 gives one dataset two approvers (low)

- **Where:** STD-004, clause 1 and Shelf notes.
- **Problem:** Clause 1 says the owner approves every change. The notes make Product the owner
  of the reminder schedule and add Operator approval for volume or cost changes.
- **Resolve:** Name one owner, and make the Operator a consulted seat on those changes.

### 22. STD-005 access log has no minimum (low)

- **Where:** STD-005, clause 5 note ("The log can be simple at the light grade, but it exists").
- **Problem:** "Simple" is undefined. A later session will guess what to record.
- **Resolve:** State the minimum: who, which member record, when, and through which screen.

## Repository guidance that made this challenge hard

1. No home for a bootstrap review. The review template and E10 assume one artifact with a
   `PREFIX-NNN` ID. The bootstrap spans many files and has no ID.
2. Two review rules collide. `BOOTSTRAP.md` step 5 asks one fresh session to review the whole
   output. E10 requires each standard's review file to name that standard's own challenger.
   STD-005 names the Architect, so one challenge cannot serve both.
3. Step 5 does not say who picks the challenger seat or the scope. The author filled the gap.
4. Session protocol step 1 says to read nothing outside the manifest. Step 5 has no manifest
   and needs the floor, roles, standards, checks, tests and the method document.
5. Step 5 says to review "against `docs/dor/floor.yml`", but most rows concern artifacts that do
   not exist yet. Nothing says which rows apply at bootstrap.
6. The review template has a Severity column with no scale, and no rule on which severities
   block approval. `open_findings` counts every finding the same.
7. The grade is defined only in `docs/method/appliance-design.md`. That document says the
   business case chooses the grade. `BOOTSTRAP.md` step 4 has the bootstrap set it.
8. "Qualified" is undefined for generic agent seats. No guidance says what evidence justifies
   narrowing a layer or claiming a domain, so a declaration cannot be tested.
9. The template's tests read the project's `docs/roles.yml` (`tests/conftest.py`,
   `CONFIG_FILES`). Recommending a seat change risks breaking the template's own tests, which
   biases both author and challenger.
10. Step 6 goes from challenge straight to merge. The constitution requires the author to
    answer findings item by item. No step or seat is named to do that for the bootstrap.
11. The bootstrap session had no seat (Q2), and seats are declared, not bound (E1). This
    session cannot show it differs from the author's seat and holder; it can only declare it.
