# Financial analysis case in Google Sheets

Use this pattern for a lightweight CFO pack.

## Suggested tabs

- `Raw_PnL`
- `Budget`
- `Customer_Metrics`
- `Cash`
- `Assumptions`
- `P&L_Summary`
- `SaaS_Metrics`
- `Forecast`
- `Dashboard`

## Core calculations

- Gross Profit = Revenue - COGS
- Gross Margin % = Gross Profit / Revenue
- EBITDA = Revenue - COGS - Sales & Marketing - R&D - G&A
- Variance = Actual - Budget
- Variance % = (Actual - Budget) / Budget
- Ending Customers = Starting + New - Churned
- Churn % = Churned / Starting
- MRR = Ending Customers * ARPA
- ARR = MRR * 12
- CAC = Sales & Marketing / New Customers
- Runway = Cash / Average burn

## Dashboard charts

- Revenue trend
- Gross margin trend
- EBITDA trend
- Budget vs actual revenue
- Customer growth and churn
- Cash balance trend

## Delivery guidance

When answering with formulas:
- state the destination tab and cell
- define the expected column layout
- include blank-safe formulas
- explain favorable vs unfavorable variance logic
