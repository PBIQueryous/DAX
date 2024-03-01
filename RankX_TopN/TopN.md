# top N

```ioke
## Apps | Course TopN = 

var RankingContext = VALUES ( DIM_AppsProgramme ) 
var TopNumber = [TopN Value]
var _m1result =
    CALCULATE (
        [_SelectedMetric],
        TOPN ( TopNumber, ALL ( DIM_AppsProgramme ), [_SelectedMetric] , DESC ),
        RankingContext
    )
RETURN
    _m1result

```
