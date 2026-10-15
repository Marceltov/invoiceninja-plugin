<h1 align="center"><a href="https://github.com/Marceltov/invoiceninja-plugin">invoiceninja-plugin</a></h1>

<p align="center">
  <a href="https://github.com/Marceltov/invoiceninja-mcp">
    <img alt="requires a running Invoice Ninja MCP server" src="https://img.shields.io/badge/requires-invoiceninja--mcp_sidecar_running-critical">
  </a>
  <a href="https://invoiceninja-plugin.marceltov.de/">
    <img alt="Documentation" src="https://img.shields.io/badge/docs-invoiceninja--plugin.marceltov.de-2563eb">
  </a>
  <a href="LICENSE">
    <img alt="License: MIT" src="https://img.shields.io/badge/License-MIT-blue.svg">
  </a>
</p>

A [Claude Code](https://code.claude.com) plugin for [Invoice Ninja](https://invoiceninja.com): skills for clients, invoices, quotes, payments and reports, plus the MCP connection to a running [Invoice Ninja MCP server](https://github.com/Marceltov/invoiceninja-mcp).

> [!IMPORTANT]
> This plugin does **not** run, host, or bundle the MCP server. It's a thin client: it only works if you already have an [Invoice Ninja MCP server](https://github.com/Marceltov/invoiceninja-mcp) deployed and reachable as a **container sidecar** next to your Invoice Ninja instance (see its [quick start](https://invoiceninja-mcp.marceltov.de/getting-started/)). Installing this plugin alone gets you nothing — without a running `invoiceninja-mcp` sidecar and a valid API token, every tool call fails.

**Full documentation: [invoiceninja-plugin.marceltov.de](https://invoiceninja-plugin.marceltov.de/)**

- [Install and connect](https://invoiceninja-plugin.marceltov.de/install/): install the plugin and register your Invoice Ninja instance
- [Multiple instances](https://invoiceninja-plugin.marceltov.de/multiple-instances/): several Invoice Ninja accounts side by side
- [Skills](https://invoiceninja-plugin.marceltov.de/skills/): everything the plugin can do

## Install

```
/plugin install invoiceninja --marketplace Marceltov/invoiceninja-plugin
```

On Claude Code before 2.1.275, add the marketplace first:

```
/plugin marketplace add Marceltov/invoiceninja-plugin
/plugin install invoiceninja
```

Then start a new session: Claude offers to connect your Invoice Ninja instance if none is configured.

**On claude.ai?** Connect the Invoice Ninja MCP server as a connector, then add this repository under *Customize → Plugins → Add → Add marketplace* and add `invoiceninja`. See [Use on claude.ai](https://invoiceninja-plugin.marceltov.de/claude-ai/).

## Related project

This plugin is the **client half**: skills that call MCP tools. The [Invoice Ninja MCP server](https://github.com/Marceltov/invoiceninja-mcp) is the **server half** — the container sidecar that exposes the Invoice Ninja v5 API as those MCP tools in the first place. You need both.

## License

MIT — see [LICENSE](LICENSE).
