# Sheets vs Excel

Use Google Sheets when the user needs:

- browser-native collaboration
- Apps Script automation
- `QUERY`, `IMPORTRANGE`, or `GOOGLEFINANCE`
- lightweight dashboards and sharing

Use Excel/xlsx workflows when the user needs:

- downloadable `.xlsx` artifacts
- VBA or Power Query
- Excel-specific features or strict compatibility

When converting logic:

- re-check separators and dynamic array behavior
- do not assume every Excel formula maps directly to Sheets
- warn when a Sheets-native function has no clean Excel equivalent
