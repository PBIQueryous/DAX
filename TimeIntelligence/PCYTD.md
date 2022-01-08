# PCYTD (previous calendar year to date)

```c#
MEASURE | PCYTD = 

// expression
CALCULATE(
  TOTALYTD( [baseMeasure], 
  SAMEPERIODLASTYEAR(Dates[Date])
  )
)
```
