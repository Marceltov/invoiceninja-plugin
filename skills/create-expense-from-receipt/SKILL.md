---
name: create-expense-from-receipt
description: Create an InvoiceNinja expense from a receipt or invoice the user provides as an image or PDF, by reading it and calling storeExpense. Use when the user passes or points to a photo/scan/PDF of a receipt, bill or supplier invoice and asks to book, log, add or create an expense from it.
---

# Create an InvoiceNinja Expense from a Receipt

Read the file yourself (the Read tool handles images and PDFs), extract the details, and create the expense. Never invent values: anything the document doesn't show is left out or asked.

If more than one InvoiceNinja instance is connected this session, resolve which one to use first — see [Working with multiple instances](../README.md#working-with-multiple-instances).

## Steps

1. **Read the document.** Get the file path from the user (ask if they only described it). Read it and extract: vendor name, document date, total amount, currency, tax amount and rate (if shown), invoice/receipt number, payment method, and a short description of what was bought. Several receipts in one file or several files → handle one expense per receipt.
2. **Resolve the vendor.** `getVendors` with `filter` set to the vendor name. One clear match → use its `id`. None → offer to create it with `storeVendor` (name only) or leave the expense without a vendor. Several → ask.
3. **Resolve the category.** `getExpenseCategorys` and pick the one that clearly fits the purchase; if none does, or it's ambiguous, show the list and ask. Don't create categories unprompted.
4. **Currency.** If the receipt's currency differs from the account default, find its `currency_id` (`getStatics`/`apiRequest` `GET /api/v1/statics` → `currencies`) and set `currency_id`; otherwise omit it.
5. **Show what you extracted** as a short list (vendor, date, amount, tax, category, reference, notes) and flag anything uncertain — blurry digits, ambiguous date formats (`03/04`), net vs gross totals. Ask the user to confirm or correct before writing.
6. **Create it** with `storeExpense`: `{ "amount": <gross total>, "date": "YYYY-MM-DD", "vendor_id": ..., "category_id": ..., "currency_id": ..., "transaction_reference": <receipt/invoice no.>, "public_notes": <description>, "private_notes": "Created from <file name>" }`. For tax shown on the receipt, add `tax_name1`/`tax_rate1` (e.g. `VAT`, `19`) and, if the amount is gross, `uses_inclusive_taxes: true`. Add `payment_date` and `payment_type_id` only when the receipt shows it was already paid and you know the type id. Add `client_id` + `should_be_invoiced: true` only if the user said to bill it on.
7. **Read it back** with `showExpense` and check the amount, date, vendor and category landed; undeclared fields are silently dropped, so redo any missing one via `apiRequest` `PUT /api/v1/expenses/{id}`. Report the expense id and the booked values.

## Limits

- **The receipt file is not attached.** InvoiceNinja's document upload for expenses (`/expenses/{id}/upload`) needs multipart and isn't fixed in invoiceninja-mcp yet (only `uploadClient` is), so tell the user to attach the file in the InvoiceNinja UI if they want it stored. The original file name goes into `private_notes` so it can be found.
- **Duplicates:** before creating, `getExpenses` with `filter` set to the receipt number (or vendor + amount) and warn if the same expense seems to exist already.
