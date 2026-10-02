# Skills

Claude picks a skill automatically when your request matches it, so you just ask in plain words ("which invoices are overdue?"). Each skill calls the [invoiceninja-mcp](https://invoiceninja-mcp.marceltov.de/) tools rather than the InvoiceNinja API directly.

## Connection

- **`manage-invoiceninja-instances`**: Configures the connection to one or more InvoiceNinja instances: add, list or remove them, each registered with `claude mcp add`. Offered automatically by the `SessionStart` hook when no instance is configured.

## Clients

- **`find-clients`**: Looks up clients by name, email, number or balance.
- **`create-client`**: Creates a client with its contacts, after checking for duplicates.

## Invoices and payments

- **`find-invoices`**: Lists invoices filtered by client, status, overdue/payable, number or date range.
- **`create-invoice`**: Creates a draft invoice with line items for an existing client.
- **`send-invoice`**: Emails an invoice, sends a reminder, or marks it sent. Confirms first.
- **`record-payment`**: Records a payment against invoices, or marks an invoice paid. Confirms first.

## Expenses

- **`create-expense-from-receipt`**: Reads a receipt or supplier invoice (image or PDF) you give it and creates the expense, after you confirm the extracted details.

## Quotes and reports

- **`convert-quote-to-invoice`**: Approves a quote or converts it into an invoice.
- **`run-report`**: Runs A/R, client balance, sales, tax, profit and loss and list reports for a period.

For anything these skills don't cover, Claude can still call any of the server's tools, or its `apiRequest` escape hatch, directly.

The sources are in [`skills/`](https://github.com/Marceltov/invoiceninja-plugin/tree/main/skills), one `SKILL.md` per skill.
