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
```c#
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
 * This script, when executed, will loop through the currently selected columns,
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
