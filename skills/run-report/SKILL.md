---
name: run-report
description: Run an InvoiceNinja report (A/R summary or detail, client balances, sales, tax, profit and loss, invoices, payments, expenses, products) via the get*Report MCP tools. Use when the user asks for a report, totals, sales figures, outstanding receivables or profit and loss over a period.
---

# Run an InvoiceNinja Report

If more than one InvoiceNinja instance is connected this session, resolve which one to use first — see [Working with multiple instances](../README.md#working-with-multiple-instances).

## Tools

All take the same body: `date_range` (e.g. `last7`, `last30`, `this_month`, `last_month`, `this_year`, `last_year`, `custom`), plus `start_date` / `end_date` (`YYYY-MM-DD`) for `custom`, an optional `date_key` column and `report_keys` to choose columns.

| Question | Tool |
| --- | --- |
| Who owes what, overall / per invoice | `getARSummaryReport` / `getARDetailReport` |
| Client balances | `getClientBalanceReport` |
| Sales per client / product / user | `getClientSalesReport` / `getProductSalesReport` / `getUserSalesReport` |
| Taxes | `getTaxSummaryReport` / `getTaxPeriodReport` |
| Profit and loss | `getProfitLossReport` |
| Raw lists | `getInvoiceReport`, `getPaymentReport`, `getExpenseReport`, `getQuoteReport`, `getClientReport`, `getProductReport` |

## Steps

1. Pick the tool from the table and turn the user's period into `date_range` (or `custom` + dates). If the period isn't stated, ask once.
2. Call it. Reports are read-only.
3. Summarize the headline numbers first (totals, top items), then offer the detail. If the response is a file or an export reference rather than rows, say so and tell the user where it came back.
4. If a report tool errors on a body field, retry the same endpoint via `apiRequest` (`POST /api/v1/reports/<name>`) rather than guessing.
