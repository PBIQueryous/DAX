# DAX Measure - Latest Month to Date
### Cumulative Measure
#### hides future values after latest month to date

```js
MeasureCYTD | CYTD CML =
// cumulative current ytd of [MeasureCYTD]
VAR _maxDate =
    MAX ( _Dates[LatestMTD] )
VAR _result =
    CALCULATE ( [MeasureCYTD], _Dates[Date] <= _maxDate )
RETURN
    // _result
    IF ( NOT ISBLANK ( [MeasureCYTD] ), _result )

```
