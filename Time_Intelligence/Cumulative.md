# DAX Measure - Base Cumulative

### Cumulative Measure

#### hides future values after latest month to date

```js
£ In Delivery | Spent | CML =
// cumulative of [BaseSumMeasure]

VAR _maxDate = 
//  MAX( DateTable[DateColumn] )
    MAX ( _Dates[Date] )

VAR _result =
//  CALCULATE( [BaseSumMeasure], DatesTable[DateColumn] <= _varMaxDate )
    CALCULATE ( [BaseSumMeasure], _Dates[Date] <= _maxDate )
RETURN
    // _result
    
    // alternative result to hide blanks based on multiple conditions/measures
    IF (
        [MeasureA] <> 0
        || NOT ISBLANK ( [MeasureB] ),
        _result
    )

```
