# Install and connect

## 1. Deploy invoiceninja-mcp

The plugin talks to InvoiceNinja through a running [invoiceninja-mcp](https://invoiceninja-mcp.marceltov.de/) server. Set that up first with its [quick start](https://invoiceninja-mcp.marceltov.de/getting-started/), and create an API token under *Settings → Account Management → Integrations → API tokens* in InvoiceNinja.

## 2. Install the plugin

In Claude Code:

```
/plugin marketplace add Marceltov/invoiceninja-plugin
/plugin install invoiceninja
```

## 3. Connect your InvoiceNinja

Start a new Claude Code session. If no InvoiceNinja is configured, the plugin's `SessionStart` hook flags it and Claude offers to run the `manage-invoiceninja-instances` skill. You can also ask for it yourself at any time, for example "set up the InvoiceNinja plugin".

The skill asks for the invoiceninja-mcp URL and your API token and registers the instance under the name `invoiceninja`, scoped to your user so every project sees it. Configure instances only through this skill, so the names match what the other skills look for.

Restart Claude Code afterwards: a newly registered server only connects on the next launch.

If you use several Claude Code installations (separate `CLAUDE_CONFIG_DIR`s), each is configured on its own.
