---
name: gsheets
description: >
  Use this skill when Google Sheets is the primary platform or the user explicitly wants a Sheets-native
  solution. Trigger for Google Sheets formulas, Apps Script automation, Sheets dashboards, debugging
  exported Google Sheet files, and Sheets-specific functions such as ARRAYFORMULA, QUERY, IMPORTRANGE,
  GOOGLEFINANCE, FILTER, UNIQUE, REGEXEXTRACT, MAP, BYROW, SCAN, and named ranges. Trigger when the
  user mentions Google Sheets, gsheets, Apps Script, IMPORTRANGE, QUERY, GOOGLEFINANCE, or asks how to
  do something in Sheets. Do not trigger when the required deliverable is a downloadable Excel workbook
  or when the user explicitly wants Excel-native behavior. In those cases, use the spreadsheet/xlsx path.
---

# Google Sheets Skill

## Purpose

This skill helps with Google Sheets-specific work in four repeatable modes:

1. **Formula mode**: write or debug Google Sheets formulas.
2. **Apps Script mode**: automate tasks that go beyond formulas.
3. **Analysis mode**: inspect uploaded CSV/XLSX data and propose a Sheets-native layout, formulas, and dashboard logic.
4. **Integration guidance mode**: explain how to connect Sheets workflows or use Sheets-native capabilities safely.

The goal is not just to answer spreadsheet questions. The goal is to deliver output the user can paste into Google Sheets with minimal rework.

## Trigger guidance

Use this skill when one or more of these are true:

- The user explicitly says **Google Sheets**, **gsheets**, or **Apps Script**.
- The user wants formulas using **ARRAYFORMULA**, **QUERY**, **IMPORTRANGE**, **FILTER**, **UNIQUE**, **GOOGLEFINANCE**, **REGEXEXTRACT**, **MAP**, **BYROW**, **SCAN**, or **named ranges**.
- The user uploaded a CSV/XLSX export from Google Sheets and wants the result to go back into Google Sheets.
- The user wants a Sheets-native dashboard, data validation, dropdowns, protected ranges, or lightweight automation.
- The user wants to convert Excel logic into Google Sheets syntax.

Do not use this skill when:

- The user wants an `.xlsx` file as the final artifact.
- The request is generic spreadsheet work with no Sheets-specific requirement.
- The user explicitly wants Excel-only features, Power Query, or VBA.

## Response modes

### 1. Formula mode

Use this when the task can be solved with formulas alone.

Deliver:
- The exact formula.
- Where to paste it.
- What assumptions it makes about headers and ranges.
- A safer variant if blanks, errors, or locale issues are likely.
- A short explanation of why the formula works.

Preferred style:
- Use Google Sheets syntax.
- Prefer dynamic formulas over manual fill-down when sensible.
- Protect against divide-by-zero and blank rows.
- Avoid volatile complexity unless it is truly needed.

### 2. Apps Script mode

Use this when the user needs:
- bulk edits
- custom menus
- scheduled jobs
- API calls
- automation triggered by edits or time
- formatting or protection logic across many cells

Deliver:
- A complete Apps Script function or file.
- Setup steps: **Extensions → Apps Script → paste → save → run**.
- Trigger guidance if needed.
- Notes on permissions, quotas, and safe testing.

Preferred style:
- Batch reads and writes with `getValues()` and `setValues()`.
- Do not do per-cell reads/writes in loops unless unavoidable.
- Use clear variable names and comments.
- Include error handling for destructive or external actions.

### 3. Analysis mode

Use this when the user uploads CSV/XLSX data but wants a Google Sheets outcome.

Deliver:
- Recommended tab structure.
- Cleaning steps.
- Key formulas or Apps Script snippets.
- Dashboard layout if relevant.
- Notes about any Google-native formulas that will not survive XLSX export/import cleanly.

Preferred style:
- Treat uploaded data as source data.
- Recommend a clean Sheets model rather than just describing the data.
- Separate **raw**, **clean**, **assumptions**, **calculations**, and **dashboard** tabs when the task is analytical.

### 4. Integration guidance mode

Use this when the user wants help connecting Sheets to other tools or workflows.

Deliver:
- Practical setup steps.
- Limitations and trade-offs.
- Safer alternatives when a setup is brittle or manual.

Avoid hard claims about time-sensitive product details unless verified elsewhere in the conversation.

## Formula patterns

### Array and expansion patterns

Use `ARRAYFORMULA` when the inner function does not spill automatically.

```gsheets
=ARRAYFORMULA(IF(A2:A="","",UPPER(A2:A)))
```

Prefer native spilling functions when possible:

```gsheets
=FILTER(A2:D, D2:D="Open")
=UNIQUE(A2:A)
=SORT(FILTER(A2:C, C2:C<>""), 3, FALSE)
```

### Lookup patterns

Single-key lookup:

```gsheets
=XLOOKUP(A2, Lookup!A:A, Lookup!B:B, "Not found")
```

Classic compatible lookup:

```gsheets
=INDEX(Lookup!B:B, MATCH(A2, Lookup!A:A, 0))
```

Multi-key lookup:

```gsheets
=INDEX(ReturnRange, MATCH(A2&B2, Key1Range&Key2Range, 0))
```

### Query patterns

Grouping and aggregation:

```gsheets
=QUERY(A1:D, "select A, sum(D) where A is not null group by A label sum(D) 'Total'", 1)
```

Month filter with date-safe helper column is often more reliable than fragile text-formatted dates inside `QUERY`.

### Cleaning patterns

```gsheets
=TRIM(CLEAN(A2))
=REGEXREPLACE(A2, "\s+", " ")
=REGEXEXTRACT(A2, "([A-Z]{3}-\d{4})")
=SPLIT(A2, ",")
=TEXTJOIN(", ", TRUE, B2:E2)
```

### Error-safe arithmetic

```gsheets
=IFERROR(B2/C2, "")
=IF(C2=0, 0, B2/C2)
```

### Date and period logic

```gsheets
=EOMONTH(A2, 0)
=TEXT(A2, "YYYY-MM")
=ROUNDUP(MONTH(A2)/3, 0)
```

### Financial analysis patterns

Month-on-month growth:

```gsheets
=IFERROR((B3/B2)-1, "")
```

Budget variance:

```gsheets
=B2-C2
```

Budget variance percent:

```gsheets
=IFERROR((B2-C2)/C2, "")
```

Runway using average burn:

```gsheets
=IF(AVERAGE(F2:F4)>=0, "Cash generative", CurrentCash/ABS(AVERAGE(F2:F4)))
```

### Import and external data patterns

```gsheets
=IMPORTRANGE("spreadsheet_url", "Raw!A:Z")
=IMPORTDATA("https://example.com/file.csv")
=GOOGLEFINANCE("NASDAQ:GOOG", "price")
```

Always warn that `IMPORTRANGE` requires one-time approval.

## Apps Script patterns

### Standard skeleton

```javascript
function main() {
  const ss = SpreadsheetApp.getActiveSpreadsheet();
  const sheet = ss.getActiveSheet();
  const values = sheet.getDataRange().getValues();
  Logger.log(`Loaded ${values.length} rows`);
}
```

### Bulk transform pattern

```javascript
function standardizeNames() {
  const sheet = SpreadsheetApp.getActiveSpreadsheet().getSheetByName('Raw');
  const range = sheet.getRange(2, 1, sheet.getLastRow() - 1, 1);
  const values = range.getValues();

  const cleaned = values.map(([name]) => [
    String(name || '').trim().replace(/\s+/g, ' ').toUpperCase()
  ]);

  sheet.getRange(2, 2, cleaned.length, 1).setValues(cleaned);
}
```

### Custom menu pattern

```javascript
function onOpen() {
  SpreadsheetApp.getUi()
    .createMenu('Finance Tools')
    .addItem('Refresh Summary', 'refreshSummary')
    .addToUi();
}
```

### API call pattern

```javascript
function fetchJson() {
  const url = 'https://api.example.com/data';
  const response = UrlFetchApp.fetch(url, { muteHttpExceptions: true });
  const code = response.getResponseCode();
  if (code !== 200) throw new Error(`API returned ${code}`);
  return JSON.parse(response.getContentText());
}
```

### Trigger guidance

- Use simple triggers like `onEdit(e)` only for lightweight operations.
- Use installable triggers for scheduled jobs, external calls, or privileged actions.
- Test on a copy before running scripts that overwrite data.

## Analytical structure guidance

For finance or operations analysis, recommend this tab pattern when relevant:

- `README` or `Instructions`
- `Raw_Data`
- `Clean_Data`
- `Assumptions`
- `Calculations`
- `Summary`
- `Dashboard`

Use this structure especially for:
- budget vs actual
- revenue analysis
- churn/cohort work
- cash flow and runway
- operational KPI dashboards

## Common pitfalls to catch

Always watch for:

- Excel formulas pasted into Sheets without adaptation.
- Locale separator issues, especially commas vs semicolons.
- `QUERY` breaking because dates are stored as text.
- Incorrect use of `ARRAYFORMULA` around already-spilling functions.
- Hardcoded totals instead of formulas.
- Full-column references in very heavy sheets causing slowdown.
- Apps Script loops doing cell-by-cell writes.
- `IMPORTRANGE` not authorized.
- Divide-by-zero in margin or growth calculations.
- Budget variance signs that hide whether a result is favorable or unfavorable.

## Quality standards

A good output for this skill should:

- be immediately usable in Google Sheets
- prefer stable, maintainable formulas
- separate data, assumptions, and outputs clearly
- explain where to paste formulas or scripts
- account for blanks, errors, and scaling issues
- avoid stale product claims unless verified elsewhere

## Complementary repo contents

This skill is designed to work with supporting examples, resources, and tests in this repository. Use them to keep answers practical rather than abstract.
