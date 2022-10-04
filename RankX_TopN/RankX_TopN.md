# RankX and TopN
## RankX and TopN measure

```Ruby

TopN | Earned = 
VAR _TopN = 'TopN'[TopN Value]
VAR _Rank =
    RANKX ( ALL ( _Providers ), [YTD Earned | Total],, DESC )
VAR _Calc =
    CALCULATE (
        [YTD Earned | Total],
        TOPN ( _TopN, VALUES ( _Providers[Provider Name] ), [YTD Earned | Total], DESC )
    )
RETURN
    IF ( _Rank > _TopN, BLANK (), _Calc )
```
