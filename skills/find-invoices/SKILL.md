---
name: find-invoices
description: List or search InvoiceNinja invoices by client, status, overdue/payable, number, text or date range using the getInvoices MCP tool. Use when the user asks which invoices are open, unpaid, overdue, paid, or asks to find an invoice by number or client.
---

# Find InvoiceNinja Invoices

A thin wrapper around `getInvoices`.

If more than one InvoiceNinja instance is connected this session, resolve which one to use first — see [Working with multiple instances](../README.md#working-with-multiple-instances).

## Filters

- `client_id` — one client's invoices (resolve a name with `find-clients` first).
- `client_status` — comma-separated `paid`, `unpaid`, `overdue`.
- `overdue=true` / `payable=<client_id>` — overdue invoices / invoices that can still be paid.
- `status_id` — `1` draft, `2` sent, `3` partial, `4` paid, `5` cancelled, `6` reversed.
- `number` — exact invoice number; `filter` — free-text search.
- `date` / `date_range` — by invoice date.
- `status` — record state: `active`, `archived`, `deleted` (default is active only).
- `include=client` to get the client name in each row; `per_page` / `page` to paginate (default 20 per page).

## Steps

1. Map the request to the simplest filter set above. "Unpaid" means `client_status=unpaid,overdue`; "open" means `status_id=2,3`.
2. Call `getInvoices`. If the result is a full page, say there may be more and offer the next page.
3. Present a compact table: number, client, date, due date, amount, balance, status — not the raw JSON. For "how much is outstanding", sum the `balance` of what you fetched and say how many invoices that covers.
