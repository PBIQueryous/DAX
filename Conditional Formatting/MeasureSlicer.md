# Measure Slicer
Using a disconnected Table to apply measure slicers

## Not optimised

```js
@ Earned Cash = 
VAR _Max = CALCULATE( MAX( Dates[Date] ) , Dates[IsCFY] = TRUE)
VAR _Ytd = [YTD Earned | Total]
VAR _F1 = [£ Earned | F+1]
VAR _F2 = [£ Earned | F+2]
VAR _F3 = [£ Earned | F+3]
VAR _EFY = [£ EFY | CML]
VAR _EFYMax = [£ EFY + Ach | CML]
RETURN
    SWITCH (
        TRUE,
        SELECTEDVALUE ( 'Metric Slicer'[Earned Metric] ) = "YTD", _Ytd,
        SELECTEDVALUE ( 'Metric Slicer'[Earned Metric] ) = "P+1", _F1,
        SELECTEDVALUE ( 'Metric Slicer'[Earned Metric] ) = "P+2", _F2,
        SELECTEDVALUE ( 'Metric Slicer'[Earned Metric] ) = "P+3", _F3,
        SELECTEDVALUE ( 'Metric Slicer'[Earned Metric] ) = "EFY", _EFY,
        SELECTEDVALUE ( 'Metric Slicer'[Earned Metric] ) = "Max", _EFYMax,
        BLANK ()
    )
```

## Optimised

```js
@ Earned Cash = 
VAR _Max = CALCULATE( MAX( Dates[Date] ) , Dates[IsCFY] = TRUE)
VAR _Ytd = [YTD Earned | Total]
VAR _F1 = [£ Earned | F+1]
VAR _F2 = [£ Earned | F+2]
VAR _F3 = [£ Earned | F+3]
VAR _EFY = [£ EFY | CML]
VAR _EFYMax = [£ EFY + Ach | CML]
VAR _SelectedItem = SELECTEDVALUE ( 'Metric Slicer'[Earned Metric] )
RETURN
    SWITCH ( _SelectedItem,
        
        "YTD", _Ytd,
        "P+1", _F1,
        "P+2", _F2,
        "P+3", _F3,
        "EFY", _EFY,
        "Max", _EFYMax,
        BLANK ()
    )
    
```
