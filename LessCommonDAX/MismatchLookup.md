# Mismatch LookUp
DAX Calculated Column - checks for a mismatches between column(s) in TableA and TableB, 


## Variation 1
```ts
Deviation1 = 
var _check = 
MAXX(
    FILTER(
        OccReportBI,
        OccReportBI[Learning aim reference] = QualsFM35[Learning Aim Ref Number]
        && OccReportBI[UKPRN] = QualsFM35[UKPRN]
        && OccReportBI[SSA Tier1] = QualsFM35[SSAT1]
    ),
    OccReportBI[Provision Types]
)
var _result = IF( _check = [Provision Type], BLANK(), _check)
RETURN
    _result
```

## Variation 2
```ts
Deviation2 = 
var _check = 
MINX(
    FILTER(
        OccReportBI,
        OccReportBI[Learning aim reference] = QualsFM35[Learning Aim Ref Number]
        && OccReportBI[UKPRN] = QualsFM35[UKPRN]
        && OccReportBI[SSA Tier1] = QualsFM35[SSAT1]
    ),
    OccReportBI[Provision Types]
)
var _result = IF( _check = [Deviation1] || _check = [Provision Type], BLANK(), _check)
RETURN
    _result
 ```
