# REM
Measure to calculate the future remaining amount beyond the latest MTD. Good for comparing Actuals and Forecasts

```c#
MEASURE | REM = 

// variable: latest month to date
VAR _YtD = CALCULATE( MAX( Dates[LatestMTD] ) , REMOVEFILTERS ()) -- Last complete month

// expression
VAR _Result = 
  CALCULATE( [MEASURE], 
     KEEPFILTERS ( Dates[Date] >= _YtD )) 

// return result
RETURN
 _Result
 ```
