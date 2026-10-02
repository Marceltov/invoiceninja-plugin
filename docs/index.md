# InvoiceNinja Plugin

A [Claude Code](https://code.claude.com) plugin with ready-made skills for [InvoiceNinja](https://invoiceninja.com): find and create clients, draft and send invoices, record payments, convert quotes and run reports. It also sets up the connection to your [invoiceninja-mcp](https://invoiceninja-mcp.marceltov.de/) server for you.

```
/plugin marketplace add Marceltov/invoiceninja-plugin
/plugin install invoiceninja
```

!!! warning "Needs a running invoiceninja-mcp server"

    This plugin is the **client half** of a pair. It does not run or bundle the MCP server: deploy [invoiceninja-mcp](https://invoiceninja-mcp.marceltov.de/) next to your InvoiceNinja first (see its [getting started](https://invoiceninja-mcp.marceltov.de/getting-started/)). Without it, every tool call fails.

<div class="grid cards" markdown>

- :material-download: **[Install and connect](install.md)** — install the plugin and register your InvoiceNinja.
- :material-lightning-bolt: **[Skills](skills.md)** — everything the plugin can do.
- :material-server-network: **[Multiple instances](multiple-instances.md)** — several accounts side by side.
- :material-server: **[invoiceninja-mcp](https://invoiceninja-mcp.marceltov.de/)** — the server half: the sidecar that exposes InvoiceNinja as MCP tools.

</div>

## What it does

- **Sets up the connection.** If no InvoiceNinja is configured, a `SessionStart` hook notices and Claude offers to register one: it asks for the URL and API token and runs `claude mcp add` for you.
- **Knows InvoiceNinja's workflows.** Skills cover invoice filters and statuses, bulk actions like email and mark-paid, quote conversion and reports, so Claude picks the right one of the server's [377 tools](https://invoiceninja-mcp.marceltov.de/tools/) instead of guessing.
- **Handles several instances.** Skills ask which one you mean when it isn't clear.
- **Asks before customer-visible changes.** Emailing an invoice or recording a payment is confirmed with you first.

## License

[MIT](https://github.com/Marceltov/invoiceninja-plugin/blob/main/LICENSE).
