# Formula patterns

## Lookup patterns

Single key:

```gsheets
=XLOOKUP(A2, Lookup!A:A, Lookup!B:B, "Not found")
```

Fallback compatible pattern:

```gsheets
=INDEX(Lookup!B:B, MATCH(A2, Lookup!A:A, 0))
```

Multi-key pattern:

```gsheets
=INDEX(Return!C:C, MATCH(A2&B2, Source!A:A&Source!B:B, 0))
```

## Dynamic row logic

```gsheets
=ARRAYFORMULA(IF(A2:A="",,B2:B*C2:C))
```

## Query examples

```gsheets
=QUERY(A1:E, "select B, sum(E) where A is not null group by B pivot C", 1)
```

## Rolling metrics

Trailing 3-month average:

```gsheets
=IF(ROW()<4, "", AVERAGE(B2:B4))
```

## Finance examples

Gross margin percent:

```gsheets
=IFERROR((Revenue-COGS)/Revenue, "")
```

Variance percent:

```gsheets
=IFERROR((Actual-Budget)/Budget, "")
```

Run-rate ARR:

```gsheets
=MRR*12
```
