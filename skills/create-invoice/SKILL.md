---
name: create-invoice
description: Create a new InvoiceNinja invoice with line items for an existing client via the storeInvoice MCP tool. Use when the user asks to create, draft, write or bill an invoice.
---

# Create an InvoiceNinja Invoice

If more than one InvoiceNinja instance is connected this session, resolve which one to use first — see [Working with multiple instances](../README.md#working-with-multiple-instances).

## Steps

1. **Resolve the client** to a `client_id` with `getClients` (see `find-clients`). If it doesn't exist, offer `create-client` first.
2. **Collect line items**: for each, `notes` (description), `quantity`, `cost` (unit price), optionally `product_key` and `tax_name1`/`tax_rate1`. To bill a catalog product, look it up with `getProducts` (`filter`) and reuse its key and cost. Optional invoice-level fields: `date`, `due_date`, `po_number`, `public_notes`, `terms`, `discount`. Ask only for what's missing; never invent prices or quantities.
3. **Confirm the draft** with the user: client, items, computed total.
4. **Call `storeInvoice`** with `{ "client_id": ..., "line_items": [{ "notes": ..., "quantity": ..., "cost": ... }], ... }`. It creates a **draft** (not sent to the client).
5. **Read it back** with `showInvoice` and check the number, line items and amount match; undeclared fields are silently dropped, so redo any missing one via `apiRequest` `PUT /api/v1/invoices/{id}`.
6. **Report** invoice number, id and total, and offer `send-invoice`.
