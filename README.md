# DAX Functions and Power BI Samples

## DAX Functions

### Folder: 01_DAX_DimTime

```
DimDate =
    var _today = TODAY()
    var startYear = YEAR ( MIN(orders[date]) )  //ระบุคอลัมน์ของวันที่ขาย
    var endYear = YEAR( MAX(orders[date]) )
    // CALENDAR(DATE(2020,01,01), DATE(2025,12,31))
    RETURN
    ADDCOLUMNS (
        CALENDAR(
            DATE(startYear,1,1),
            DATE(endYear,12,31)
        ),
        "DateID", FORMAT([Date], "YYYYMMDD"),
        "DateTH", FORMAT([Date], "dd/mm/yyyy", "th-TH"),
        "Year", YEAR([Date]),
        "Month", FORMAT([Date], "mmm"),
        "Month-Full", FORMAT([Date], "mmmm"),
        "MonthID", MONTH([Date]),
        "MonthYear", FORMAT([Date], "mmm yyyy"),
        "MonthYearID", INT(FORMAT([Date], "yyyymm")),
        "QuarterYearTH", "Q" & FORMAT([Date], "q yyyy", "th-TH"),
        "QuarterYear", "Q" & FORMAT([Date], "q yyyy"),
        "QuarterYearID", INT(FORMAT([Date], "yyyyq")),
        "DayName", FORMAT([Date],"ddd"),
        "DayName-Full", FORMAT([Date],"dddd"),
        "DayNum", WEEKDAY([Date])
    )
    //Powered by Ampon(Tle) Training

```
