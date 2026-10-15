# Skills

This directory holds the plugin's skills, one per subdirectory:

```
skills/
  <skill-name>/
    SKILL.md
```

- **`invoiceninja-manage-instances`** — configures the connection to one or more Invoice Ninja instances by asking the user and registering each with `claude mcp add`. Offered once per installation by the `SessionStart` hook in `hooks/`, in the first session without a configured instance.
- **`invoiceninja-find-clients`** — looks up clients by name, email, number or balance.
- **`invoiceninja-create-client`** — creates a client with its contacts.
- **`invoiceninja-find-invoices`** — lists invoices filtered by client, status, overdue/payable, number or date range.
- **`invoiceninja-create-invoice`** — creates an invoice (line items, taxes, due date) for an existing client.
- **`invoiceninja-send-invoice`** — emails an invoice, or marks it sent without emailing.
- **`invoiceninja-record-payment`** — records a payment against invoices, or marks an invoice paid.
- **`invoiceninja-convert-quote-to-invoice`** — approves/converts a quote into an invoice.
- **`invoiceninja-create-expense-from-receipt`** — reads a receipt or supplier invoice (image or PDF) and creates the expense from it, with the file attached.
- **`invoiceninja-run-report`** — runs one of Invoice Ninja's reports (A/R, sales, tax, profit/loss, …).

See the [Claude Code plugin docs](https://code.claude.com/docs/en/plugins) for the `SKILL.md` format, or the `superpowers:writing-skills` skill for a guided walkthrough.

Skills here should call the MCP tools exposed by [Invoice Ninja MCP server](https://github.com/Marceltov/invoiceninja-mcp) (`getClients`, `storeInvoice`, `bulkInvoices`, …) rather than talking to the Invoice Ninja API directly. For anything a generated tool can't express (a field missing from the OpenAPI spec, an endpoint it doesn't cover), use the server's `apiRequest` tool — see [Tools](https://invoiceninja-mcp.marceltov.de/tools/).

## Writing skills here

- Names start with `invoiceninja-`: on claude.ai, skills are called without the plugin name, so the prefix keeps them apart from other plugins' skills.
- The rules every skill shares (which instance to use, connectors) live in [`shared/rules.md`](../shared/rules.md). Edit only that file and run `scripts/sync-shared.sh`; the Checks workflow fails when a copy in `skills/*/reference/` is out of date. Skills link to their own copy as `reference/shared-rules.md`, so every skill folder is complete on its own.
- Write for both Claude Code and claude.ai. Where they differ, phrase it as a condition ("if you have a shell", "if you can start subagents") instead of assuming Claude Code.
