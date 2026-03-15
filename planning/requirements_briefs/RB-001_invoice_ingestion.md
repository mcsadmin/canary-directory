# Requirements Brief: Invoice Ingestion

**Date:** 14 March 2026
**Issued to:** Tom and Dil
**Status:** Open

---

## Scope of This Group

This group covers how the system accepts invoice data — the input format, validation rules, what constitutes a valid invoice, and what happens to invalid data. It is the entry point for all data that feeds the Directory: nothing enters the invoice graph without passing through this layer.

Out of scope for this session: how the system resolves ingested data into business nodes (that is Group 2: Entity Resolution), the data model for the business directory itself (Group 3), and any UI concerns (Group 4). This session defines what goes in; subsequent sessions define what the system does with it.

## Dependency Context

This is the first group in the sequence — it has no dependencies on other groups. However, every subsequent group depends on the decisions made here, particularly the invoice data format and the validation rules. The requirements coaching session should be aware that its outputs will be the foundation for Entity Resolution (Group 2).

**Known constraint:** PostgreSQL on Railway, relational schema — confirmed and off the table. The invoice data format and structure are explicitly open and should be addressed in this session.

## Questions to Answer

1. What does an invoice look like in this system? What fields does it contain, and which are mandatory versus optional?
2. How does invoice data enter the system? What is the ingestion mechanism for the canary (bearing in mind the done criterion: "input arbitrary but valid sets of invoice data")?
3. What makes an invoice valid? What are the validation rules, and at what point is validation applied?
4. What happens when invalid data is submitted? Is it rejected, quarantined, partially accepted, or something else?
5. Is there a distinction between a single invoice and a batch of invoices? If so, how does batch behaviour differ from single-invoice behaviour?

## Expected Outputs

The requirements coaching session should produce Gherkin scenarios covering the behaviours described above, and any ADRs required to record design decisions about the invoice data format and ingestion mechanism. A data dictionary entry for the invoice entity is expected.

## Sufficiency Test

This group is complete when: the invoice data format is defined (fields, types, mandatory/optional), the validation rules are specified as testable scenarios, the behaviour on invalid data is explicit, and the ingestion mechanism is described clearly enough that a coding agent could implement it. If any of these are missing or ambiguous, the group remains open.
