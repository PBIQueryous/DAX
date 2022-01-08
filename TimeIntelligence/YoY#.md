# YoY (year over year value)

```c#
MEASURE | YoY # =

// variable previous year amount
VAR _Prev =
    CALCULATE ( [baseMeasure], SAMEPERIODLASTYEAR ( Dates[Date] ) )

// variable current month amount
VAR _Curr = [baseMeasure]

// expression
VAR _Result = _Curr - _Prev

result
RETURN
    IF ( NOT ISBLANK ( [baseMeasure] ), _Result )
```
