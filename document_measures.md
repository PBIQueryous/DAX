# INFO.MEASURES() - Measures Schema
## Document Semantic Model Measures

```ioke
EVALUATE
VAR _measures =
    ADDCOLUMNS (
        SELECTCOLUMNS (
            INFO.MEASURES (),
            "Measure", [Name],
            "DataType", [DataType],
            "Desc", [Description],
            "DAX formula", [Expression],
            "TableID", [TableID],
            "Display Folder", [DisplayFolder]
        ),
        "Data Type",
            SWITCH (
                [DataType],
                2, "String",               -- string
                6, "Int64",                -- whole
                8, "Double",               -- pct
                9, "DateTime",             -- dateTime
                10, "Currency",            -- float
                11, "Boolean"              -- boolean
            ),
        "SubFolders",
            RIGHT (
                [Display Folder],
                LEN ( [Display Folder] ) - SEARCH ( "\", [Display Folder], 1, 0 )
            )
    )
    
VAR _tables =
    SELECTCOLUMNS ( INFO.TABLES (), "TableID", [ID], "Table", [Name] )
VAR _combined =
    NATURALLEFTOUTERJOIN ( _measures, _tables )
RETURN
    SELECTCOLUMNS (
        _combined,
        "Measure", [Measure],
        "Data Type", [Data Type],
        "Desc", [Desc],
        "DAX Formula", [DAX formula],
        "Home Table", [Table],
        "Display Folder", [Display Folder],
        "SubFolders", [SubFolders]
    )

```
