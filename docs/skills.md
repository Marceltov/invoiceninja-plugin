# Skills

Claude picks a skill automatically when your request matches it, so you just ask in plain words ("which invoices are overdue?"). Each skill calls the [Invoice Ninja MCP server](https://invoiceninja-mcp.marceltov.de/) tools rather than the Invoice Ninja API directly.

## Connection

- **`invoiceninja-manage-instances`**: Configures the connection to one or more Invoice Ninja instances: add, list or remove them, each registered with `claude mcp add`. Offered once by the `SessionStart` hook, in the first session without a configured instance.

## Clients

- **`invoiceninja-find-clients`**: Looks up clients by name, email, number or balance.
- **`invoiceninja-create-client`**: Creates a client with its contacts, after checking for duplicates.

## Invoices and payments

- **`invoiceninja-find-invoices`**: Lists invoices filtered by client, status, overdue/payable, number or date range.
- **`invoiceninja-create-invoice`**: Creates a draft invoice with line items for an existing client.
- **`invoiceninja-send-invoice`**: Emails an invoice, sends a reminder, or marks it sent. Confirms first.
- **`invoiceninja-record-payment`**: Records a payment against invoices, or marks an invoice paid. Confirms first.

## Expenses

- **`invoiceninja-create-expense-from-receipt`**: Reads a receipt or supplier invoice (image or PDF) you give it and creates the expense with the file attached, after you confirm the extracted details. Needs a recent [Invoice Ninja MCP server](https://invoiceninja-mcp.marceltov.de/tools/) with its file upload route.

## Quotes and reports

- **`invoiceninja-convert-quote-to-invoice`**: Approves a quote or converts it into an invoice.
- **`invoiceninja-run-report`**: Runs A/R, client balance, sales, tax, profit and loss and list reports for a period.

For anything these skills don't cover, Claude can still call any of the server's tools, or its `apiRequest` escape hatch, directly.

The sources are in [`skills/`](https://github.com/Marceltov/invoiceninja-plugin/tree/main/skills), one `SKILL.md` per skill.
