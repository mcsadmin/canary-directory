# Requirements Brief: Entity Resolution

**Date:** 15 March 2026
**Issued to:** Tom and Dil
**Status:** Open

---

## Scope of This Group

This group covers how the system builds and maintains the business directory from ingested invoice data. It includes: how business nodes are created, how the system determines whether a name in an invoice refers to an existing node or a new one, the distinction between registered and inferred nodes, what "minimal human input" means in practice, and how the directory is updated as new invoice data arrives.

Out of scope for this session: the invoice ingestion mechanism itself (closed in Group 1), the full data model for a business node (Group 3), the operator UI (Group 4), and any non-functional requirements (Group 5). This session defines how raw invoice names become directory entries; subsequent sessions define what those entries look like and how users interact with them.

## Dependency Context

This group depends on Group 1 (Invoice Ingestion), which is now closed. The following decisions from Group 1 are relevant and must be respected:

**ADR-001 (Minimal Invoice Data Structure):** The invoice has four fields — `invoice_id` (UUID), `creditor_name`, `debtor_name`, `amount_due`. The `creditor_name` and `debtor_name` are the inputs to entity resolution. ADR-001 explicitly states: *"the entity resolution process will need to be robust enough to handle potentially ambiguous or inconsistent names."*

**ADR-004 (Single Creditor Per File):** Each CSV file contains invoices from a single creditor. This means the creditor identity is effectively known at file level, but debtor identities are only known at row level. The entity resolution process may need to treat these asymmetrically.

**ADR-003 (Valid Data Assumption):** All ingested data is assumed valid for the canary. This means entity resolution does not need to handle malformed names, but it does need to handle legitimate variations (e.g., "Acme Ltd" vs "Acme Limited" vs "ACME LTD").

**Files to read before starting:** `specs/adrs/ADR-001_invoice_data_structure.md`, `specs/adrs/ADR-003_valid_data_assumption.md`, `specs/adrs/ADR-004_single_creditor_per_file.md`, `specs/features/invoice_ingestion.feature`, `specs/data_dictionary/data_dictionary.md`.

## Questions to Answer

1. When a name appears in invoice data, how does the system decide whether it matches an existing business node or requires a new one? What is the matching logic for the canary?
2. What is the distinction between a registered node and an inferred node? How does a business become registered? Can an inferred node become registered, and if so, how?
3. What does "minimal human input" mean in practice? Is there an operator review step, an automated confidence threshold, or something else?
4. When the same business appears under different name variations across multiple invoice files, how is this resolved? Is deduplication automatic, operator-assisted, or deferred?
5. What happens to existing invoice records when two nodes are merged? Do the edges (invoices) update to point to the merged node?

## Expected Outputs

Gherkin scenarios covering: new node creation from invoice data, matching against existing nodes, the registered/inferred distinction, name variation handling, and any operator involvement in the resolution process. ADRs for any design decisions about matching logic, the registered/inferred model, and deduplication approach. Data dictionary entries for the Business Node entity (or updates to the existing dictionary).

## Sufficiency Test

This group is complete when: the matching logic is specified as testable scenarios, the registered/inferred distinction is defined and its transitions are explicit, "minimal human input" has a concrete meaning, name variation handling is addressed, and the relationship between entity resolution and existing invoice records is clear. If any of these are missing or ambiguous, the group remains open.
