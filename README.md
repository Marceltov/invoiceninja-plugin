<h1 align="center"><a href="https://github.com/Marceltov/invoiceninja-plugin">invoiceninja-plugin</a></h1>

<p align="center">
  <a href="https://github.com/Marceltov/invoiceninja-mcp">
    <img alt="requires invoiceninja-mcp sidecar" src="https://img.shields.io/badge/requires-invoiceninja--mcp_sidecar_running-critical">
  </a>
  <a href="https://invoiceninja-plugin.marceltov.de/">
    <img alt="Documentation" src="https://img.shields.io/badge/docs-invoiceninja--plugin.marceltov.de-1f6fb2">
  </a>
  <a href="LICENSE">
    <img alt="License: MIT" src="https://img.shields.io/badge/License-MIT-blue.svg">
  </a>
</p>

A [Claude Code](https://code.claude.com) plugin for [InvoiceNinja](https://invoiceninja.com): skills for clients, invoices, quotes, payments and reports, plus the MCP connection to a running [invoiceninja-mcp](https://github.com/Marceltov/invoiceninja-mcp) server.

> [!IMPORTANT]
> This plugin does **not** run, host, or bundle the MCP server. It's a thin client: it only works if you already have [`invoiceninja-mcp`](https://github.com/Marceltov/invoiceninja-mcp) deployed and reachable as a **container sidecar** next to your InvoiceNinja instance (see its [quick start](https://invoiceninja-mcp.marceltov.de/getting-started/)). Installing this plugin alone gets you nothing — without a running `invoiceninja-mcp` sidecar and a valid API token, every tool call fails.

**Full documentation: [invoiceninja-plugin.marceltov.de](https://invoiceninja-plugin.marceltov.de/)**

- [Install and connect](https://invoiceninja-plugin.marceltov.de/install/): install the plugin and register your InvoiceNinja
- [Multiple instances](https://invoiceninja-plugin.marceltov.de/multiple-instances/): several InvoiceNinja accounts side by side
- [Skills](https://invoiceninja-plugin.marceltov.de/skills/): everything the plugin can do

## Install

```
/plugin marketplace add Marceltov/invoiceninja-plugin
/plugin install invoiceninja
```

Then start a new session: Claude offers to connect your InvoiceNinja if none is configured.

## Related project

This plugin is the **client half**: skills that call MCP tools. [`invoiceninja-mcp`](https://github.com/Marceltov/invoiceninja-mcp) is the **server half** — the container sidecar that exposes the InvoiceNinja v5 API as those MCP tools in the first place. You need both.

## License

MIT — see [LICENSE](LICENSE).
