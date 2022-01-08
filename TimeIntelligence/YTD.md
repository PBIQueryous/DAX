# YTD
```javascript
YTD Earned | Total = 
VAR _YtD =
    MAX ( Dates[LatestMTD] ) -- last complete month YTD
VAR _Result =
    CALCULATE ( [ELIGIBLE CASH], Dates[Date] < _YtD ) -- eligible enrols
RETURN
    _Result
```
