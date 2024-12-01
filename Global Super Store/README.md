# **Global SuperStore Dashboard**

### **Dashboard Link:** [Global SuperStore Dashboard](https://app.powerbi.com/view?r=eyJrIjoiNTI0MTUwMTktNjBkMy00NmRiLWEwZjEtYmQ5OWU4NDAwNjM5IiwidCI6IjM5Y2MyYmM1LWE0MTMtNDA0NS1hNWJhLWQwMTk2OGVlMjhlZCJ9)

---

## **Project Overview**

The **Global Superstore Dashboard** was created to provide actionable insights into sales, profit, and operational performance across various regions and segments. It serves as a dynamic reporting tool, enabling stakeholders to analyze trends, identify growth opportunities, and monitor key performance indicators (KPIs).

---

## **Key Features**

### 1. **Dynamic KPIs**
- **Total Sales**: Displays the total revenue generated.
- **Total Profit**: Summarizes the overall profitability.
- **Profit Margin**: Calculates the profitability percentage using the formula:
  ```DAX
  Profit Margin = DIVIDE(SUM(Profit), SUM(Sales)) * 100
  ```
- **Total Orders**: Represents the number of transactions recorded.

### 2. **Regional Insights**
- **Sales by Region**:
  - Bar chart visualizing revenue distribution across regions.
  - Helps identify high-performing and underperforming areas.
- **Profit by Region**:
  - A heatmap showcasing profitability, making it easier to pinpoint areas needing improvement.

### 3. **Segment Analysis**
- **Sales and Profit by Segment**:
  - Stacked column charts comparing sales and profit across Consumer, Corporate, and Home Office segments.

### 4. **Category and Sub-Category Analysis**
- **Top Categories**:
  - Bar charts highlighting the best-performing categories in terms of sales and profit.
- **Sub-Category Drilldown**:
  - Allows users to drill down into specific sub-categories to analyze their contribution.

### 5. **Time-Series Analysis**
- **Monthly Sales Trend**:
  - Line chart showing sales trends over months.
  - Provides insights into seasonal patterns and peak periods.
- **Year-over-Year Growth**:
  - Calculated measure to track growth using:
    ```DAX
    YoY Growth = 
    DIVIDE(
      SUM(Sales) - CALCULATE(SUM(Sales), SAMEPERIODLASTYEAR(Date[Date])),
      CALCULATE(SUM(Sales), SAMEPERIODLASTYEAR(Date[Date]))
    ) * 100
    ```

### 6. **Customer Insights**
- **Customer Count**:
  - Card visual indicating the total number of unique customers.
- **Customer Segmentation**:
  - Clustered bar chart categorizing customers by region and segment for deeper insights.

### 7. **Operational Performance**
- **Order Processing Time**:
  - Average time taken to fulfill orders, calculated as:
    ```DAX
    Avg Processing Time = AVERAGE(DATEDIFF(Order Date, Ship Date, DAY))
    ```
- **Shipping Mode Analysis**:
  - Pie chart breaking down orders by shipping modes (Standard, Express, Same-Day).

### 8. **Geographical Visualization**
- **World Map**:
  - Map visual with bubbles representing sales volume by country.
  - Bubble size varies based on revenue, providing a quick overview of geographical performance.

### 9. **Interactive Filters**
- Filters for dynamic analysis:
  - **Date Range**: Allows users to filter data by specific time periods.
  - **Region**: Drill down into specific regions for detailed insights.
  - **Category and Sub-Category**: Focus on specific product lines.

---

## **Calculated Measures**


### **Basic Measures**

1. **Total Sales**:
   - Aggregates the sales revenue across all transactions.
   ```DAX
   Total Sales = SUM(Sales)
   ```

2. **Total Profit**:
   - Calculates the overall profit.
   ```DAX
   Total Profit = SUM(Profit)
   ```

3. **Average Discount**:
   - Computes the average discount applied across all transactions.
   ```DAX
   Avg Discount = AVERAGE(Discount)
   ```


### **Complex Measures**

4. **Year-over-Year Sales Growth (YoY Growth)**:
   - Measures the percentage growth in sales compared to the previous year.
   ```DAX
   YoY Growth = 
   DIVIDE(
      SUM(Sales) - CALCULATE(SUM(Sales), SAMEPERIODLASTYEAR(Date[Date])),
      CALCULATE(SUM(Sales), SAMEPERIODLASTYEAR(Date[Date]))
   ) * 100
   ```

5. **Cumulative Sales**:
   - Tracks the running total of sales over time.
   ```DAX
   Cumulative Sales = 
   CALCULATE(
      SUM(Sales),
      FILTER(
         ALL(Date[Date]),
         Date[Date] <= MAX(Date[Date])
      )
   )
   ```

6. **Rolling 12-Month Sales**:
   - Computes the total sales over the last 12 months for trend analysis.
   ```DAX
   Rolling 12-Month Sales = 
   CALCULATE(
      SUM(Sales),
      DATESINPERIOD(Date[Date], LASTDATE(Date[Date]), -12, MONTH)
   )
   ```

7. **Dynamic Rank for Sales**:
    - Ranks regions dynamically based on their sales performance.
    ```DAX
    Sales Rank = 
    RANKX(
       ALL(Region),
       SUM(Sales),
       ,
       DESC,
       Dense
    )
    ```

8. **Customer Lifetime Value (CLV)**:
    - Combines multiple factors like sales, profit, and frequency of transactions to compute the lifetime value of a customer.
    ```DAX
    CLV = 
    SUMX(
       CustomerTable,
       (Total Sales + Total Profit) * FrequencyFactor
    )
    ```

9. **Average Order Value (AOV)**:
    - Determines the average revenue per order.
    ```DAX
    AOV = DIVIDE(SUM(Sales), COUNT(OrderID))
    ```


### **Operational Measures**

10. **Average Order Processing Time**:
    - Calculates the average time taken to process and ship orders.
    ```DAX
    Avg Processing Time = 
    AVERAGE(DATEDIFF(OrderDate, ShipDate, DAY))
    ```

11. **Orders Delivered On Time (%)**:
    - Measures the percentage of orders delivered within the expected time.
    ```DAX
    On Time Delivery (%) = 
    DIVIDE(
       COUNTROWS(FILTER(Table, DeliveryStatus = "On Time")),
       COUNTROWS(Table)
    ) * 100
    ```


### **Advanced Measures**

12. **Profit Variance Analysis**:
    - Compares profit for the current period with a benchmark or previous period.
    ```DAX
    Profit Variance = 
    SUM(Profit) - CALCULATE(SUM(Profit), SAMEPERIODLASTYEAR(Date[Date]))
    ```

13. **Top Product Contribution**:
    - Calculates the contribution of top-performing products to overall sales.
    ```DAX
    Top Product Contribution = 
    DIVIDE(
       CALCULATE(SUM(Sales), TOPN(10, ProductTable, SUM(Sales), DESC)),
       SUM(Sales)
    ) * 100
    ```

14. **Segmental Sales Growth**:
    - Tracks growth in sales for each segment dynamically.
    ```DAX
    Segment Sales Growth = 
    DIVIDE(
       SUM(Sales) - CALCULATE(SUM(Sales), SAMEPERIODLASTYEAR(Date[Date])),
       CALCULATE(SUM(Sales), SAMEPERIODLASTYEAR(Date[Date]))
    )
    ```

15. **Shipping Mode Analysis**:
    - Analyzes the distribution of shipping modes.
    ```DAX
    Shipping Mode % = 
    DIVIDE(
       COUNTROWS(FILTER(Table, ShippingMode = "Specific Mode")),
       COUNTROWS(Table)
    ) * 100
    ```


### Importance of These Measures
- **Strategic Decision-Making**: Complex measures like YoY Growth and CLV help management make long-term strategic decisions.
- **Trend Analysis**: Measures like Rolling 12-Month Sales and Cumulative Sales provide insights into sales trends and seasonality.
- **Operational Efficiency**: Metrics like Average Processing Time and On-Time Delivery % highlight areas for operational improvement.
- **Customer Insights**: Calculated metrics like AOV and CLV provide a deeper understanding of customer behavior and value.

---

## **Visualizations Used in the Dashboard**

1. **Stacked Column Chart**: 
   - Visualizes sales and profit distribution across categories and regions.

2. **Cards**:
   - Displays key metrics such as Total Sales, Total Profit, and Total Orders.

3. **Funnel Chart**:
   - Shows the sales process flow or order fulfillment stages.

4. **Maps**:
   - Bubble maps display sales volume by location, and filled maps show performance intensity geographically.

5. **Tables**:
   - Provides detailed data with metrics like sales, profit, and discounts.

6. **Line Charts**:
   - Tracks trends over time for metrics like sales and profit.

7. **Donut Charts**:
   - Represents proportional data like category-wise or region-wise sales distribution.

8. **Pie Charts**:
   - Displays contributions of various categories to total sales.

9. **Clustered Bar Chart**:
   - Compares metrics like sales and profit across multiple categories side by side.

10. **Gauge**:
    - Tracks KPIs like profit margin against target thresholds.

11. **Tree Map**:
    - Visualizes hierarchical sales contributions by category and subcategory.

12. **Stacked Area Chart**:
    - Displays cumulative metrics over time, such as total sales and profit trends.

13. **Slicers**:
    - Enables filtering by dimensions like Region, Category, or Time Period.

14. **Sliders**:
    - Provides date or range selection for time-specific analysis.

15. **Waterfall Chart**:
    - Highlights incremental changes in profit or revenue by categories or regions.

16. **Stacked Bar Chart**:
    - Shows contributions across multiple categories horizontally for better comparisons.

17. **Axis Tables**:
    - Combines numerical and categorical data for detailed analysis with axis-based relationships.

---

## **Benefits of the Dashboard**

1. **Enhanced Decision-Making**:
   - Provides real-time insights into sales, profit, and customer behavior.
2. **Operational Efficiency**:
   - Highlights bottlenecks in order processing and delivery.
3. **Strategic Planning**:
   - Helps identify growth opportunities by analyzing segment and category performance.

---

## **How to Use the Dashboard**

1. Navigate to the [Dashboard Link](https://app.powerbi.com/view?r=eyJrIjoiNTI0MTUwMTktNjBkMy00NmRiLWEwZjEtYmQ5OWU4NDAwNjM5IiwidCI6IjM5Y2MyYmM1LWE0MTMtNDA0NS1hNWJhLWQwMTk2OGVlMjhlZCJ9).
2. Use the slicers and filters to customize your view.
3. Explore visualizations to gain insights into sales, profit, and operations.
4. Drill down into specific regions, segments, or time periods for detailed analysis.

---

## **Files in the Repository**

- `Global SuperStore.pbix`: The Power BI project file for the dashboard.
- `Dataset`: The dataset used for this project.
---

Feel free to contribute, raise issues, or share feedback to enhance this project further! 🎉
