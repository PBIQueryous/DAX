# YTD conditional formatting
Use this measure as conditional formatting in table values.
YTD values show BLUE, values for next 3 months show TEAL, remaining future values show GREY

```c#
@ActualForecastColour = 
// variable end month, 3 months after lastest MTD
VAR _YtD3 = LASTDATE( Dates[MTDAdd3] )

// variable latest month to date
VAR _YtD = LASTDATE( Dates[LatestMTD] )

// expression
RETURN
SWITCH(  
    TRUE(), 
    SELECTEDVALUE(Dates[MonthStart]) <  _YtD, "Blue", --dark blue (hex optional)
    SELECTEDVALUE(Dates[MonthStart]) > _YtD3,"Light Grey", --light grey (hex optional)
    "#3FB6C9") --teal (hex)
```
