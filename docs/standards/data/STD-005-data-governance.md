---
id: STD-005
type: standard
title: Data governance
status: draft
author_seat: Operator
challenger_seat: Architect
approver: Engineering Lead
parents: []
approved_at: null
---

# Data governance

- **Kind:** craft standard
- **Owning seat:** Operator, who owns the classification rule (floor row C5)
- **Applies to:** all data the system stores, processes or exports. For Shelf: member
  contact details, tool listings, loan history, and reminder delivery records

Adapted for Shelf at bootstrap (2026-10-01). Still a draft: a challenger reviews it and
the Engineering Lead approves it (floor row D11). A rejection must cite a clause number.

## Clauses

1. **Classification drives handling.** Every dataset carries a class from the risk
   assessment: public, internal, confidential or regulated. Each class has a written rule
   for access, encryption at rest and in transit, and export.
2. **Retention is a schedule, not a habit.** Each class has a retention period that meets
   the NFR in floor row C4. Data past its period is deleted or anonymised by a scheduled job.
3. **Deletion is tested.** A user or legal deletion request removes the data from every
   store that holds it, including derived data and backups within their stated window.
   The deletion path has its own test.
4. **Lineage is recorded.** Every derived dataset names its inputs, so the effect of a
   bad input or a deletion can be traced forward.
5. **Access to confidential and regulated data is logged.** The log records who, what,
   when and why, and is kept for the audit period.
6. **Agents get least privilege.** No agent seat holds credentials to production data it
   does not need for its charter.

## Shelf notes

Proposed classes, to be confirmed by the risk assessment (floor row C5):

| Data | Proposed class | Why |
| --- | --- | --- |
| Member name, email, phone, address | confidential | Personal information of private people |
| Loan history (who borrowed what, when) | confidential | Shows a member's presence at home and what they own |
| Tool listings | internal | Visible to members, not to the public |
| Reminder delivery records | confidential | Carry contact details |

No data is classed regulated unless discovery adds money handling, such as deposits or
late fees. If it does, the grade and the Data Architect decision are reopened.

- Clause 3: a member who leaves is deleted or anonymised. A loan still out when the member
  leaves is resolved by the coordinator first. The retention period comes from the NFR.
- Clause 5: the coordinator view of member contact details is access to confidential data,
  so it is logged. The log can be simple at the light grade, but it exists.
