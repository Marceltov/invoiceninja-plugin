# Skills

This directory holds the plugin's skills, one per subdirectory:

```
skills/
  <skill-name>/
    SKILL.md
```

- **`manage-invoiceninja-instances`** — configures the connection to one or more InvoiceNinja instances by asking the user and registering each with `claude mcp add`. Triggered automatically by the `SessionStart` hook in `hooks/` when no instance is configured.
- **`find-clients`** — looks up clients by name, email, number or balance.
- **`create-client`** — creates a client with its contacts.
- **`find-invoices`** — lists invoices filtered by client, status, overdue/payable, number or date range.
- **`create-invoice`** — creates an invoice (line items, taxes, due date) for an existing client.
- **`send-invoice`** — emails an invoice, or marks it sent without emailing.
- **`record-payment`** — records a payment against invoices, or marks an invoice paid.
- **`convert-quote-to-invoice`** — approves/converts a quote into an invoice.
- **`create-expense-from-receipt`** — reads a receipt or supplier invoice (image or PDF) and creates the expense from it, with the file attached.
- **`run-report`** — runs one of InvoiceNinja's reports (A/R, sales, tax, profit/loss, …).

See the [Claude Code plugin docs](https://code.claude.com/docs/en/plugins) for the `SKILL.md` format, or the `superpowers:writing-skills` skill for a guided walkthrough.

Skills here should call the MCP tools exposed by [invoiceninja-mcp](https://github.com/Marceltov/invoiceninja-mcp) (`getClients`, `storeInvoice`, `bulkInvoices`, …) rather than talking to the InvoiceNinja API directly. For anything a generated tool can't express (a field missing from the OpenAPI spec, an endpoint it doesn't cover), use the server's `apiRequest` tool — see [Tools](https://invoiceninja-mcp.marceltov.de/tools/).

## Working with multiple instances

A user may have more than one InvoiceNinja instance connected at once — each shows up as its own MCP server, so tool names are namespaced per instance: `mcp__invoiceninja__getClients` for the default instance, `mcp__invoiceninja-work__getClients` for one labeled `work`, and so on (see `manage-invoiceninja-instances`). Every skill that calls InvoiceNinja MCP tools resolves which instance to use like this:

1. Note which `mcp__invoiceninja([-_].+)?__*` tool prefixes are actually available this session.
2. Exactly one exists → use it, no question asked.
3. More than one exists → check whether the user already named an instance in the conversation (by label, or something identifying like "the work one") and match it to the corresponding prefix.
4. Still ambiguous → ask once which instance, listing the available labels.
5. Use that one resolved prefix for every MCP tool call for the rest of the current task.

## Conventions

- InvoiceNinja ids are opaque hashed strings (e.g. `D2J234DFA`), not integers. Always resolve names to ids via a lookup, never guess.
- The server treats the OpenAPI spec as a hint: request bodies require nothing up front and **undeclared fields are silently dropped** by generated tools. After creating or updating something, read it back and check the fields you set actually landed; if one didn't, redo it with `apiRequest`.
- Money-moving or customer-visible actions (emailing, recording payments, deleting) are confirmed with the user first.
