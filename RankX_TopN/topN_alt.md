# TopN Andrzej
```

[Running total % of interactions]:=
//Running total % of interactions
VAR _RunningTotal =
 SUMX (
 WINDOW (
 1,
 --from the 1st row (including)
 ABS,
 0,
 --to the current row (including)
 REL,
 ALLSELECTED ( 'Location'[State] ), --YOUR DIMENSION
 --order by State
 --from a state with max interactions to the state with min interactions
 ORDERBY ( [Count of Interactions], DESC ) --YOUR MEASURE
 ),
 [Count of Interactions] --YOUR MEASURE
 )
VAR _Total =
 --total interactions for all states
 CALCULATE (
 [Count of Interactions], --YOUR MEASURE
 ALL ( 'Location'[State] ) --YOUR DIMENSION
 )
VAR _Result =
 DIVIDE ( _RunningTotal, _Total )
RETURN
 _Result

Filter: [Running total % of interactions]<=60%

A measure for the title (subtitle):

//Chart subtitle: % of the interactions included
VAR _Included =
 CALCULATE (
 [Count of Interactions], --YOUR MEASURE
 ALLSELECTED ( 'Location'[State] ) --YOUR DIMENSION
 )
VAR _Total =
 --total interactions for all states
 CALCULATE (
 [Count of Interactions], --YOUR MEASURE
 ALL ( 'Location'[State] ) --YOUR DIMENSION
 )
VAR _Result =
 DIVIDE ( _Included, _Total )
VAR _Text = FORMAT ( _Result, "0%" ) & " (<=60%) of the interactions" 
RETURN
 _Text


```
