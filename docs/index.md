---
hide:
  - toc
---

<div class="tm-hero" markdown>

# Skills for your InvoiceNinja in Claude Code

<p class="tm-hero__lead">invoiceninja-plugin gives Claude Code ready-made skills for InvoiceNinja: find and create clients, draft and send invoices, record payments, convert quotes and run reports. It also sets up the connection to your invoiceninja-mcp server for you.</p>

[Install it](install.md){ .md-button .md-button--primary } [Browse the skills](skills.md){ .md-button }

</div>

```
/plugin marketplace add Marceltov/invoiceninja-plugin
/plugin install invoiceninja
```

!!! warning "Needs a running invoiceninja-mcp server"

    This plugin is only the client half. It does not run or bundle the MCP server: you need [invoiceninja-mcp](https://invoiceninja-mcp.marceltov.de/) deployed next to your InvoiceNinja first (see its [quick start](https://invoiceninja-mcp.marceltov.de/getting-started/)). Without it, every tool call fails.

## What it does

- **Sets up the connection.** If no InvoiceNinja is configured, a `SessionStart` hook notices and Claude offers to register one: it asks for the URL and API token and runs `claude mcp add` for you.
- **Knows InvoiceNinja's workflows.** Skills cover invoice filters and statuses, bulk actions like email and mark-paid, quote conversion and reports, so Claude picks the right one of the server's 377 tools instead of guessing.
- **Handles several instances.** Connect more than one account side by side; skills ask which one you mean when it isn't clear. See [Multiple instances](multiple-instances.md).
- **Asks before customer-visible changes.** Emailing an invoice or recording a payment is confirmed with you first.
