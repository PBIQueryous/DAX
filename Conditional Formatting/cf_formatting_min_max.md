# min / max / actual_forecat

```ruby
CF_TEST = 
var _rankMax =
RANK(
    SKIP,
    ADDCOLUMNS(
        ALLSELECTED(
            DIM_Dates[Month Start]
        ),
        "RankOrder",
        [ActualYTD]
    ),
    ORDERBY( [Actual],DESC BLANKS LAST )
)
var _rankMin = 
RANK(
    SKIP,
    ADDCOLUMNS(
        ALLSELECTED(
            DIM_Dates[Month Start]
        ),
        "RankOrder",
        [ActualYTD]
    ),
    ORDERBY( [Actual],ASC BLANKS LAST )
)
var _result =
SWITCH(
    TRUE(),
    _rankMax = 1, "#8FBC8F",
    _rankMin = 1, "#FF6347",
    SELECTEDVALUE(DIM_Dates[MonthOFFSET]) == 0, "ORANGE",
    SELECTEDVALUE(DIM_Dates[MonthOFFSET]) < 0, "#B3B3B3",
    SELECTEDVALUE(DIM_Dates[MonthOFFSET]) > 0, "#FFFFF0",
    SELECTEDVALUE( DIM_Dates[Month Start] ) <= TODAY(), "#404040",
    "#F3F3F3"
)
RETURN
_result

```
