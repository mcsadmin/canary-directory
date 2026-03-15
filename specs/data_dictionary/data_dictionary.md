# Data Dictionary

This document is the authoritative reference for all data entities and attributes in the system. It is maintained by the requirements coaching agent and is always consistent with the accepted ADRs.

---

## Invoice Entity

**Source ADR:** [ADR-001: Minimal Invoice Data Structure for Canary](ADR-001_invoice_data_structure.md)

| Attribute | Data Type | Description |
|---|---|---|
| `invoice_id` | UUID | A UUID assigned by the system upon ingestion. Guarantees every invoice can be uniquely referenced, including across multiple uploads. |
| `creditor_name` | string | The name of the creditor (supplier) as it appears on the invoice. |
| `debtor_name` | string | The name of the debtor (customer) as it appears on the invoice. |
| `amount_due` | number | The amount outstanding at the present time. |
