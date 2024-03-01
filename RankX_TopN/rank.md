# Rank DAX

```ioke
## Apps | Rank TopN = 
var _isVisible = NOT( ISBLANK( [_SelectedMetric] ))
var _m1=
IF(
        _isVisible ,
RANK(
    DENSE,
    ALLSELECTED( DIM_AppsProgramme ),
    ORDERBY ( [_SelectedMetric]   , DESC )
)
)
RETURN
    _m1

```
