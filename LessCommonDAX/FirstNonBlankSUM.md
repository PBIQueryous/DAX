# FirstNonBlank
Using SUMX to iterate through each row, captures the first non-blank item of each row in the specific column/table and aggregates these items

```javascript
Achievement = 

// latest mtd variable
VAR _LatestDate =
    CALCULATE ( MAX ( Dates[LatestMTD] ), REMOVEFILTERS () ) -- variable for last completed month (latest MTD)

// expression
VAR _Calc =
    SUMX (
        // column with unique items to iterate
        DISTINCT ( Tbl[COLUMN1] ), 
        
        // iterates across every unique enrolment
        CALCULATE (
             // filters/captures first Achievement Element value in column
            FIRSTNONBLANK ( Tbl[COLUMN2], 0 ), -- column to retrieve first non blank value
           
           // keeps rowfilters in table for all dates before LatestMTD
            KEEPFILTERS ( Dates[Date] <= _LatestDate ) -- optional YTD filter
            
            )
        )
 
 // return result
 RETURN
      _Calc
```
