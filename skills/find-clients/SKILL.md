---
name: find-clients
description: Look up InvoiceNinja clients by name, email, number, id number or balance using the getClients MCP tool. Use when the user asks to find, list, search or look up a client or customer, or when another skill needs a client id from a name.
---

# Find InvoiceNinja Clients

A thin wrapper around `getClients`. Other skills use it to turn a client name into the hashed `client_id` they need.

If more than one InvoiceNinja instance is connected this session, resolve which one to use first — see [Working with multiple instances](../README.md#working-with-multiple-instances).

## Steps

1. **Pick the filter** from what the user gave you: `filter` is a free-text search over name/contacts/email; `email`, `number`, `id_number` match those fields exactly; `balance` / `between_balance` filter by outstanding balance. Prefer `filter` for a plain name. Don't invent a filter more specific than the request.
2. **Call `getClients`** with it. Use `per_page` (default is 20) and `page` if the user wants more, and `include=contacts` when they need contact emails.
3. **Zero results**: say so and offer a broader `filter` (a shorter fragment of the name); don't silently retry variations.
4. **Several results** when a single client is needed: list them (name, number, email) and ask which one.
5. **Present concisely**: name, client id, number, balance, primary contact email — not the full JSON.
