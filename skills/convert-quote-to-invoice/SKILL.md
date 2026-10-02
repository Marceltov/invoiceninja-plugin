---
name: convert-quote-to-invoice
description: Approve an InvoiceNinja quote or convert it into an invoice via the bulkQuotes MCP tool. Use when the user asks to convert, turn, promote or approve a quote, or to invoice an accepted quote.
---

# Convert an InvoiceNinja Quote to an Invoice

If more than one InvoiceNinja instance is connected this session, resolve which one to use first — see [Working with multiple instances](../README.md#working-with-multiple-instances).

## Steps

1. **Resolve the quote** with `getQuotes` (`number`, `client_id` or `filter`) and read it with `showQuote`. Report number, client and total. If it is already converted (it has an `invoice_id`), say so and stop.
2. **Confirm** with the user which quote is being converted. Ask whether it should also be approved first (`"action": "approve"`) — only if they say it was accepted.
3. **Call `bulkQuotes`** with `{ "ids": ["<quote id>"], "action": "convert" }`.
4. **Find the new invoice**: `showQuote` now carries its `invoice_id`; read it with `showInvoice` and report number, id and total. The invoice is a draft — offer `send-invoice`.
