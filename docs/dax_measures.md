## 📊 DAX Measures & Business Logic

This project utilizes advanced DAX (Data Analysis Expressions) to derive actionable insights. The measures are categorized into Profitability, Logistics, and Time Intelligence.

| Measure Name | DAX Formula | Description |
| :--- | :--- | :--- |
| **Total Profit** | `SUM(factSales[PROFIT])` | Calculates the gross profit across all sales transactions. |
| **Total Quantity** | `SUM(factSales[QUANTITY])` | The total number of units sold across all product categories. |
| **Total Order** | `COUNT(factSales[ORDER_KEY])` | Counts the total number of unique orders processed in the system. |
| **Total Customer** | `COUNT(customerDim[CUSTOMER_ID])` | Provides the total count of unique customers in the database. |
| **Avg Shipping Days** | `AVERAGEX(factSales, DATEDIFF(factSales[ORDER_DATE], factSales[SHIP_DATE], DAY))` | Measures logistics efficiency by calculating the average lead time between order and shipment dates. |
| **Profit Previous Month** | `CALCULATE([Total Profit], DATEADD('Calendar'[Date], -1, MONTH))` | Shifts the profit calculation to the previous month for month-over-month comparison. |
| **MOM Profit Variance** | `[Total Profit] - [Profit Previous Month]` | Measures the absolute change in profit compared to the prior month. |
| **MoM Profit Growth %** | `DIVIDE([MOM Profit Variance], [Profit Previous Month], 0)` | Calculates the percentage growth or decline in profit month-over-month. |
| **Profit Last Year** | `CALCULATE([Total Profit], SAMEPERIODLASTYEAR('Calendar'[Date]))` | Calculates the total profit for the same period in the previous year (YoY Analysis). |
| **Profit Variance** | `[Total Profit] - [profit last year]` | The absolute difference in profit between the current period and the same period last year. |
| **Profit Variance %** | `DIVIDE([profit Variance], [profit last year], 0)` | The percentage change in profit compared to the same period last year. |
| **Number Of Region** | `COUNT(geographyDim[REGION_ID])` | Counts the total number of unique operational regions. |
| **Total Number of City** | `COUNT(geographyDim[CITY_ID])` | Counts the total number of unique cities within the geography dimension. |
| **Total Number of Product**| `COUNT(productDim[PRODUCT_ID])` | Provides the total variety of products offered in the catalog. |

---

### 💡 Technical Highlights:
* **Time Intelligence:** Used `DATEADD` and `SAMEPERIODLASTYEAR` to enable dynamic trend analysis.
* **Row-Level Calculation:** `AVERAGEX` was used for "Avg Shipping Days" to ensure the `DATEDIFF` is calculated for every row in the Fact table before averaging, providing high accuracy.
* **Error Handling:** Used the `DIVIDE` function with an alternate result (0) to prevent "Division by Zero" errors in growth percentage visuals.
