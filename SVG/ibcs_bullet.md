# ibcs bullet 

```ioke
IBCS Bullet = 
//Colors
VAR BaseMetric = "#3f4850" // Dark Grey
VAR VarBelow = "#24E41D" //Green
VAR VarAbove = "#F74B4B" //Red
var _deltaHeight = 45
//Metrics
VAR Actual = SUM( _temp[Actual] )
VAR Comparison = SUM( _temp[Plan] )
VAR ComparisonActualVar = Comparison - Actual
var _variance = Actual - Comparison
var _varFMT = FORMAT( _variance , "+#0,0; -#0,0" )
VAR MAXActual = MAXX( SUMMARIZE( ALL(_temp), _temp[Category]), SUM( _temp[Actual] ))

//SVG Stuff
VAR AXISRANGE = MAXX(
    {
        Comparison,
        MAXActual
    },
    [Value]
)
//Keeps all bar sizes relative to the min/max sizes for all rows
VAR PlanBarWidth = Comparison/AXISRANGE*100
VAR ActualBarWidth = Actual/AXISRANGE*100
VAR VarBarWidth = ABS(ComparisonActualVar/AXISRANGE*100)
//Determines the width of the variance bar, using ABS to convert the number to positive since SVG length doesn't work with negatives
VAR VarBarStart = IF(ComparisonActualVar >= 0, ActualBarWidth, PlanBarWidth)
//If the actual is <= comparison, the variance bar starts at the end of the actual bar, else starts at the end of the comparison bar
VAR VarBarColor = IF(ComparisonActualVar >= 0, VarAbove, VarBelow)
//If the actual is <= comparison, the variance bar gets VarBelow color, else gets VarAbove color

VAR SVGImage = 
"data:image/svg+xml;utf8," & "
    <svg width='100%' height='100%' xmlns='http://www.w3.org/2000/svg' xmlns:xlink='http://www.w3.org/1999/xlink' overflow='visible'>
        <rect id='fill' x='0' y='4' width='" & ActualBarWidth & "' height='75%' fill='" & BaseMetric & "'></rect>
        <rect id='track' x='" & VarBarStart & "' y='4' width='" & VarBarWidth & "' height='" & _deltaHeight & "%' fill='" & VarBarColor & "'/>
        <text x='110' y='20' font-family='Helvetica' font-size='xx-medium' font-weight='bold' >"& _varFMT & "</text> 
    </svg>
"
RETURN
IF(ISFILTERED(_temp[Category]) = FALSE(), BLANK(), SVGImage)
```
