# YTD
```javascript
YTD Earned | Total = 

// variable for end of latest month using dates table
VAR _YtD =
    MAX ( Dates[LatestMTD] ) 

// expression
VAR _Result =
    CALCULATE ( [BASE TOTAL], Dates[Date] < _YtD )
    
// return result
RETURN
    _Result
```
