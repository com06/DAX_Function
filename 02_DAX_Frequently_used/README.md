# DAX Functions and Power BI Samples

## Folder: 02_DAX_Frequently_used

### SUM
สูตร SUM ช่วยให้คุณสามารถหาผลรวมของคอลัมน์ที่กำหนด
```
TotalSales = SUM(Sales[Revenue])
```
### AVERAGE
สูตร AVERAGE ช่วยให้คุณสามารถหาค่าเฉลี่ยของคอลัมน์ที่กำหนด
```
AverageRevenue = AVERAGE(Sales[Revenue])
```
### COUNT
สูตร COUNT ช่วยให้คุณสามารถนับจำนวนแถวที่มีข้อมูลในคอลัมน์ที่กำหนด
```
TotalOrders = COUNT(Sales[OrderID])
```
### CALCULATE
สูตร CALCULATE ช่วยให้คุณสามารถคำนวณค่าตามเงื่อนไขที่กำหนด 
ในตัวอย่างนี้ สูตร CALCULATE คำนวณยอดขายรวม (SUM) ของคอลัมน์ Revenue โดยเลือกเฉพาะแถวที่มีค่า CustomerType เป็น “VIP” ผลลัพธ์ที่ได้คือยอดขายรวมสำหรับลูกค้า VIP เท่านั้น
```
TotalSalesVIP = CALCULATE(SUM(Sales[Revenue]), Sales[CustomerType] = "VIP")
```
### MAX
สูตร MAX ช่วยให้คุณหาค่าสูงสุดในคอลัมน์ที่กำหนด
```
MaxRevenue = MAX(Sales[Revenue])
```
### MIN
สูตร MIN ช่วยให้คุณหาค่าต่ำสุดในคอลัมน์ที่กำหนด
```
MinRevenue = MIN(Sales[Revenue])
```
### RANKX
สูตร RANKX ช่วยให้คุณคำนวณอันดับของข้อมูลตามเงื่อนไขที่กำหนด
```
ProductRank = RANKX(ALL(Sales[Product]), Sales[Revenue], , DESC)
```
### DIVIDE
สูตร DIVIDE ช่วยให้คุณหารค่าและจัดการกับการหารด้วยศูนย์ได้อย่างถูกต้อง
```
ProfitMargin = DIVIDE(Sales[Profit], Sales[Revenue])
```
### ALL
สูตร ALL ช่วยให้คุณลบกรองที่ใช้ในตารางหรือคอลัมน์เพื่อคำนวณข้อมูลในมุมมองที่แตกต่าง
```
TotalSalesAllRegions = CALCULATE(SUM(Sales[Revenue]), ALL(Sales[Region]))
```

### การคำนวณ Year-over-Year Growth
สูตรนี้ช่วยคำนวณเปอร์เซ็นต์การเติบโตในยอดขายระหว่างปีปัจจุบันและปีก่อนหน้า
```
YoY_Growth =
VAR CurrentYearSales = CALCULATE(SUM(Sales[Revenue]), SAMEPERIODLASTYEAR('Date'[Date]))
VAR PreviousYearSales = CALCULATE(SUM(Sales[Revenue]), SAMEPERIODLASTYEAR('Date'[Date]),
SAMEPERIODLASTYEAR('Date'[Date]))
RETURN DIVIDE(CurrentYearSales - PreviousYearSales, PreviousYearSales)
```
### การคำนวณสัดส่วนของยอดขายต่อประเภทสินค้า
สูตรนี้ช่วยคำนวณสัดส่วนของยอดขายของแต่ละประเภทสินค้าเทียบกับยอดขายรวม
```
ProductCategoryShare =
VAR TotalSales = SUM(Sales[Revenue])
VAR CategorySales = CALCULATE(SUM(Sales[Revenue]), ALLEXCEPT(Sales, Sales[ProductCategory]))
RETURN DIVIDE(CategorySales, TotalSales)
```
### การคำนวณ Moving Average จาก 3 เดือน
สูตรนี้ช่วยคำนวณค่าเฉลี่ยเคลื่อนไหว (Moving Average) จากยอดขายในระยะเวลา 3 เดือน
```
ThreeMonthsMovingAverage =
CALCULATE(
   AVERAGE(Sales[Revenue]),
   DATESINPERIOD('Date'[Date],
LASTDATE('Date'[Date]), -3, MONTH)
)
```
### การคำนวณยอดขายรวมแยกตามปี
สูตรนี้ช่วยคำนวณยอดขายรวมสำหรับแต่ละปี โดยที่ผลลัพธ์จะแยกตามปี
```
TotalSalesByYear =
CALCULATE(
SUM(Sales[Revenue]),
ALLSELECTED('Date'[Year])
)
```
### การคำนวณสัดส่วนของยอดขายของพนักงานขาย
สูตรนี้ช่วยคำนวณสัดส่วนของยอดขายของแต่ละพนักงานขายเทียบกับยอดขายรวมของทุกพนักงานขาย
```
SalesPersonShare =
VAR TotalSales = SUM(Sales[Revenue])
VAR SalesPersonSales = CALCULATE(SUM(Sales[Revenue]), ALLEXCEPT(Sales, Sales[SalesPerson]))
RETURN DIVIDE(SalesPersonSales, TotalSales)
```
### การคำนวณเปอร์เซ็นต์การเติบโตระหว่างไตรมาส
สูตรนี้ช่วยคำนวณเปอร์เซ็นต์การเติบโตของยอดขายระหว่างไตรมาสปัจจุบันและไตรมาสก่อนหน้า
```
QoQ_Growth =
VAR CurrentQuarterSales = CALCULATE(SUM(Sales[Revenue]), SAMEPERIODLASTYEAR('Date'[Date]))
VAR PreviousQuarterSales = CALCULATE(SUM(Sales[Revenue]), SAMEPERIODLASTYEAR('Date'[Date]),
SAMEPERIODLASTYEAR('Date'[Date]))
RETURN DIVIDE(CurrentQuarterSales - PreviousQuarterSales, PreviousQuarterSales)
```
### การคำนวณกำไรสะสม
สูตรนี้ช่วยคำนวณกำไรสะสมสำหรับแต่ละวัน
```
CumulativeProfit =
CALCULATE(
  SUM(Sales[Profit]),
  FILTER(
   ALLSELECTED('Date'[Date]),
  'Date'[Date] <= MAX('Date'[Date])
  )
)
```
### การคำนวณสัดส่วนของยอดขายต่อลูกค้า
สูตรนี้ช่วยคำนวณสัดส่วนของยอดขายของแต่ละลูกค้าเทียบกับยอดขายรวม
```
CustomerShare = VAR TotalSales = SUM(Sales[Revenue]) VAR CustomerSales = CALCULATE(SUM(Sales[Revenue]), ALLEXCEPT(Sales, Sales[Customer])) RETURN DIVIDE(CustomerSales, TotalSales)
```
### การคำนวณสัดส่วนของยอดขายต่อภูมิภาค
สูตรนี้ช่วยคำนวณสัดส่วนของยอดขายของแต่ละภูมิภาคเทียบกับยอดขายรวม
```
RegionShare =
VAR TotalSales = SUM(Sales[Revenue])
VAR RegionSales = CALCULATE(SUM(Sales[Revenue]), ALLEXCEPT(Sales, Sales[Region]))
RETURN DIVIDE(RegionSales, TotalSales)
```
### การคำนวณจำนวนสินค้าที่ขายดีที่สุด 10 อันดับ
สูตรนี้ช่วยคำนวณจำนวนสินค้าที่ขายดีที่สุด 10 อันดับจากยอดขายรวม
```
Top10Products =
CALCULATETABLE(
   VALUES(Sales[Product]),
   TOPN(10, ALL(Sales[Product]),
[TotalSales], DESC)
)
```
### การคำนวณค่าเฉลี่ยของยอดขายต่อวันในสัปดาห์
สูตรนี้ช่วยคำนวณค่าเฉลี่ยของยอดขายต่อวันในแต่ละสัปดาห์
```
AverageSalesPerWeekday =
AVERAGEX(
   SUMMARIZE(Sales, 'Date'[Weekday],
   "DailySales", SUM(Sales[Revenue])),
[DailySales]
)
```
### EDATE
แสดงวันที่ซึ่งเป็นจำนวนเดือนที่ระบุก่อนหรือหลังวันที่เริ่มต้นเหมาะกับการคำนวณวันที่ถึงกำหนดหรือวันที่ครบกำหนดซึ่งเป็นวันเดียวกันของเดือนในวันที่ระบุ
```
ExpiryDate = EDATE(<start_date>, <months>)
```
start_date คือ วันที่ในรูปแบบ วันที่เวลา หรือ ข้อความ ที่แสดงถึงวันที่เริ่มต้น  
months คือ จำนวนเต็มที่แสดงถึงจำนวนเดือนก่อนหรือหลัง start_date
  
https://quickerpthailand.com/blog-power-bi-dax/  
  
https://www.9experttraining.com/articles/dax-functions-a-to-z