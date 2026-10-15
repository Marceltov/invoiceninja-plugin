---
name: invoiceninja-manage-instances
description: Configure connections to one or more Invoice Ninja MCP server deployments — add a new instance (URL + Invoice Ninja API token, optionally labeled), list which are configured/connected, or remove one. Every instance is registered as a user-scoped MCP server via `claude mcp add` (no settings.json editing, no bundled .mcp.json). Use when the invoiceninja MCP server is unconfigured, disconnected, or missing credentials (including when a SessionStart hook flags this), when the user asks to set up, configure, or connect the Invoice Ninja plugin, or when they want to add, remove, or list additional Invoice Ninja instances.
---

# Manage Invoice Ninja Instances

Every Invoice Ninja instance — including the first/default one — is its own MCP server registered with the `claude mcp add` CLI command, scoped `user` so it's available to every project under the current Claude Code installation. The default instance is named `invoiceninja`; each additional one is `invoiceninja-<label>` (e.g. `invoiceninja-work`). Claude Code exposes each server's tools under its own prefix (`mcp__invoiceninja__*`, `mcp__invoiceninja-work__*`, …); skills resolve which prefix to use per task — see [Working with multiple instances](reference/shared-rules.md#working-with-multiple-instances).

**This skill is the only way instances get configured.** Go through `claude mcp add`/`list`/`get`/`remove` only. Never write an `mcpServers` block into settings.json (it rejects the key) and never create a `.mcp.json`.

## On claude.ai, the desktop app or Cowork

There is no `claude` CLI there; an Invoice Ninja MCP server is connected as a **custom connector** instead. If Invoice Ninja tools are already available, say so and stop. Otherwise explain the steps, with the Invoice Ninja MCP server's public HTTPS URL ending in `/mcp`:

1. The server needs OAuth enabled (`MCP_BASE_URL` and `MCP_OAUTH_SECRET` set); claude.ai can't send a token header.
2. *Customize → Connectors → Add custom connector*: a name (for example `Invoice Ninja`) and the URL, keep the detected OAuth choices, then sign in with the Invoice Ninja email and password.
3. Switch the connector on in the chat (**+ → Connectors**).

Details: https://invoiceninja-mcp.marceltov.de/connecting/#with-oauth. Everything below applies to Claude Code only.

## Determine the operation

Figure out from the request whether the user wants to **add** a new instance, **list** what's configured, or **remove** one. Default to **add** when the trigger is a missing-credentials nudge and the user hasn't said otherwise.

## Add an instance

1. **Check whether the default instance is already configured before asking anything**, if this is meant to be the first/default instance. Two independent signals both indicate it's already in place:
   - The `mcp__invoiceninja__*` tools are already listed as available in *this* session.
   - Run `claude mcp get invoiceninja` — exit code 0 means it's already registered (the output includes its scope and live connection status; it never shows the token).

   If either signal is true, **stop here** — report that it's already configured (naming the scope from `claude mcp get`'s output) and finish. Only continue if the user explicitly wants an *additional* instance, or neither signal is true.

2. **If this is an additional instance (not the first), ask for a label** — a short name like `work` or `home`. Sanitize it: lowercase, replace every run of characters outside `[a-z0-9]` with `-`, strip leading/trailing `-`. If the result is empty, ask for a different one. Then run `claude mcp get invoiceninja-<label>` — exit 0 means that name is already taken — ask for a different label rather than silently overwriting an existing instance.

   Skip this step entirely for the first/default instance — it always stays unlabeled (server name `invoiceninja`).

3. **Ask for the remaining values**, one at a time (plain question or `AskUserQuestion` if available):
   - The Invoice Ninja MCP server URL, ending in `/mcp`. For the default instance, `http://localhost:8081/mcp` is a common value for a local Invoice Ninja MCP server container — offer it as a suggestion, but still ask. For a labeled instance, there's no natural default — just ask.
   - Its API token, created in Invoice Ninja under *Settings → Account Management → Integrations → API tokens*. Required, no default.
   - Never print the token back in a confirmation message or log it.

4. **Register the instance** by running:

       claude mcp add --transport http <name> "<url>" -H "Authorization: <token>" -s user

   where `<name>` is `invoiceninja` for the default instance or `invoiceninja-<label>` for an additional one. The token appears in this command's arguments (visible in the tool call, and briefly in process listings while it runs) — there's no CLI option to supply it another way; don't additionally print or log it anywhere else.

5. **Tell the user to restart Claude Code** (or start a new session) — a newly `claude mcp add`-ed server only connects into the current session's tool list after a restart.

## List instances

1. Run `claude mcp list` — it health-checks every configured server fresh.
2. Keep only lines whose server name matches `invoiceninja([-_].+)?`. Each line ends with a connection status (e.g. "✔ Connected" or "✘ Failed to connect — ...").
3. Report the list: label (or "default" for `invoiceninja`), and connected/not-connected per that status. Never print token values.

## Remove an instance

1. Ask which instance (by label, or "default" for `invoiceninja`) if not already clear.
2. Run `claude mcp remove <name>` with the instance's exact server name (no `-s` needed).
3. Tell the user to restart Claude Code for the removal to take effect.
