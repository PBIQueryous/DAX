# progress bar

```ioke
Progress Bar = 
VAR MAXGrandTotal = CALCULATE(SUM(_temp[Actual] ), ALL(_temp) )
VAR MAXActual = SUM(_temp[Actual] )
VAR GTPct = DIVIDE(MAXActual, MAXGrandTotal)
VAR GTPctFmt = IF(GTPct = 1, FORMAT(GTPct, "0%"), FORMAT(GTPct, "0.0%"))
VAR AXISRANGE = MAXX(
    {
        MAXGrandTotal,
        MAXActual
    },
    [Value]
)
VAR TRACKWIDTH = MAXGrandTotal/AXISRANGE*123
VAR PERCENTAGEFILL = MAXActual/AXISRANGE*123
VAR SVGImage =
"data:image/svg+xml;utf8," & "
    <svg width='200' height='25' viewBox='-0 -2 200 20' xmlns='http://www.w3.org/2000/svg' xmlns:xlink='http://www.w3.org/1999/xlink' overflow='visible'>
        <rect id='track' x='0' y='1' width=' " & TRACKWIDTH & "' height='20' fill='#efefef'/>
        <rect id='fill' x='0' y='5' width='" & PERCENTAGEFILL & "' height='12' fill='#384268'></rect>
        <text x='128' y='15' font-family='Helvetica' font-size='xx-medium' font-weight='bold' >"& GTPctFmt & "</text> 
    </svg>
"
VAR SVGNoTotal = IF(HASONEVALUE(_temp[Category]), SVGImage)
RETURN
SVGNoTotal

```
