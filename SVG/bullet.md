# svg bullet

  ```ioke
SVG Bullet = 
//initial DAX came from Kerry's site here, this version has been modified: https://kerrykolosko.com/portfolio/progress-bars/
//my dataset has no "plan", but if yours does you can use it below
//this version has a bar color that changes to green when over 100%
//VAR MAXPlan = [Profit Plan]
VAR MAXActual = SUM( _temp[Actual] )
VAR MAXTarget = SUM( _temp[Plan] )
var varColour = IF( MAXActual < MAXTarget , "Orange", "#24E41D" )
VAR AXISRANGE = MAXX(
    {
       // MAXPlan,
        MAXActual,
        MAXTarget
    },
    [Value]
)
//VAR TRACKWIDTH = MAXPlan/AXISRANGE*100
VAR PERCENTAGEFILL = MAXActual/AXISRANGE*118  //Note: the number that is being multiplied here is essentially what you want the max width of these to be; I have it a couple pixels smaller than the width of the track so that there is a tiny bit of breathing room
VAR PERCENTAGETARGET = MAXTarget/AXISRANGE*118
VAR BARCOLOR = IF(PERCENTAGEFILL > 100, "navy", "#b5b5b5")
var _otherBar = IF(PERCENTAGEFILL > 100, "Yellow", "Pink")
VAR svg = "data:image/svg+xml;utf8," & "
    <svg xmlns='http://www.w3.org/2000/svg' xmlns:xlink='http://www.w3.org/1999/xlink' overflow='visible'>
        <rect id='track' x='0' y='3' width='120' height='18' fill='#efefef'/> 
        <rect id='fill' x='0' y='7' width="& "'"& PERCENTAGEFILL &"'"&" height='10' fill='" & BARCOLOR & "'></rect>
        <rect id='marker' x="&"'"&PERCENTAGETARGET&"'"&" y='2' width='2' height='20' fill='" & varColour & "'></rect>
    </svg>"

RETURN
//Note: In Kerry's example, the track line below referenced "&TRACKWIDTH&", which I have commented out (this data has no "plan", so I just set the width to a static value)
//she also had a HASONEVALUE() in the return clause to prevent a "totals" bar from being displayed; I took that out so that we have an overall progress bar
//previously, you needed to set a width and height for the SVG in the SVG tag, but since we're now setting it with visual settings we can remove it (it was width='120' height='24' previously)
IF(ISFILTERED(_temp[Category]) = FALSE(), BLANK(), svg)

  ```
