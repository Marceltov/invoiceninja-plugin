---
name: send-invoice
description: Email an InvoiceNinja invoice to the client, or mark it sent without emailing, via the bulkInvoices MCP tool. Use when the user asks to send, email, deliver or mark-as-sent an invoice, or to send a payment reminder.
---

# Send an InvoiceNinja Invoice

Emailing is customer-visible and can't be recalled, so always confirm first.

If more than one InvoiceNinja instance is connected this session, resolve which one to use first — see [Working with multiple instances](../README.md#working-with-multiple-instances).

## Steps

1. **Resolve the invoice** to an id with `getInvoices` (`number` or `client_id`; see `find-invoices`). If several match, list them and ask.
2. **Check it is sendable** with `showInvoice` (`include=client`): the client needs a contact with an email address. If not, say so and stop (or offer to add one via `updateClient`).
3. **Confirm** with the user, naming invoice number, client, recipient email and amount.
4. **Call `bulkInvoices`** with `{ "ids": ["<id>"], "action": "email" }`. For a reminder use `"action": "send_email"` with `"email_type": "reminder1"` (or `reminder2`, `reminder3`). To only mark it sent without emailing, use `"action": "mark_sent"`.
5. **Verify** with `showInvoice`: status should now be sent (`status_id` 2). Report it.
