---
name: record-payment
description: Record a payment against InvoiceNinja invoices via the storePayment MCP tool, or mark an invoice paid via bulkInvoices. Use when the user says a client paid, asks to record, log or apply a payment, or to mark an invoice as paid.
---

# Record an InvoiceNinja Payment

This changes balances and may email a receipt to the client, so confirm first.

If more than one InvoiceNinja instance is connected this session, resolve which one to use first — see [Working with multiple instances](../README.md#working-with-multiple-instances).

## Steps

1. **Resolve the invoice(s)** via `getInvoices` (see `find-invoices`) and read each with `showInvoice` to get `client_id` and the open `balance`.
2. **Full payment of a single invoice, no extra details**: confirm, then call `bulkInvoices` with `{ "ids": ["<id>"], "action": "mark_paid" }`.
3. **Anything else** (partial amount, payment date, reference, several invoices): collect `amount`, `date` (default today), `transaction_reference`, `type_id` if known, then confirm and call `storePayment` with `{ "client_id": ..., "amount": ..., "date": ..., "transaction_reference": ..., "invoices": [{ "invoice_id": "<id>", "amount": <applied> }] }`. The applied amounts must not exceed each invoice's balance. Pass `email_receipt=true` only if the user wants the client to get a receipt.
4. **Verify** with `showInvoice`: balance reduced and status `3` (partial) or `4` (paid). Report the payment id and the new balance.

Refunds (`storeRefund`) and deleting a payment are out of scope here; confirm explicitly with the user before doing either.
