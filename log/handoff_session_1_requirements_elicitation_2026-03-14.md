# Handoff Note — Session 1: Requirements Elicitation (Canary Scope)
**Date:** 14 March 2026

---

## What Was Decided

The canary scope boundary was anchored: the Directory provides confidence to Local Loop operators that — on the basis of realistic ingested invoice data — an accurate, reliable, and robust directory of businesses/nodes is being built, and will be updated/maintained on the basis of future invoice data with minimal human input.

The "done" criterion was agreed: a data table showing the business directory, with the ability to input arbitrary but valid sets of invoice data and verify with minimal effort that all expected behaviour has occurred and no unexpected behaviour has occurred.

Known constraints were confirmed: PostgreSQL on Railway (relational schema) is confirmed and off the table. Invoice data format and entity resolution logic are explicitly open — to be addressed in requirements sessions. UI design is not covered by the functional prototype; all UI choices will be made with the coding agent.

Five requirements groups were identified, sequenced, and committed to the requirements map: (1) Invoice Ingestion, (2) Entity Resolution, (3) Directory State and Integrity, (4) Operator UI and Verification, (5) Non-Functional Requirements. Dependency chain runs sequentially 1 through 5.

## What Was Parked (and Why)

Detailed discussion of specific requirement decisions was deliberately parked. The team wants the requirements coaching sessions to be as open-ended as possible — pre-deciding answers here would constrain those sessions unnecessarily.

## What the Next Session Needs to Address

The requirements specification work will proceed as one session with five sub-sessions (Option A). The coaching agent produces a lightweight requirements brief for each group, reviews the returned artefacts for sufficiency and consistency before briefing the next group, and surfaces any issues requiring Tom and Dil's attention. The full session boundary checklist applies at the end of the whole session, not between sub-sessions.

The coaching agent is responsible for integrity and consistency analysis across all returned artefacts — Tom and Dil are relying on this monitoring function to catch gaps, contradictions, and propagation issues between groups.
