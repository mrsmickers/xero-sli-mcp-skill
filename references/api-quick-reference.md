# Xero API Quick Reference

## Authentication

This skill assumes the `xero-accounting-mcp` server is configured with:
- `XERO_CLIENT_ID` — from Xero Developer Portal (Custom Connection)
- `XERO_CLIENT_SECRET` — from Xero Developer Portal
- `XERO_TENANT_ID` — the connected tenant UUID

Client credentials grant, auto-refresh every 30 minutes. No manual token handling needed.

## Rate Limits

- 60 calls/min per tenant
- 5,000 calls/day (Core+ tier)
- 5 concurrent requests max
- On 429: respect `Retry-After` header

## Scopes Required

```
accounting.reports.read
accounting.transactions.read
accounting.contacts.read
accounting.settings.read
accounting.budgets.read
accounting.journals.read
```

## Report Response Format

All Xero reports return:
```json
{
  "Reports": [{
    "ReportID": "...",
    "ReportName": "...",
    "ReportTitles": ["..."],
    "Rows": [
      { "RowType": "Header", "Cells": [...] },
      { "RowType": "Section", "Title": "...", "Rows": [...] },
      { "RowType": "Row", "Cells": [{ "Value": "Label" }, { "Value": "1234.56" }] },
      { "RowType": "SummaryRow", "Cells": [...] }
    ]
  }]
}
```

## Key Endpoints (via MCP tools)

### P&L with Period Comparison
```
xero_get_reports_profit_and_loss({
  fromDate: "2026-01-01",
  toDate: "2026-03-31",
  periods: 3,
  timeframe: "MONTH"
})
```

### Balance Sheet
```
xero_get_reports_balance_sheet({ date: "2026-03-31" })
```

### Executive Summary
```
xero_get_reports_executive_summary({ date: "2026-03-31" })
```

### Aged Receivables by Contact
```
xero_get_reports_aged_receivables({ contactId: "uuid-here" })
```

### Invoices with Filtering
```
xero_get_invoices({
  page: 1,
  where: 'Status=="AUTHORISED"&&Type=="ACCREC"',
  order: "Date DESC"
})
```

### Chart of Accounts
```
xero_get_accounts()
```

### Any Endpoint (passthrough)
```
xero_api_call({
  method: "GET",
  path: "/Reports/ProfitAndLoss",
  queryParams: { fromDate: "2026-01-01", toDate: "2026-03-31" }
})
```

## OData Where Clauses

Xero uses OData-style filters:
- `Status=="AUTHORISED"`
- `Type=="ACCREC"` (sales) / `Type=="ACCPAY"` (bills)
- `Date>=DateTime(2026,01,01)`
- Combine with `&&`: `Status=="AUTHORISED"&&Type=="ACCREC"`
- URL-encode the where parameter value

## Pagination

- Invoices, Contacts, BankTransactions, CreditNotes: `?page=N` (100 per page)
- Journals: `?offset=N` (use JournalNumber from last record)
- Reports: single response, not paginated