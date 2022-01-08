# PMTD (previous month to date)

```c#
MEASURE | PMTD = 

// expression - previous month total year to date
TOTALMTD([baseMeasure],
  DATEADD( Dates[Date], -1,MONTH)
)
```
