# Session Summary: RB-001 Invoice Ingestion

**Date:** 15 March 2026

This document summarises the outputs of the requirements coaching session for the Invoice Ingestion group (RB-001).

---

### 1. Material Processed

- Requirements Brief RB-001: Invoice Ingestion

### 2. Gherkin Scenarios Produced

- **File:** `invoice_ingestion.feature`
- **Scenario 1:** A creditor submits a single invoice.
- **Scenario 2:** A creditor submits multiple invoices to multiple debtors.
- **Scenario 3:** A creditor submits multiple invoices to the same debtor.
- **Scenario 4:** A creditor submits an empty CSV file.
- **Scenario 5:** A creditor submits an invoice with a zero amount due.

### 3. Architecture Decision Records (ADRs) Produced

- **ADR-001:** Minimal Invoice Data Structure for Canary
- **ADR-002:** CSV Ingestion for Invoice Data
- **ADR-003:** Assumption of Valid Ingested Data for Canary
- **ADR-004:** Each CSV File Represents a Single Creditor

### 4. Data Dictionary Updates

- The **Invoice Entity** has been added to the Data Dictionary, defining the attributes `invoice_id`, `creditor_name`, `debtor_name`, and `amount_due`.

### 5. Acceptance Criteria Clarified

The Gherkin scenario in `invoice_ingestion.feature` serves as the primary, testable acceptance criterion for this requirements group.

### 6. Out of Scope (Parking Lot)

- **Data Validation and Error Handling:** Explicitly deferred to a post-canary phase. The system will assume all ingested data is valid. This is formally captured in **ADR-003**.
- **Multi-creditor batch files:** Out of scope for the canary. Each CSV file will contain invoices from a single creditor only. This is formally captured in **ADR-004**.

### 7. Open / Unresolved Items

- None. All questions in the requirements brief for RB-001 have been addressed and resolved for the canary scope.

### 8. Integrity Mandate Confirmation

All artefacts produced during this session (ADRs, Data Dictionary, Gherkin scenarios) are internally consistent. The Integrity Mandate has been applied throughout the session. The term "obligation" has been removed from all artefacts and replaced with "invoice" to restrict the canary scope explicitly to invoices.

---

**Status: Closed** — confirmed by Tom and Dil, 15 March 2026.
