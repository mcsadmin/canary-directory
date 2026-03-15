# Requirements Map — Canary (Directory / Entity Resolution)

**Last updated:** 14 March 2026

---

## Requirements Groups

| # | Group | Status | Brief | Dependencies | Assigned To | Notes |
|---|---|---|---|---|---|---|
| 1 | Invoice Ingestion | Closed | RB-001 | None | Tom and Dil | ADR-001, ADR-002, ADR-003, ADR-004. Sufficiency confirmed 15 March 2026. |
| 2 | Entity Resolution | Open | RB-002 | Invoice Ingestion | Tom and Dil | How the system builds and maintains the directory — businesses/nodes created via platform user registration *and* via invoice ingestion and analysis; deduplication, merging, updating; what “minimal human input” means in practice; the registered/inferred node distinction |
| 3 | Directory State and Integrity | Not Yet Briefed | — | Entity Resolution | — | The data model for a business node; integrity constraints the system must enforce; what "accurate, reliable, and robust" means as testable properties |
| 4 | Operator UI and Verification | Not Yet Briefed | — | Directory State and Integrity | — | The data table view; the invoice input mechanism; legibility of expected and unexpected behaviour; the "verify with minimal effort" requirement |
| 5 | Non-Functional Requirements | Not Yet Briefed | — | All of the above | — | Minimal NFR set for the canary — performance, reliability, and any other non-functional constraints the team considers necessary |

---

## Sequencing

Groups are sequenced by dependency: 1 → 2 → 3 → 4 → 5. Each group must reach `Closed` status before the coding agent handoff can proceed.

**Status values:** `Not Yet Briefed` / `Open` (brief issued, session in progress) / `Returned` (artefacts received, assessment in progress) / `Closed` (sufficiency confirmed by Tom and Dil).

---

## Context

**Canary scope anchor (Q1):** Provides confidence to Local Loop operators that — on the basis of realistic ingested invoice data — an accurate, reliable, and robust directory of businesses/nodes is being built, and will be updated/maintained on the basis of future invoice data with minimal human input.

**Done criterion (Q2):** See: business directory as a data table. Do: input arbitrary but valid sets of invoice data and verify with minimal effort that all of the expected behaviour has occurred, and no unexpected behaviour has occurred.

**Known constraints:**
- PostgreSQL on Railway, relational schema — confirmed and off the table.
- Invoice data format, entity resolution logic — explicitly open, to be addressed in requirements sessions.
- UI design — not covered by the functional prototype; all UI choices made with the coding agent.
