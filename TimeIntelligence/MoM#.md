# MoM# (month over month value)

```c#
MEASURE | MoM # =

// variable previous month amount
VAR _PrevMonth =
    CALCULATE ( [baseMeasure], PREVIOUSMONTH ( Dates[Date] ) )

// variable current month amount
VAR _CurrentMonth = [baseMeasure]

// expression
VAR _Result = _CurrentMonth - _PrevMonth

result
RETURN
    IF ( NOT ISBLANK ( [baseMeasure] ), _Result )
```
