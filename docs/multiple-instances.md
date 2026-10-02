# Multiple instances

You can connect more than one InvoiceNinja, for example separate accounts for two companies. Each needs its own [invoiceninja-mcp container](https://invoiceninja-mcp.marceltov.de/getting-started/) and is registered under its own name:

| Instance | MCP server name | Tool prefix |
| -------- | --------------- | ----------- |
| Default | `invoiceninja` | `mcp__invoiceninja__*` |
| Labeled, e.g. `work` | `invoiceninja-work` | `mcp__invoiceninja-work__*` |

## Add, list or remove one

Ask Claude, for example "add my work InvoiceNinja" or "which InvoiceNinja instances are connected?". The `manage-invoiceninja-instances` skill handles it:

- **Add:** asks for a label (like `work` or `home`), the URL and the API token, then registers `invoiceninja-<label>`. Restart Claude Code so it connects.
- **List:** shows every configured instance and whether it's connected.
- **Remove:** unregisters the instance you name.

## Which instance a skill uses

With one instance connected, skills just use it. With several, a skill first checks whether you already named one ("in my work account"); if not, it asks once which instance you mean and sticks with it for the rest of the task.
