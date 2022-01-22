# RAG (Conditional Formatting)
Font colour formatting based on variables - good for bar/bullet charts

```c
@RAG = 
VAR _Red = -0.500
VAR _Amber = -0.160
VAR _Green = -1

RETURN
    SWITCH (
        TRUE,
       [Measure] < _Red, "Red",
       [Measure] < _Amber, "Dark Orange",
       [Measure] >= _Amber, "Green",
        BLANK()
    )
```
