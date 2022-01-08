# PFYTD (previous financial year to date)

```c#
MEASURE | PFYTD = 

// variable latest month to date
VAR _YtD = MAX( Dates[LatestMTD] )

// expression
VAR _Result = 
  CALCULATE( [baseMeasure], 
  DATESYTD( 
    SAMEPERIODLASTYEAR( Dates[Date]), 
    "31/7" )
  )

// return result
RETURN
      IF(  NOT ISBLANK( [baseMeasure] ) , _Result )
```
