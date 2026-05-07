# Xero + SLI MSP Financial Analysis Skill

A skill that analyses Xero financial data through the Service Leadership Index (SLI) account code framework, producing actionable management accounts — not just reports.

## What It Does

This skill teaches an AI agent how to:

- **Query Xero financial data** via the [xero-accounting-mcp](https://github.com/mrsmickers/xero-mcp) npm package
- **Map Xero accounts to SLI revenue and expense categories** using the standard SLI account codes
- **Produce management accounts** that compare actuals to benchmarks and flag anomalies
- **Calculate key MSP financial ratios** (profit %, gross margin %, service multiple, sales multiple, G&A %)
- **Maintain a context log** so recurring reports improve over time with business context

## How It Works

The skill works alongside the `xero-accounting-mcp` MCP server, which provides full passthrough access to the Xero Accounting API. The skill doesn't make API calls itself — it tells the agent *what to call* and *how to interpret* the results.

1. **Data collection** — P&L, Balance Sheet, Executive Summary, Aged Receivables, Chart of Accounts, Tracking Categories from Xero
2. **Account mapping** — Xero account types and names mapped to SLI revenue and expense categories
3. **Benchmark comparison** — Actuals vs SLI SLI targets for your cohort
4. **Anomaly detection** — Variance flagging on client billing, cost changes, and one-off items
5. **Report generation** — Structured management accounts with context, not just numbers

## Requirements

- A Xero account with a Custom Connection app (client_credentials grant)
- The [xero-accounting-mcp](https://github.com/npmjs.com/package/xero-accounting-mcp) npm package installed and configured as an MCP server
- Xero scopes: `accounting.reports.read`, `accounting.transactions.read`, `accounting.contacts.read`, `accounting.settings.read`, `accounting.budgets.read`, `accounting.journals.read`

## Installation

### With Claude Code

Copy or clone this skill into your Claude Code skills directory:

```bash
cp -r xero-sli-mcp-skill ~/.claude/skills/
```

Claude reads the `SKILL.md` frontmatter `description` field and activates the skill when the task matches (financial analysis, P&L, management accounts, benchmarking, etc.).

### With Claude Desktop

Add the skill folder path to your Claude Desktop project, or paste the `SKILL.md` content into your project instructions.

## Skill Structure

```
xero-sli-mcp-skill/
├── SKILL.md                              # Core instructions — always loaded when skill is active
├── references/
│   ├── xero-sli-mapping.md              # Xero account → SLI category mapping guide
│   └── api-quick-reference.md            # Xero MCP tool usage and API patterns
└── LICENSE                               # MIT
```

The skill uses progressive disclosure: the agent loads `SKILL.md` when activated, then reads reference files on demand when it needs mapping details or API specifics.

## What's SLI?

The Service Leadership Index is a financial benchmarking framework used by MSPs. It defines standardised account codes for categorising revenue and expenses, enabling apples-to-apples comparison across businesses. This skill uses the SLI account code structure to map Xero data into those categories.

## License

MIT