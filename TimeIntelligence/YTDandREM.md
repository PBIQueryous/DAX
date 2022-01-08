# YTD & REM (Actual YTD & Forecast REM)
Useful for having both the Actual YTD and remaining future VALUE

```c#
Actual & Forecast | YTD = 

// variable latest availabe date in Dates table
VAR _YtD = CALCULATE( MAX( Dates[LatestMTD] ) , REMOVEFILTERS ()) -- Last complete month

// Actual for YTD (LatestMTD)
VAR _Actual = [measureActual]

// Forecast or VALUE remaining (anything after LatestMTD)
VAR _Forecast = 
 CALCULATE( [measureRemainder] , 
     KEEPFILTERS ( Dates[Date]>= _YtD )
 ) 

// expression
VAR _Result = _Actual + _Forecast

// result
RETURN
 _Result
```
