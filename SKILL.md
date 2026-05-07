---
name: xero-sli-mcp
description: "Analyse Xero financial data through the lens of Service Leadership Index (SLI) benchmarks. Use when asked about MSP financial health, P&L analysis, gross margin, profit benchmarks, management accounts, SLI scoring, revenue ratios, or any Xero+SLI combined analysis. Requires the xero-accounting-mcp npm package."
license: MIT
compatibility: Requires Node.js 18+ and the xero-accounting-mcp MCP server installed and configured
metadata:
  author: simonsmyth
  version: "1.0.0"
  category: finance
  tags: xero,accounting,msp,sli,benchmarks,management-accounts
---

# Xero + SLI MSP Financial Analysis

Query Xero Accounting data through the Service Leadership Index (SLI) benchmark framework. Produce actionable management accounts, not just reports.

## Architecture

This skill works with the `xero-accounting-mcp` npm package — a full-passthrough MCP server for the Xero Accounting API. The MCP server provides 23 tools (3 schema discovery + 1 generic passthrough + 19 convenience). This skill tells you *how to use those tools* to produce MSP-specific financial analysis.

If the MCP server isn't configured, guide the user through setup:

```
Set up the Xero Accounting MCP server. The npm package is xero-accounting-mcp.
1. Ask whether they want Claude Code, Claude Desktop, or Claude Cowork.
2. Help them get Xero credentials from https://developer.xero.com/app/manage
3. Add the MCP config to the appropriate settings file.
```

## Quick Start

### Schema Discovery (always available)
- `xero_get_schema_overview` — 138 endpoints, API version, categories
- `xero_get_endpoint_details` — drill into specific endpoints by regex
- `xero_search_endpoints` — keyword search

### Key Reports
- `xero_get_reports_profit_and_loss` — with periods, tracking, date ranges
- `xero_get_reports_balance_sheet` — as-at date, comparative periods
- `xero_get_reports_budget_summary` — budget vs actuals
- `xero_get_reports_executive_summary` — key metrics at a glance
- `xero_get_reports_trial_balance` — TB as-at date
- `xero_get_reports_bank_summary` — all bank balances
- `xero_get_reports_aged_payables` / `xero_get_reports_aged_receivables` — by contact

### Data Endpoints
- `xero_get_organisation` — legal name, currency, year end
- `xero_get_accounts` — chart of accounts
- `xero_get_invoices` — paginated, filterable
- `xero_get_contacts` — paginated, filterable
- `xero_get_journals` — general ledger (offset-based pagination)
- `xero_get_tracking_categories` — tracking categories + options
- `xero_get_budgets` — list or single budget detail

### Generic Passthrough
- `xero_api_call` — hit ANY Xero endpoint: method, path, body, queryParams

## Xero Report Parsing

All Xero reports return `{ Reports: [{ Rows: [...] }] }`:
- `RowType: "Header"` — column headers
- `RowType: "Section"` — group with Title and nested Rows
- `RowType: "Row"` — data row, Cells: `[{Value: "Name"}, {Value: "1234.56"}]`
- `RowType: "SummaryRow"` — totals

## SLI Framework — MSP Financial Benchmarks

The Service Leadership Index is the gold standard for MSP financial benchmarking. This skill maps Xero data to SLI categories and benchmarks every number against best-in-class targets.

### Core Principle
**Gross Margin − Expenses = Profit** — the only formula that matters.

### Revenue Categories (map Xero accounts to these)

| SLI Category | Code Range | Description |
|---|---|---|
| Product Resale | PR1–PR9 | Hardware, software, cloud resale |
| Cloud Services Resale | CL1–CL3 | White-label and third-party branded |
| Hardware as a Service | HA1 | Monthly, client doesn't take title |
| Software Licenses (Own) | SL1–SL4 | Software you built/published |
| Infra — Technical Services | I1–I6 | Break/fix, staff aug, training |
| Infra — Professional Services | I7–I13 | Scoped projects: install, design |
| Infra — Managed Services | I14–I26 | SLA-based flat fee support |
| Infra — Shared Infra | I27–I31 | Hosting, colo, ISP from your infra |
| Apps — Technical Services | A1–A5 | App dev T&M, training |
| Apps — Professional Services | A6–A9 | Custom app dev, packaged config |
| Apps — Managed Services | A10–A15 | ASP, app monitoring, DBA (SLA) |
| Commissions & Agency | CA1–CA5 | Third-party vendor fees |

### 8 Best-in-Class Financial Principles

1. **Revenue Ratio** (per $1 managed service revenue): 40¢ cloud recurring, 35¢ physical product, 15¢ project labour
2. **GM Growth Rate** must match or exceed revenue growth rate
3. **Profit %**: Best-in-class MSPs target 18%+ profit (top 25th percentile)
4. **Gross Margin %**: BIC target varies by model; MSP target ~42%
5. **Service Multiple**: GM / Service delivery cost — target ≥1.4x
6. **Sales Multiple**: Revenue / Sales expense — target ≥6x
7. **G&A %**: Keep below 12% of revenue
8. **Revenue per employee**: BIC target varies; MSP ~$150K–$200K

### Key SLI Ratios to Calculate

| Ratio | Formula | BIC Target |
|---|---|---|
| Profit % | Profit / Revenue | ≥18% |
| GM % | Gross Margin / Revenue | ≥42% (MSP) |
| Service Multiple | GM / (Service Delivery Cost) | ≥1.4x |
| Sales Multiple | Revenue / Sales Expense | ≥6x |
| G&A % | G&A Expense / Revenue | ≤12% |
| Revenue/Employee | Total Revenue / FTE Count | ~$150-200K |

## Management Accounts Workflow

**This is NOT a bland accountant report.** Every number gets context, every trend gets an explanation, every section ends with actions.

### Data Collection
1. Xero: P&L (month + YTD + prior year), Balance Sheet, Executive Summary, Aged Receivables
2. Adjusted P&L (subtract owner drawings from profit — Xero doesn't track this automatically)
3. Xero: Chart of Accounts + Tracking Categories for SLI mapping
4. Optional: CRM pipeline (HubSpot/ConnectWise), PPC data, forecast model

### Report Structure

| Section | Purpose | Data Sources |
|---|---|---|
| Executive Summary | 3 bullets: what happened, what's changing, what to do | All |
| P&L — Standard View | Month vs prior month vs prior year | Xero |
| P&L — Adjusted View | After owner drawings — the REAL cash picture | Xero + drawings |
| Revenue Analysis | By SLI category, vs targets, vs prior periods | Xero mapped to SLI |
| Gross Margin Deep Dive | GM% by service line vs SLI BIC | Xero + SLI benchmarks |
| Client Health | Top 10 by revenue, risers/fallers, concentration risk | Xero contacts/invoices |
| Cash & Debtors | Bank balance, aged receivables, debtor days | Xero balance sheet |
| SLI Scorecard | Key ratios vs BIC: profit%, GM%, service multiple | Calculated vs SLI |
| Actions | Numbered, assigned, with deadlines | All |

### Critical Rules
- **Always show Adjusted profit** (after owner drawings) — raw P&L is misleading
- **Never report a number without context** — "Revenue down 5%" is useless. "Revenue down 5% because Client X left, offset by £2K new business from PPC" is useful
- **Compare to SLI benchmarks** not just prior periods — "GM% is 46% (BIC target 42%)" tells you more than "GM% up 1%"
- **Flag anomalies** — any client billing >50% variance from their 3-month average gets called out
- **End with actions** — every report must answer "so what do I DO?"

## Mapping Xero Accounts to SLI

Xero accounts map to SLI categories by account type and name patterns. See [references/xero-sli-mapping.md](references/xero-sli-mapping.md) for the full mapping guide.

Common mappings:
- `REVENUE` + managed service contracts → I14–I26
- `REVENUE` + break/fix/time → I1–I6
- `REVENUE` + product resale → PR1–PR9
- `DIRECTCOSTS` → COGS (cost of goods sold)
- `EXPENSE` + sales staff → Sales expense (for Sales Multiple)
- `EXPENSE` + admin staff → G&A (for G&A %)

## Rate Limits

Xero enforces: 60 calls/min, 5,000/day, 5 concurrent. On 429, respect `Retry-After`.

## Context Log Pattern

When producing recurring management accounts, maintain a running context log (datestamped entries) that captures:
- Client gains/losses and revenue impact
- Staff changes and salary adjustments
- Cost changes (new subscriptions, cancellations, renegotiated contracts)
- One-off events affecting that month's numbers
- Owner drawing patterns

This context log is THE key differentiator between a report and a management tool. Without it, you're just reading numbers. With it, you're providing insight.