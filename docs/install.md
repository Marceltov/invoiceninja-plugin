# Install and connect

## 1. Deploy the Invoice Ninja MCP server

The plugin talks to Invoice Ninja through a running [Invoice Ninja MCP server](https://invoiceninja-mcp.marceltov.de/). Set that up first with its [quick start](https://invoiceninja-mcp.marceltov.de/getting-started/), and create an API token under *Settings → Account Management → Integrations → API tokens* in Invoice Ninja.

## 2. Install the plugin

In Claude Code:

```
/plugin marketplace add Marceltov/invoiceninja-plugin
/plugin install invoiceninja
```

## 3. Connect your Invoice Ninja instance

Start a new Claude Code session. If no Invoice Ninja instance is configured, the plugin's `SessionStart` hook flags it and Claude offers to run the `invoiceninja-manage-instances` skill. It offers this once per installation, in the first session after you install the plugin; after that, ask for it yourself whenever you like, for example "set up the Invoice Ninja plugin".

The skill asks for the Invoice Ninja MCP server URL and your API token and registers the instance under the name `invoiceninja`, scoped to your user so every project sees it. Configure instances only through this skill, so the names match what the other skills look for.

Restart Claude Code afterwards: a newly registered server only connects on the next launch.

If you use several Claude Code installations (separate `CLAUDE_CONFIG_DIR`s), each is configured on its own.
