# Xero Account → SLI Category Mapping Guide

## How to Map

Xero accounts have a `Type` field and a `Name` field. The Type narrows the SLI category; the Name disambiguates within it.

## By Account Type

### REVENUE Accounts

| Xero Account Name Pattern | SLI Category | Codes |
|---|---|---|
| Managed Services, Support Contract, SLA, Retainer | Infra — Managed Services | I14–I26 |
| Break/Fix, Time & Materials, Ad-hoc, Reactive | Infra — Technical Services | I1–I6 |
| Project, Installation, Design, Rollout | Infra — Professional Services | I7–I13 |
| Hosting, Colocation, ISP, Backup (your infra) | Infra — Shared Infrastructure | I27–I31 |
| Cloud (your brand) | Cloud Services Resale | CL1 |
| Cloud (vendor brand, e.g. M365) | Cloud Services Resale | CL2 |
| Hardware, Servers, PCs, Printers, Network Equipment | Product Resale | PR1–PR7 |
| Software Resale, Licenses (others') | Product Resale | PR4–PR5 |
| App Development (T&M) | Apps — Technical Services | A1–A5 |
| App Development (scoped project) | Apps — Professional Services | A6–A9 |
| App Monitoring, DBA, ASP | Apps — Managed Services | A10–A15 |
| Commission, Referral, Agency Fee | Commissions & Agency | CA1–CA5 |
| Software (own product) | Software Licenses (Own) | SL1–SL4 |
| HaaS, Hardware Rental | Hardware as a Service | HA1 |

### DIRECTCOSTS / COGS Accounts

| Xero Account Name Pattern | SLI Category | Notes |
|---|---|---|
| Product COGS, Hardware COGS | Product Resale COGS | Matches to PR1–PR9 revenue |
| Cloud COGS, M365 COGS | Cloud Services COGS | Matches to CL1–CL3 revenue |
| Subcontractors, Outsource | Service Delivery Cost | For Service Multiple calc |
| Software COGS (own product) | Software Licenses COGS | Matches to SL1–SL4 revenue |

### EXPENSE Accounts — Sales

| Xero Account Name Pattern | SLI Category | Notes |
|---|---|---|
| Sales Salary, Commission, Bonus | Sales Expense | For Sales Multiple calc |
| Marketing, PPC, Advertising | Marketing Expense | Below the line for SLI |
| Travel (sales-related) | Sales Expense | If attributable to sales team |

### EXPENSE Accounts — G&A

| Xero Account Name Pattern | SLI Category | Notes |
|---|---|---|
| Admin Salary, Office, Insurance | G&A | For G&A % calc |
| Accounting, Legal, Bank Fees | G&A | |
| Rent, Utilities | G&A | |

### EXPENSE Accounts — Service Delivery

| Xero Account Name Pattern | SLI Category | Notes |
|---|---|---|
| Engineer Salary, Tech Salary | Service Delivery | For Service Multiple calc |
| Training, Certifications | Service Delivery | |
| Software (internal tools, RMM, PSA) | Service Delivery | |

## Service Multiple Calculation

```
Service Multiple = Gross Margin / Service Delivery Cost
```

Where:
- **Gross Margin** = Revenue − Direct Costs (COGS)
- **Service Delivery Cost** = Engineer salaries + training + internal tooling

Compare against your SLI benchmark data

## Sales Multiple Calculation

```
Sales Multiple = Total Revenue / Total Sales Expense
```

Where:
- **Total Revenue** = all SLI categories combined
- **Total Sales Expense** = sales salaries + commissions + marketing

Compare against your SLI benchmark data

## G&A Percentage Calculation

```
G&A % = G&A Expense / Total Revenue × 100
```

Compare against your SLI benchmark data

## Profit Percentage (Adjusted)

```
Adjusted Profit % = (Net Profit − Owner Drawings) / Revenue × 100
```

Owner drawings are NOT in Xero — they're typically dividends + PAYE above market rate. Ask the business owner for their drawing amount per month.

Compare against your SLI benchmark data

## Common Xero Account Type Codes

| Code | Type | Typical SLI Mapping |
|---|---|---|
| REVENUE | Revenue | Map by name to SLI categories above |
| SALES | Revenue | Usually I1–I6 or I14–I26 |
| DIRECTCOSTS | Cost of Sales | COGS — map to matching revenue category |
| EXPENSE | Expense | Split into Sales, G&A, Service Delivery |
| OVERHEADS | Expense | Usually G&A |
| DEPRECIATN | Depreciation | G&A (below operating profit) |
| CURRLIAB | Current Liability | Balance sheet only |
| LIABILITY | Liability | Balance sheet only |
| EQUITY | Equity | Balance sheet only |
| BANK | Bank Account | Balance sheet only |
| FIXED | Fixed Asset | Balance sheet only |