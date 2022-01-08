# WTD (week to date)

```c#
MEASURE | WTD =

// expression
IF (
    // year and week & year number from Dates table
    HASONEVALUE ( Dates[Year] ) && HASONEVALUE ( Dates[WeekYearN] ),
    
    //
    CALCULATE (
        [baseMeasure],
        
        // table filter
        FILTER (
            ALL ( Dates ),
            
            // filter to match year and week & year number
            Dates[Year] = VALUES ( Dates[Year] )
                && Dates[WeekYearN] = VALUES ( Dates[WeekYearN] )
                && Dates[Date] <= MAX ( Dates[Date] )
        )
    )
)
```
