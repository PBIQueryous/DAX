# Non-standard Calendar (12months or more)

````javascript
CalendarSnaps = 
VAR FirstAcademicMonth = 8   -- First month of the Academic year
VAR MonthsInYear = 14      -- Must be 12 for GranularityByDate
                           -- can be different for GranularityByMonth
VAR CalendarFirstDate = MIN ( Dates[Date]  )
VAR CalendarLastDate = MAX ( Dates[Date]  )

VAR CalendarFirstYear = YEAR ( CalendarFirstDate )
VAR CalendarFirstMonth = MONTH ( CalendarFirstDate )
VAR CalendarLastYear = YEAR ( CalendarLastDate )
VAR CalendarLastMonth = MONTH ( CalendarLastDate )

-------------------------
-- Internal calculations
-------------------------
VAR GranularityByDate =
    ADDCOLUMNS (
        CALENDAR (
            DATE ( CalendarFirstYear, CalendarFirstMonth, 1 ),
            EOMONTH (
                DATE ( CalendarLastYear, CalendarLastMonth, 1 ),
                0
            )
        ),
        "Original Year Month Number", YEAR ( [Date] ) * MonthsInYear
            + MONTH ( [Date] ) - 1
    )
VAR GranularityByMonth =
    SELECTCOLUMNS (
        GENERATESERIES (
            CalendarFirstYear * MonthsInYear + CalendarFirstMonth - 1 
                - (MonthsInYear - 12) * (CalendarFirstMonth < FirstAcademicMonth),
            CalendarLastYear * MonthsInYear + CalendarLastMonth - 1
                - (MonthsInYear - 12) * (CalendarLastMonth < FirstAcademicMonth),
            1
        ),
        "Original Year Month Number", [Value]
    )
RETURN GENERATE (
    GranularityByMonth,
    VAR YearMonthNumber = [Original Year Month Number]
    VAR AcademicMonthNumber = 
        MOD (
            YearMonthNumber + 1
                * (FirstAcademicMonth > 1)
                * (MonthsInYear + 1 - FirstAcademicMonth),
            MonthsInYear
        ) + 1
    VAR AcademicYearNumber = 
        QUOTIENT ( 
            YearMonthNumber + 1 
                * (FirstAcademicMonth > 1) 
                * (MonthsInYear + 1 - FirstAcademicMonth),
            MonthsInYear
        ) 
    VAR OffsetAcademicMonthNumber = MonthsInYear + 1 - (MonthsInYear - 12)
    VAR MonthNumber =
        IF ( 
            AcademicMonthNumber <= 12 && FirstAcademicMonth > 1,
            AcademicMonthNumber + FirstAcademicMonth
                - IF ( 
                    AcademicMonthNumber > (OffsetAcademicMonthNumber - FirstAcademicMonth), 
                    OffsetAcademicMonthNumber, 
                    1 
                ),
            AcademicMonthNumber
        )
    VAR YearNumber = AcademicYearNumber - 1 * (MonthNumber > AcademicMonthNumber)
    VAR YearMonthKey = YearNumber * 100 + MonthNumber
    
    VAR MonthDate = DATE ( YearNumber, MonthNumber, 1 )
    VAR AcademicQuarterNumber = MIN ( ROUNDUP ( AcademicMonthNumber / 3, 0 ), 4 )
    VAR AcademicYearQuarterNumber = AcademicYearNumber * 4 + AcademicQuarterNumber - 1
    VAR AcademicMonthInQuarterNumber = MOD ( AcademicMonthNumber - 1, 3 ) + 1 + 3 * (MonthNumber > 12)
    VAR MonthInQuarterNumber = MOD ( MonthNumber - 1, 3 ) + 1 + 3 * (MonthNumber > 12)
    
    -- Special handling Calendar has 12 months while Academic has 14 months
    VAR CalendarMonthNumber = 
        IF (
            MonthNumber > 12,
            (FirstAcademicMonth - 1 ) + 12 * (FirstAcademicMonth = 1),
            MonthNumber
        )
    VAR QuarterNumber = MIN ( ROUNDUP ( CalendarMonthNumber / 3, 0 ), 4 )
    VAR YearQuarterNumber = YearNumber * 4 + QuarterNumber - 1
    
    RETURN ROW (
        "Year Month Key", YearMonthKey,
        "Year", YearNumber,
        "Year Quarter", FORMAT ( QuarterNumber, "\Q0" ) & "-" & FORMAT ( YearNumber, "0000" ),
        "Year Quarter Number", YearQuarterNumber,
        "Quarter", FORMAT ( QuarterNumber, "\Q0" ),
	    "Year Month", FORMAT ( 
	        IF (
	            MonthNumber > 12,
	            DATE ( YearNumber, CalendarMonthNumber, 1 ),
	            MonthDate
	        ), 
	        "mmm yyyy" 
	    ),
	    "Calendar Year Month Number", YearNumber * 12 + CalendarMonthNumber - 1,
	    "Month", FORMAT ( 
	        IF (
	            MonthNumber > 12,
                DATE ( YearNumber, FirstAcademicMonth, 1 ) - 1,
	            MonthDate
	        ), 
	        "mmm" 
	    ),
        "Month Number", CalendarMonthNumber,
        "MonthInQuarterNumber", MonthInQuarterNumber,
        "Academic Year", FORMAT ( AcademicYearNumber, "\A\Y 0000" ),
        "AcYrNumber", AcademicYearNumber,
        "Academic Year Quarter", FORMAT ( AcademicQuarterNumber, "\A\Q0" ) & "-"
	        & FORMAT ( AcademicYearNumber, "0000" ),
	    "AcademicYearQuarterNumber", AcademicYearQuarterNumber,
        "Academic Quarter", FORMAT ( AcademicQuarterNumber, "\A\Q0" ),
	    "AcademicYearMonth", IF (
	        MonthNumber > 12,
	        FORMAT ( MonthNumber, "\R00" ) & FORMAT ( YearNumber, " 0000" ),
	        FORMAT ( MonthDate, "mmm yyyy" )
	    ),
        "AcademicYearMonthNumber", YearMonthNumber,
        "Academic Month", IF (
	        MonthNumber > 12,
	        FORMAT ( MonthNumber, "\R00" ),
	        FORMAT ( MonthDate, "mmm" )
	    ),
	    "Academic Month Number", AcademicMonthNumber,
        "Return", "R" & AcademicMonthNumber,
        "AcademicMonthInQuarterNumber", AcademicMonthInQuarterNumber,
        "AcademicYear", AcademicYearNumber-1 & "/" & RIGHT(AcademicYearNumber,2),
        "AcYrReturnKEY", INT(AcademicYearNumber-1 & RIGHT(AcademicYearNumber,2) & FORMAT(AcademicMonthNumber, "00")),
        "YrReturnKEY", INT(AcademicYearNumber-1 & FORMAT(AcademicMonthNumber, "00")),
        "AcYrKEY", INT(AcademicYearNumber-1 & RIGHT(AcademicYearNumber,2)),
        "Date", DATE ( YearNumber, CalendarMonthNumber, 1 ),
        "Year and Return", AcademicYearNumber-1 & "/" & RIGHT(AcademicYearNumber,2) & " - " & "R" & AcademicMonthNumber
    )
)
```
