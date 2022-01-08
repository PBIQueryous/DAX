# CFY (Current Financial Year)

```c#
measureName | CFY = 

// variable - latest month to date
VAR _YtD = CALCULATE( MAX( Dates[LatestMTD] ), REMOVEFILTERS ())

// expression, cumulative total filtered for current financial year
VAR _Result = CALCULATE ( [baseMeasure], KEEPFILTERS ( Dates[Date]<= _YtD ), Dates[IsCFY] = TRUE()) 

// result
RETURN
_Result
```
