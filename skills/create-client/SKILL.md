---
name: create-client
description: Create a new InvoiceNinja client with contacts via the storeClient MCP tool. Use when the user asks to add, create or register a new client or customer in InvoiceNinja.
---

# Create an InvoiceNinja Client

If more than one InvoiceNinja instance is connected this session, resolve which one to use first — see [Working with multiple instances](../README.md#working-with-multiple-instances).

## Steps

1. **Check for a duplicate first**: call `getClients` with `filter` set to the name (and `email` if given). If something plausible exists, show it and ask whether to use it instead.
2. **Collect the fields**, asking only for what's missing: `name` (required in practice), and at least one contact (`first_name`, `last_name`, `email`) if the client should receive invoices by email. Optional: `address1`, `city`, `postal_code`, `country_id`, `vat_number`, `phone`, `website`, `private_notes`. Never make up values.
3. **Call `storeClient`** with `{ "name": ..., "contacts": [{ "first_name": ..., "last_name": ..., "email": ... }], ... }`.
4. **Read it back** with `showClient` (`include=contacts`) and confirm the contacts landed — undeclared fields are silently dropped by the generated tool; if one is missing, set it via `apiRequest` `PUT /api/v1/clients/{id}`.
5. **Report** the name and client id.
