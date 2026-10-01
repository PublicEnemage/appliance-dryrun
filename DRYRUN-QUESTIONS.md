# Dry-run questions

Logged by the bootstrap session (BOOTSTRAP.md step 4), 2026-10-01. No human was available.
Each entry gives the question, where it came from, and the assumption made to continue.

## Session and process

### Q1. What is the bootstrap session's input manifest?
- **Source:** `CLAUDE.md`, Session protocol step 1 ("Read ... the input manifest of your task.
  Read nothing else unless the manifest names it"); floor row I7.
- **Assumption:** No manifest exists for bootstrap. BOOTSTRAP.md step 4 and the files it names
  served as the manifest. To do the step safely I also read files those point to:
  `docs/dor/floor.yml`, `docs/artifact-types.yml`, `docs/enforcement.yml`, the templates for
  standard, review, role proposal, business case, intent and architecture, every check under
  `tools/checks/`, the tests, and the "Grades of rigor" section of
  `docs/method/appliance-design.md`. A bootstrap manifest in BOOTSTRAP.md would remove the
  conflict between "read nothing else" and "follow what these files point to".

### Q2. Which seat does the bootstrap session hold?
- **Source:** `BOOTSTRAP.md` step 5 ("A fresh session, a different seat, reviews the bootstrap
  output"); `CLAUDE.md`, The chain (author, challenger, approver on different seats).
- **Assumption:** None is named, so I acted as an unseated bootstrap session. I edited the five
  data standards, whose front matter names Architect (STD-001 to STD-004) and Operator
  (STD-005) as author. The challenger in step 5 should be a seat other than Architect and
  Operator, and on a holder other than Agent A and Agent B. Verifier (Agent D) fits both.
  Step 4 should name the bootstrap seat.

### Q3. Were steps 1 to 3 done before this session?
- **Source:** `BOOTSTRAP.md` steps 1 to 3.
- **Assumption:** `floor_version` is pinned at 0.1.0 (step 1, done in the template). Branch
  protection and required checks (step 2) cannot be seen from the repository; assumed done.
  Step 3 (the human names the seats in `CLAUDE.md`) was not done; I filled both seats with
  PublicEnemage from the session briefing.

### Q4. Under which git identity should this session commit?
- **Source:** `BOOTSTRAP.md` step 2 ("agents commit under the human's GitHub identity");
  `CLAUDE.md`, Governance.
- **Assumption:** The local git identity is `Claude <noreply@anthropic.com>`, not
  PublicEnemage. I did not change git configuration. Commits use the configured identity.
  The push, done by the human, carries his authorization. Confirm whether commit authorship
  must also be the human's.

### Q5. What can the step 5 challenger check against the floor?
- **Source:** `BOOTSTRAP.md` step 5; `STATE.md`, Next.
- **Assumption:** Most floor rows concern artifacts that do not exist yet. The rows the bootstrap
  touches are C6 (grade), C8 (domain seats), D6 (layer charters) and D11 (data standards).
  STATE.md names these for the challenger.

### Q6. Should the template's own material stay in the project?
- **Source:** `README.md` (describes the template, not Shelf); `docs/method/`.
- **Assumption:** Step 4 does not list them, so I left both unchanged. A later session may
  rewrite README.md for Shelf.

## Constitution

### Q7. Is the mission right?
- **Source:** `CLAUDE.md`, Mission.
- **Assumption:** Written from the briefing only: lend, borrow, return, reminders, and a
  coordinator who sees what is out and overdue. "A volunteer coordinator" may be one person
  or several; the wording allows either. The Intent Owner should confirm.

### Q8. Are the four principles the Intent Owner's?
- **Source:** `CLAUDE.md`, Principles ("Three to five project principles").
- **Assumption:** Nothing was decided, so all four are inventions: volunteer time first, least
  personal data, reliable reminders, cheap and easy to hand over. Each will steer later
  trade-offs (stack, reminder channel, data collected), so each needs explicit approval. I
  marked them as drafts in the constitution's opening note.

### Q9. May the bootstrap rewrite the "Slots ... are filled at bootstrap" sentence?
- **Source:** `CLAUDE.md`, opening paragraph.
- **Assumption:** Yes. With slots filled, the sentence was stale. It now says the slots were
  filled at bootstrap and that mission and principles are drafts until approved.

## Roles

### Q10. On what basis are agent seats narrowed?
- **Source:** `BOOTSTRAP.md` step 4 ("narrow each seat's qualified_layers to what its holder can
  judge"); `docs/roles.yml` header.
- **Assumption:** The holders are generic agent sessions. Nothing in the repository says what
  any of them can or cannot judge, and Shelf uses every layer. I narrowed nothing and only
  added domain-core. Guidance on what evidence justifies narrowing would help.

### Q11. The template's tests read the project's roles.yml. Is that intended?
- **Source:** `tests/conftest.py` (`CONFIG_FILES` copies `docs/roles.yml` and `appliance.yml`
  into each test repo); `tests/test_check_seats.py`
  (`test_refuses_unqualified_layer_seat_and_points_to_crew_review` asserts that the Architect
  is not qualified for domain-core; `good_layers` relies on the template charters).
- **Assumption:** A project that adds domain-core to the Architect, or narrows Architect,
  Builder, Operator or Verifier, breaks the template's own tests. This partly drove my choice
  of Product and Verifier for domain-core over the Architect. The tests should probably carry
  their own fixture roster.

### Q12. Who should hold domain-core for Shelf?
- **Source:** `BOOTSTRAP.md` step 4 ("name the seat for domain-core"); `docs/roles.yml`.
- **Assumption:** Shelf's domain core is the loan lifecycle and reminder timing. Product authors
  it from the use cases; Verifier challenges it, which fits D7 and D8 (failure modes, state
  diagrams). Both are on different holders (Agent A, Agent D). The Architect would be a
  natural author but see Q11. Neither seat has real tool-library experience.

### Q13. Which domain areas, and with what counter-perspective?
- **Source:** `BOOTSTRAP.md` step 4 ("any qualified_domains"); floor row C8 ("with a
  counter-perspective where judgment is contested").
- **Assumption:** I named one area, `tool-lending-operations`, challenged by the Verifier, because
  the business case template has Product author and Verifier challenge it. The business case
  must use this exact string. No seat gives a counter-perspective; I pointed to the
  coordinator's input as discovery evidence (C9) instead. If loan rules turn out contested
  (deposits, damage, bans), a crew review may be needed.

### Q14. Is too much judgment concentrated on the Verifier?
- **Source:** `docs/dor/floor.yml` (Verifier judges C7, D7, D8, D12, I1, I4 to I6);
  `docs/roles.yml`.
- **Assumption:** Adding domain-core and the domain area to the Verifier adds load on Agent D.
  Accepted at the light grade; flagged for the challenger.

## appliance.yml

### Q15. Light or standard grade?
- **Source:** `appliance.yml` (`grade`); `docs/method/appliance-design.md`, Grades of rigor
  ("Standard: default for anything with users"; "A small venture testing an idea should start
  Light"); floor row C6.
- **Assumption:** Light, because Shelf is a small idea under test. Shelf does have real users and
  personal data, so I added a note to raise it to standard before real members' data is stored
  in production. The Intent Owner must approve this in the business case (C6).

### Q16. Which source governs the grade: appliance.yml or the business case?
- **Source:** `BOOTSTRAP.md` step 4 (bootstrap sets the grade); `appliance-design.md` ("The grade
  is chosen in the business case"); floor row C6.
- **Assumption:** The business case decides; `appliance.yml` mirrors it. Bootstrap sets a
  proposal. No check compares the two.

### Q17. Merge autonomy?
- **Source:** `appliance.yml`, `autonomy`.
- **Assumption:** Kept `human` for main and lanes. With a single principal and no independent
  review, the merge is the only human check on agent work.

### Q18. Where do contracts and migrations live, given no stack is chosen?
- **Source:** `BOOTSTRAP.md` step 4; `appliance.yml`, `data`.
- **Assumption:** Kept the defaults `contracts` and `migrations` and did not create either
  folder. Many frameworks keep migrations in their own place; the stack ADR may need to move
  `migrations_dir` before the first migration merges. Moving it after a merge would hide old
  migrations from E15.

## CODEOWNERS

### Q19. Was the handle meant to be in the template already?
- **Source:** `.github/CODEOWNERS` ("Bootstrap replaces the handle below").
- **Assumption:** The template already carried @PublicEnemage, which matches this project. I kept
  it and added explicit lines for `/docs/standards/`, `/contracts/` and `/migrations/`. A
  template for other owners should carry a placeholder.

## Data standards

### Q20. Adapt or mark not-applicable?
- **Source:** `BOOTSTRAP.md` step 4; `docs/standards/data/README.md`; floor row D11.
- **Assumption:** I adapted all five. STD-003 was borderline; I kept it for bulk imports, provider
  delivery results and possible photo uploads.

### Q21. Who fills `approver` on a not-applicable row at bootstrap?
- **Source:** `docs/dor/checklist.yml` header ("not-applicable -> reason and approver");
  `tools/checks/check_dor.py`.
- **Assumption:** The check accepts any non-empty approver. Writing "Engineering Lead" before he
  signs would be a false declaration. This was one reason not to mark any standard
  not-applicable. The process should say whether a bootstrap may write a pending approver.

### Q22. Is one child row per standard under D11 acceptable?
- **Source:** `docs/dor/checklist.yml`; floor row D11.
- **Assumption:** Yes. I added D11.1 to D11.5, one per standard, all open. They are finer than D11
  and pass the DOR check.

### Q23. Where is the Data Architect decision recorded?
- **Source:** `BOOTSTRAP.md` step 4 ("Decide whether a Data Architect seat is needed now");
  `CLAUDE.md` ("A decision counts only once it is written to a named artifact").
- **Assumption:** An ADR is the obvious home, but an ADR is not a root type in
  `docs/artifact-types.yml` and nothing upstream exists yet, so E10 would refuse it. I recorded
  the decision, trigger by trigger, in `docs/standards/data/README.md`. Decision: no Data
  Architect seat now.

### Q24. Which privacy law applies?
- **Source:** `docs/standards/data/STD-005-data-governance.md` (retention, deletion).
- **Assumption:** The library's location is not decided, so no law is named. Retention periods are
  left to the NFR (C4) and the risk assessment (C5).

### Q25. Does Shelf handle money?
- **Source:** STD-005 notes; `docs/standards/data/README.md` trigger 2; `appliance-design.md`
  ("Assured: ... handling money").
- **Assumption:** No. Deposits or late fees would raise the grade question, may make data
  regulated, and would reopen the Data Architect decision. Recorded as a reopen condition.

### Q26. Email, SMS, or both for reminders?
- **Source:** STD-002 and STD-003 notes.
- **Assumption:** Undecided. The standards say "email or SMS provider". The choice affects cost
  (principle 4) and the personal data held (principle 2).

### Q27. Who owns Shelf's reference data?
- **Source:** STD-004 clause 1.
- **Assumption:** Proposed in STD-004 notes: Product owns tool categories, the default loan period
  and the reminder schedule; Operator approves changes that alter send volume or cost. To be
  confirmed when the architecture names the datasets.

### Q28. Are the proposed data classes right?
- **Source:** STD-005 clause 1; floor row C5.
- **Assumption:** Contact details, loan history and delivery records are confidential; tool
  listings are internal; nothing is regulated. Loan history is classed confidential because it
  shows what a member owns and when they are home. The risk assessment confirms.

### Q29. Should STD-001 clause 3 be relaxed for a single-instance app?
- **Source:** STD-001 clause 3 (expand, migrate, contract).
- **Assumption:** No. I kept it whole and noted why it still matters with one instance. If the
  availability NFR allows downtime, a later ADR may revisit it.

## Checks

### Q30. Does E15 fail once a migrations folder exists locally?
- **Source:** `tools/checks/check_migrations.py`.
- **Assumption:** It needs `origin/main` resolvable. In this checkout it is, and CI fetches full
  history. A clone without the remote ref will fail E15 as soon as `migrations/` exists. No
  action taken; noted for whoever creates the folder.
