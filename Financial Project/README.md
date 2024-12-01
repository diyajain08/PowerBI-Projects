# Financial Dashboard

### Dashboard Link: 
[To view Dashboard click here](https://app.powerbi.com/view?r=eyJrIjoiNDJkM2ZkNmItNjQ2Ny00YjgwLWFmMWYtMzZmNjRmMjVkNWNmIiwidCI6IjM5Y2MyYmM1LWE0MTMtNDA0NS1hNWJhLWQwMTk2OGVlMjhlZCJ9)

## Problem Statement

The Financial Dashboard was designed to address a critical need for streamlining financial data processing and analysis. The client faced inefficiencies in handling daily data from multiple sources, leading to time delays, increased costs, and reduced accuracy. This project automates the process and delivers a dynamic Power BI dashboard for real-time financial insights.

---

## Project Overview

### Challenges Faced by the Client:
1. **Time-Consuming Workflow**: Daily manual downloading, merging, and cleaning of data delayed dashboard updates.
2. **High Costs**: Hiring additional staff to meet deadlines increased monthly expenses by $12,000.
3. **Errors in Data**: Manual manipulation led to inaccuracies in financial reporting.
4. **Workload Management**: Other projects suffered due to the heavy workload.

### Goals:
1. Automate data handling and integration to save time.
2. Minimize errors in data processing.
3. Reduce costs by eliminating manual intervention.
4. Provide actionable insights through a dynamic financial dashboard.

---

### Automation Flow

The automation flow is a key component of this project, designed to address the client's challenges of manual data processing, time inefficiency, and high error rates. Here’s a detailed explanation of the automation workflow:

---

#### **Step 1: Email Automation**
1. **Custom Rule in Outlook**:
   - Created a rule in Microsoft Outlook to automatically sort incoming emails with specific keywords in the subject or body (e.g., "Financial Data") into a dedicated folder.
   - This step ensures that all relevant files are organized and accessible without manual intervention.

2. **Daily Trigger**:
   - Power Automate monitors the designated folder for new emails containing financial data attachments.
   - Trigger: `When a new email arrives (V2)` action in Power Automate.


#### **Step 2: Google Drive Integration**
1. **Uploading Files**:
   - Power Automate is configured to automatically extract attachments from the emails and upload them to a specific folder in Google Drive.
   - Ensured the folder is structured for seamless data organization (e.g., by date or department).

2. **Folder Synchronization**:
   - The Google Drive folder acts as a centralized repository for all financial data files, ensuring that no files are missed during processing.


#### **Step 3: API Connection**
1. **Google Cloud API Setup**:
   - Created an API in Google Cloud Platform (GCP) to act as a bridge between Google Drive and Python.
   - Generated an API key to securely access the files stored in Google Drive.

2. **Python Integration**:
   - Developed Python scripts to fetch data from Google Drive via the API.
   - The script:
     - Downloads the latest files from the Drive folder.
     - Merges them into a single dataset for further processing.


#### **Step 4: Data Processing in Python**
1. **Data Cleaning**:
   - The Python script cleans the merged dataset:
     - Handles null values and inconsistencies.
     - Formats the data for compatibility with Power BI.
   - Key Python libraries used: `pandas`, `numpy`, and `google-auth`.

2. **Automated File Preparation**:
   - After cleaning and transforming the data, the script saves the processed file in a location accessible by Power BI.


#### **Step 5: Power BI Integration**
1. **Dynamic Data Loading**:
   - Power BI Desktop is linked to the processed dataset via Python or a live connection to Google Drive.
   - Configured to refresh data automatically whenever updates are made to the source file.

2. **Scheduled Refresh**:
   - Scheduled daily refreshes in Power BI Service to pull the latest data from Google Drive or Python output, ensuring the dashboard always displays real-time insights.


#### **Step 6: Real-Time Reporting**
1. **Automated Updates**:
   - Stakeholders receive a fully updated dashboard without manual intervention, ready for analysis by the specified time (e.g., 8 PM daily).
2. **Error Reduction**:
   - Eliminates manual errors by automating the entire data handling and processing workflow.


### Steps Followed in Power BI

1. **Data Integration**:
   - Connected Power BI to the Google Drive folder via Python API to automate data retrieval.
   - Ensured dynamic updates to the dataset whenever new files are uploaded to Google Drive.

2. **Data Cleaning and Preparation**:
   - Used Power Query Editor for transformations:
     - Checked for column quality, distribution, and profiling.
     - Filtered out irrelevant or null data fields for accurate analysis.
   - Created calculated columns to group customers into predefined age categories: "Teen," "Young Adult," "Old Adult," etc.

3. **Visualizations**:
   - **Key KPIs**:
     - Displayed Average Annual Income, Monthly Balance, Credit Utilization, and Payment Delays using card visuals.
   - **Charts**:
     - Created bar charts to analyze credit mix and age demographics.
     - Used distribution plots for age group analysis.
   - **Interactive Filters**:
     - Incorporated slicers for fields such as Credit Mix, Age Group, and Payment Behavior to enable customized insights.

---

### Calculated Columns and Measures

#### **Calculated Columns**
1. **Age Grouping**:
   - Segmented customers into predefined age categories for targeted analysis using the following logic:
     - 14–19: "Teen"
     - 19–25: "Young Adult"
     - 25–35: "Old Adult"
     - 35–45: "Old1"
     - >45: "Old2"
   - DAX formula used:
     ```DAX
     Age Group = 
     IF(Age <= 19, "Teen",
     IF(Age <= 25, "Young Adult",
     IF(Age <= 35, "Old Adult",
     IF(Age <= 45, "Old1", "Old2"))))
     ```

2. **Customer Categorization**:
   - Grouped customers based on payment behavior to analyze trends in financial habits.

3. **Credit Score Categories**:
   - Calculated categories for credit scores (e.g., "Good," "Average," "Poor") based on predefined thresholds.
   - Example DAX formula:
     ```DAX
     Credit Score Category = 
     IF(CreditScore >= 750, "Good",
     IF(CreditScore >= 600, "Average", "Poor"))
     ```

4. **Loan Types Count**:
   - Created columns to categorize customers by the number of loans they held, useful for trend analysis.

#### **Calculated Measures and Columns**
1. **LTV Score**:
   - Designed to evaluate customer lifetime value (LTV) using the formula:
     ```DAX
     LTV = (0.3 * [Average Annual Income]) - 
           (0.15 * [Average Payment Delay Days]) + 
           (0.4 * [Average Credit Score]) + 
           (0.075 * [Average Investment Amount]) + 
           (0.075 * [Average Monthly Balance])
     ```
   - Used to segment customers into promotional tiers based on thresholds:
     - LTV > 80,000: Tier 1 Promotion
     - LTV 60,000–80,000: Tier 2 Promotion
     - LTV 50,000–60,000: Tier 3 Promotion

2. **Average Payment Delay**:
   - Calculated the average days of payment delay for different credit categories:
     ```DAX
     Avg Delay = AVERAGE(PaymentDelayDays)
     ```

3. **Credit Utilization**:
   - Determined the average percentage of credit utilization across customer groups:
     ```DAX
     Credit Utilization = 
     AVERAGE(CreditUtilized / CreditLimit * 100)
     ```

4. **Loan Dispersal by Type**:
   - Counted the total number of loans dispersed for each loan type:
     ```DAX
     Loan Count = COUNTROWS(FILTER(Table, Table[LoanType] = "SpecificType"))
     ```

5. **Customer Distribution**:
   - Measured the percentage distribution of customers across age groups:
     ```DAX
     Customer Percentage = 
     DIVIDE(COUNT(Customers[ID]), COUNTROWS(Customers)) * 100
     ```

6. **Inquiry Rate Analysis**:
   - Identified potential customers based on average inquiry rates:
     ```DAX
     Avg Inquiry = AVERAGE(Inquiries)
     ```

7. **Credit Card Ownership**:
   - Calculated the average number of credit cards held by customers in different age groups:
     ```DAX
     Avg Credit Cards = AVERAGE(CreditCardsOwned)
     ```

8. **Promotional Insights**:
   - Calculated counts for each promotional tier based on LTV scores for targeted campaigns.

---

#### **Insights Derived from Calculations**
- Segmentation by age group and credit score helped identify high-value customer groups.
- Analysis of payment behavior and delays provided insights into financial risk management.
- Calculations for promotional tiers enabled targeted marketing strategies, enhancing customer engagement.

These calculated columns and measures added depth to the dashboard, ensuring meaningful insights and actionable recommendations for the client.

5. **Age-Specific Analysis**:
   - Segmented customers into age groups based on the rules provided.
   - Calculated potential customers based on inquiry averages (>7.5) and linked this to promotional strategies.

6. **Loan and Credit Card Trends**:
   - Analyzed the number and types of loans dispersed.
   - Compared the frequency of credit card ownership by age group.

7. **LTV-Based Promotions**:
   - Developed visuals and calculations to classify customers into promotion eligibility categories based on their LTV scores.

8. **Publishing**:
   - Published the dashboard to Power BI Service, enabling real-time data updates and accessibility to stakeholders. 

---

## Key Deliverables

### Insights from the Dashboard:
1. **Customer Behavior**:
   - Variations in credit utilization, delays in payment, and credit card ownership by age groups.
   
2. **LTV Analysis**:
   - Promotions tailored to LTV scores:
     - LTV > 80,000: 30% off + home loan at 4% interest.
     - LTV between 60,000–80,000: 15% off + gift hampers.
     - LTV between 50,000–60,000: Loans at 5% interest.
     
3. **Potential Customers**:
   - Identified key age groups for loan targeting based on inquiry rates (>7.5 average).

4. **Loan Dispersal**:
   - Visualized counts for various loan types to prioritize offerings.

---

## Files Included

1. `financialproject.pbix`: Power BI Desktop file containing the dashboard.
2. `Financial_Dataset_Code.ipynb`: Python script for automation and data processing.
3. `Financial Dashboard.docx`: Client-provided problem statement and requirements.

---

## Benefits of the Solution

1. **Time Saved**: Automation reduced the manual workload significantly.
2. **Cost Efficiency**: Eliminated the need for additional hires.
3. **Error-Free Reports**: Improved accuracy in financial insights.
4. **Enhanced Decision-Making**: The client can now make informed, data-driven decisions.

---

Feel free to contribute, raise issues, or share feedback on this repository. Your input is valuable in enhancing this project!

--- 

This README highlights all key aspects of your project, integrating automation, data analysis, and dashboarding as per your client's requirements.
