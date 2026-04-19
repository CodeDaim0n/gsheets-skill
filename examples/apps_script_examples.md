# Apps Script examples

## 1. Clean vendor names

```javascript
function cleanVendorNames() {
  const sheet = SpreadsheetApp.getActiveSpreadsheet().getSheetByName('Raw_Data');
  const values = sheet.getRange(2, 1, sheet.getLastRow() - 1, 1).getValues();
  const output = values.map(([v]) => [String(v || '').trim().replace(/\s+/g, ' ').toUpperCase()]);
  sheet.getRange(2, 2, output.length, 1).setValues(output);
}
```

## 2. Rebuild monthly summary tab

```javascript
function refreshSummary() {
  const ss = SpreadsheetApp.getActiveSpreadsheet();
  const raw = ss.getSheetByName('Raw_Data');
  let summary = ss.getSheetByName('Summary');
  if (!summary) summary = ss.insertSheet('Summary');
  summary.clear();
  summary.getRange('A1').setValue('Rebuild summary formulas here');
}
```

## 3. Add menu

```javascript
function onOpen() {
  SpreadsheetApp.getUi()
    .createMenu('Sheet Tools')
    .addItem('Clean Vendors', 'cleanVendorNames')
    .addItem('Refresh Summary', 'refreshSummary')
    .addToUi();
}
```

## 4. Send summary email

```javascript
function sendSummaryEmail() {
  const recipient = 'finance@example.com';
  const subject = 'Weekly finance summary';
  const body = 'Review the updated Google Sheet summary tab.';
  MailApp.sendEmail(recipient, subject, body);
}
```

## Notes

- Batch reads and writes.
- Test destructive operations on a copy.
- Add installable triggers for scheduled work.
