# Shared rules for the Invoice Ninja skills

<!-- Single source. Copied into every skills/*/reference/shared-rules.md by scripts/sync-shared.sh; edit this file, not the copies. -->

## Working with multiple instances

A user may have more than one Invoice Ninja instance connected at once — each shows up as its own MCP server, so tool names are namespaced per instance: `mcp__invoiceninja__getClients` for the default instance, `mcp__invoiceninja-work__getClients` for one labeled `work`, and so on (see `invoiceninja-manage-instances`). Every skill that calls Invoice Ninja MCP tools resolves which instance to use like this:

1. Note which Invoice Ninja tool prefixes are actually available in this conversation: `mcp__invoiceninja([-_].+)?__*`, plus a claude.ai connector's (see below).
2. Exactly one exists → use it, no question asked.
3. More than one exists → check whether the user already named an instance in the conversation (by label, or something identifying like "the work one") and match it to the corresponding prefix.
4. Still ambiguous → ask once which instance, listing the available labels.
5. Use that one resolved prefix for every MCP tool call for the rest of the current task.

On claude.ai, the Claude desktop app or in Cowork, an Invoice Ninja MCP server is connected as a connector instead, and its tools carry the connector's name as prefix (for example `mcp__claude_ai_<connector name>__getClients`). Treat it like any other instance. If no Invoice Ninja tools are available at all, say so and explain how to connect: in Claude Code with the `invoiceninja-manage-instances` skill, on claude.ai by adding the Invoice Ninja MCP server as a custom connector.

## Conventions

- Invoice Ninja ids are opaque hashed strings (e.g. `D2J234DFA`), not integers. Always resolve names to ids via a lookup, never guess.
- The server treats the OpenAPI spec as a hint: request bodies require nothing up front and **undeclared fields are silently dropped** by generated tools. After creating or updating something, read it back and check the fields you set actually landed; if one didn't, redo it with `apiRequest`.
- Money-moving or customer-visible actions (emailing, recording payments, deleting) are confirmed with the user first.
