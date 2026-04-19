# Common errors and anti-patterns

## Formula issues

- `#REF!` from unauthorized `IMPORTRANGE`
- `QUERY` returning blanks because dates are text
- `ARRAYFORMULA` wrapped around already-spilling functions
- incorrect absolute/relative references
- full-column formulas on huge sheets causing lag

## Analysis issues

- hardcoded totals in summary tabs
- mixing actual and budget signs incorrectly
- churn calculated on ending customers instead of starting customers
- growth rates calculated against blanks or zero

## Apps Script issues

- cell-by-cell writes in loops
- destructive scripts without backups
- missing trigger or authorization guidance
- fetching external APIs without checking response codes
