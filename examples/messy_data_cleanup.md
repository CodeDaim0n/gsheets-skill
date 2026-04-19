# Messy data cleanup patterns

Typical issues in Google Sheets exports:

- month values mixed as text and dates
- currency symbols embedded in numeric fields
- duplicate header rows after pasted exports
- whitespace and casing issues in names
- negative numbers represented with parentheses

## Useful formula snippets

Normalize whitespace:

```gsheets
=ARRAYFORMULA(IF(A2:A="",,TRIM(CLEAN(A2:A))))
```

Strip currency symbols:

```gsheets
=ARRAYFORMULA(IF(B2:B="",,VALUE(REGEXREPLACE(B2:B, "[^0-9.-]", ""))))
```

Convert `(123)` to `-123`:

```gsheets
=ARRAYFORMULA(IF(C2:C="",,VALUE(REGEXREPLACE(REGEXREPLACE(C2:C, "\((.*)\)", "-$1"), "[^0-9.-]", ""))))
```
```
