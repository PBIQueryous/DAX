MoM& (month over month percentage)

```c#
MEASURE | MoM % =

// variable previous month value
VAR _PrevMonth =
    CALCULATE ( [baseMeasure], PREVIOUSMONTH ( Dates[Date] ) )

// variable current month value
VAR _CurrentMonth = [baseMeasure]

// variable difference to previous month
VAR _Calc = _CurrentMonth - _PrevMonth

// expression
VAR _Result =
    DIVIDE ( _Calc, _PrevMonth, BLANK () )

// result
RETURN
    IF ( _Result = -1, BLANK (), _Result )
```
