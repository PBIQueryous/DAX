# PowerBI DAX
PBI DAX patterns
This repo contains custom (template) expressions in DAX, which can be used in PowerBI.

| Theme      | Description |
| ---------- |----------------------------------------------------------|
| Time-intelligence | DAX time-intelligence expressions, from YTD, FTYD, CML and much more. <br/> **NOTE:** All my time-intelligence measures are based on this my Dates Table here > [datesTable](https://github.com/PBIQueryous/M-Code/tree/main/Calendars) |
| [Functions](https://github.com/PBIQueryous/M-Code/tree/main/Functions) | M-code related transformations and functions |

---

## Common Templates
Examples of Tabular Editor Templates for DAX
### Simple version



#### Selected Measures
```c#
/*
 * Title: Auto-generate SUM measures from measures
 * 
 * Author:
 * Imran Haq, PBIQueryous
 * 
 * Inspiration and Credits:
 * ------------------------
 * PowerBI.Tips Team, Curbal, Chandoo, Goodly, SQLBI, 
 * Enterprise DNA, GreySkullBI, The PowerBI Guy and so many more!
 * Daniel Otykier, twitter.com/DOtykier,
 * Mike Carlo, Tommy Puglia, Seth
 * Tabular Editor 2/3 Documentation,
 * ------------------------
 * 
 * This script, when executed, will loop through the currently selected MEASURES,
 * creating one SUM measure for each MEASURE.
 */
/******** SCRIPT START ********/
// Creates time intelligence measures for every selected measure:
    foreach(var m in Selected.Measures) 
    {
    m.Table.AddMeasure(
        // Name
        m.Name + "Name Affix",
        
        // DAX expression // 
        "var _Result = CALCULATE( " + m.DaxObjectName " )"
         + '\n' + "RETURN"
         + '\n' + "'\t' _Result",
        
        // Display Folder
        m.DisplayFolder)
        
        // Format String
        .FormatString = "0.00";
}
/******** SCRIPT END ********/
```


#### Selected Columns
```c#
/*
 * Title: Auto-generate SUM measures from columns
 * 
 * Author:
 * Imran Haq, PBIQueryous
 * 
 * Inspiration and Credits:
 * ------------------------
 * PowerBI.Tips Team, Curbal, Chandoo, Goodly, SQLBI, 
 * Enterprise DNA, GreySkullBI, The PowerBI Guy and so many more!
 * Daniel Otykier, twitter.com/DOtykier,
 * Mike Carlo, Tommy Puglia, Seth
 * Tabular Editor 2/3 Documentation,
 * ------------------------
 * 
 * This script, when executed, will loop through the currently selected COLUMNS,
 * creating one SUM measure for each COLUMNS.
 */
/******** SCRIPT START ********/
// Creates time intelligence measures for every selected column:
    foreach(var c in Selected.Columns) 
    {
    c.Table.AddMeasure(
        // Name
        c.Name + "Name Affix",
        
        // DAX expression // 
        "var _Result = CALCULATE( " + c.DaxObjectName " )"
         + '\n' + "RETURN"
         + '\n' + "'\t' _Result",
        
        // Display Folder
        c.DisplayFolder)
        
        // Format String
        .FormatString = "0.00";
}
/******** SCRIPT END ********/
```


### Advanced version
```c#
/*
 * Title: Auto-generate SUM measures from columns
 * 
 * Author:
 * Imran Haq, PBIQueryous
 * 
 * Inspiration and Credits:
 * ------------------------
 * PowerBI.Tips Team, Curbal, Chandoo, Goodly, SQLBI, 
 * Enterprise DNA, GreySkullBI, The PowerBI Guy and so many more!
 * Daniel Otykier, twitter.com/DOtykier,
 * Mike Carlo, Tommy Puglia, Seth
 * Tabular Editor 2/3 Documentation,
 * ------------------------
 * 
 * This script, when executed, will loop through the currently selected COLUMNS,
 * creating one SUM measure for each column and also hiding the column itself.
 */
 
/******** SCRIPT START ********/
// Loop through all currently selected columns:
foreach(var c in Selected.Columns)
{
    var newMeasure = c.Table.AddMeasure(
        "Sum of " + c.Name,                    // Name
        "SUM(" + c.DaxObjectFullName + ")",    // DAX expression
        c.DisplayFolder                        // Display Folder
    );
    
    // Set the format string on the new measure:
    newMeasure.FormatString = "0.00";

    // Provide some documentation:
    newMeasure.Description = "This measure is the sum of column " + c.DaxObjectFullName;

    // Hide the base column:
    c.IsHidden = true;
}
/******** SCRIPT END ********/
```


### Complex version (Case-specific)
```c#
/*
 * Title: Auto-generate SUM measures from columns
 * 
 * Author:
 * Imran Haq, PBIQueryous
 * 
 * Inspiration and Credits:
 * ------------------------
 * PowerBI.Tips Team, Curbal, Chandoo, Goodly, SQLBI, 
 * Enterprise DNA, GreySkullBI, The PowerBI Guy and so many more!
 * Daniel Otykier, twitter.com/DOtykier,
 * Mike Carlo, Tommy Puglia, Seth
 * Tabular Editor 2/3 Documentation,
 * ------------------------
 * 
 * This script, when executed, will loop through the currently selected COLUMNS,
 * creating one SUM measure for each column and also hiding the column itself.
 */
 
/*# FULL SCRIPT START #*/
// Script Variable
// Creates a series of time intelligence measures for each selected (base SUM) measure:
foreach(var m in Selected.Measures) 

    {   // SCRIPT START
    
/***************************************** MeasureStart ************************************/
// Measure1: SUM
    var m1 = m.Table.AddMeasure
    (                             
        // MeasureStart //
        // MeasureName
        m.Name + " | SUM",                               
    
        // Measure Descriptive Text
        '\n' + "// Base SUM "                           
        
        /***** DAX expression START *****/
        // DAX Start
        // DAX Variables
        + '\n' + '\n' + calcVarMaxDateFY                
        + '\n' + "VAR _result = CALCULATE( " + m.DaxObjectName + " ) " + '\n'
        
        // Return Result
        + '\n' + rReturn
        + '\n' + '\t' + "// IF(  NOT ISBLANK( " + m.DaxObjectName + " ) ,  _result  )"
        + '\n' + '\t' + rResult
    );
        /***** DAX expression END *****/
        
        // Display Folder (default - same folder as selected)
        m1.DisplayFolder 
        // Optional: new Folder name below
        = "Base" 
        ;      
    
        /***** Provide some documentation *****/
        m1.Description = "Derived from " + m.Name + ": " + 
        // Type metadata text here
        "Base Measure - End for Year, no filters."
        ;                             
        m1.FormatString = Currency
        ; 
    }                                               
/**************************************** MeasureEnd **************************************/
/*# FULL SCRIPT END #*/
