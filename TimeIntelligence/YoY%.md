YoY% (year over ear percentage)

```c#
MEASURE | YoY % =

// variable previous year value
VAR _Prev =
    CALCULATE ( [baseMeasure], SAMEPERIODLASTYEAR ( Dates[Date] ) )

// variable current year value
VAR _Curr = [baseMeasure]

// variable difference to previous year
VAR _Calc = _Curr - _Prev

// expression
VAR _Result =
    DIVIDE ( _Calc, _Pre, BLANK () )

// result
RETURN
    IF ( _Result = -1, BLANK (), _Result )
```
