# Use on claude.ai

The Invoice Ninja plugin works on claude.ai too, in chat on the web, in the Claude desktop app and in Cowork. You connect the Invoice Ninja MCP server as a connector and add the plugin from this repository; the skills are the same as in Claude Code.

## 1. Connect the Invoice Ninja MCP server as a connector

Claude.ai signs in with OAuth, so the server needs `MCP_BASE_URL` and `MCP_OAUTH_SECRET` set and a public HTTPS URL. Then add `https://<your host>/mcp` as a custom connector and complete the login: see [Connecting clients](https://invoiceninja-mcp.marceltov.de/connecting/#with-oauth) in the Invoice Ninja MCP server docs.

## 2. Add the plugin

1. Open *Customize → Plugins*, click **Add** and choose *Add marketplace → Add from a repository*.
2. Enter `Marceltov/invoiceninja-plugin`.
3. Open the `invoiceninja` plugin from that marketplace and click **Add**.

In a chat, type `/` to see the skills, for example `/invoiceninja-find-invoices`, or just ask and let Claude pick the right one. New versions arrive through the repository; there is nothing to re-upload.

The plugin's setup hook only runs in Claude Code and Cowork; in chat it is shown greyed out and does nothing. `invoiceninja-manage-instances` notices the connector and explains how to add one if it's missing.

## What's different from Claude Code

- **No shell in chat.** `invoiceninja-create-expense-from-receipt` books the expense from a receipt you attach in the chat. It attaches the file to the expense only if it's reachable at a URL; otherwise you add it in Invoice Ninja yourself.
