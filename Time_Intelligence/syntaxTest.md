# test DAX syntax highlighting
### -
#### -

## (ioke)
```ioke               

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
## (objectivej)
```objectivej 

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
## (ocaml)
```ocaml

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
## (PHP)
```php

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
## (VALA)
```vala

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
## (SMALLTALK)
```smalltalk

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
