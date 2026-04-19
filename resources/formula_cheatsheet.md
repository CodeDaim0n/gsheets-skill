# Formula cheatsheet

## High-value functions

- `ARRAYFORMULA`
- `FILTER`
- `UNIQUE`
- `SORT`
- `QUERY`
- `XLOOKUP`
- `INDEX`
- `MATCH`
- `SUMIFS`
- `COUNTIFS`
- `TEXTJOIN`
- `SPLIT`
- `REGEXEXTRACT`
- `REGEXREPLACE`
- `MAP`
- `BYROW`
- `SCAN`
- `IFERROR`
- `EOMONTH`
- `GOOGLEFINANCE`
- `IMPORTRANGE`

## Safe defaults

- Prefer helper columns over brittle mega-formulas when readability matters.
- Guard ratios with `IFERROR` or explicit zero checks.
- Prefer named ranges for reusable assumptions.
- Use finite ranges when very large sheets become slow.
