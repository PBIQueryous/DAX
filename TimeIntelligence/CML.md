# CML (cumulative)

```c#
MEASURE | CML = 

// variable latest availabe date in Dates table
VAR _YtD = CALCULATE( MAX( Dates[Date] ) /*,  Dates[IsCFY] = TRUE */ )

// expression
VAR _Result = CALCULATE ( [baseMeasure], Dates[Date]<= _YtD ) 


RETURN
/* IF(  NOT ISBLANK( [baseMeasure] ) ,  _Result  ) */
_Result
```
