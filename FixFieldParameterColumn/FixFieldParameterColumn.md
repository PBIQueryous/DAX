```c#

Fields = 
var _x = RIGHT([Column1], LEN([Column1]) - SEARCH("'[",[Column1],,0))
var _y = SUBSTITUTE( _x , "[", "" )
var _z = SUBSTITUTE( _y , "]", "" )
RETURN
    _z

```
