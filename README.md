# Customer Churn Analysis

## 1. Introduction to Churn Analysis

**Objective**:  
In today’s competitive business environment, retaining customers is crucial for long-term success. Churn analysis helps identify why customers leave and predict which ones are at risk of leaving. Businesses can utilize this information to develop strategies that improve customer satisfaction and loyalty, ultimately reducing churn.

**Business Value**:  
By analyzing customer data, businesses can proactively take steps to improve customer retention. Understanding the factors that lead to churn allows companies to address these issues and reduce customer turnover. Predictive models can help businesses target customers who are at risk of leaving, providing them with an opportunity to retain them through tailored interventions.

**Tools Used**:
- **SQL Server**: For managing, cleaning, and preparing customer data.
- **Power BI**: For visualizing insights derived from customer data.

---

## 2. Data & Resources Used

**Data Source**:  
Customer data from the telecom industry was used for this project. The data included various demographic, geographic, account, and service-related variables that provided a comprehensive view of the customer base. The analysis and methodologies used in this project, however, are applicable to any industry where customer retention is important.

**Other Resources**:
- **SQL Server**: For data transformation and analysis.
- **Power BI**: For creating dashboards and visualizing data insights.

**Target Audience**:  
This analysis benefits businesses from any industry seeking to reduce customer churn and improve customer retention. The techniques and insights provided in this portfolio are applicable across various sectors such as retail, finance, telecom, healthcare, and more.

---

## 3. Project Target

### Goals:
- Build an ETL (Extract, Transform, Load) process in SQL Server to manage and process customer data.
- Create a Power BI dashboard to visualize demographic, geographic, and customer account data.
- Identify patterns in customer churn and suggest actionable insights.
- Predict customer churn using machine learning techniques.

### Metrics to Track:
- Total Customers
- Total Churn & Churn Rate
- New Joiners

---

## 4. ETL Process in SQL Server

### Data Loading:
The first step was to load the customer data into SQL Server using a CSV file. This data was imported into a staging table, `Customer_Data`, using the Import Wizard in SQL Server Management Studio (SSMS).

### Data Exploration:
Before analyzing the data, it’s essential to understand its structure, characteristics, and potential issues. The following SQL queries were used to explore distinct values and check the data for consistency:

### 1. Check Distribution by Gender:
```sql
SELECT Gender, COUNT(Gender) AS TotalCount,
COUNT(Gender)*100.0 / (SELECT COUNT(*) FROM Customer_Data) AS Percentage
FROM Customer_Data
GROUP BY Gender;
```
### 2. Check Distribution by Contract
```sql
SELECT Contract, COUNT(Contract) AS TotalCount,
COUNT(Contract)*100.0 / (SELECT COUNT(*) FROM Customer_Data) AS Percentage
FROM Customer_Data
GROUP BY Contract;
```

### 3. Check Customer Status & Revenue Distribution:
```sql
SELECT Customer_Status, COUNT(Customer_Status) AS TotalCount, SUM(Total_Revenue) AS TotalRev,
SUM(Total_Revenue) / (SELECT SUM(Total_Revenue) FROM Customer_Data) * 100 AS RevPercentage
FROM Customer_Data
GROUP BY Customer_Status;
```
### 4. Check Distribution by State:
```sql
SELECT State, COUNT(State) AS TotalCount,
COUNT(State)*100.0 / (SELECT COUNT(*) FROM Customer_Data) AS Percentage
FROM Customer_Data
GROUP BY State
ORDER BY Percentage DESC;
```
### 5. Identify Unique Internet Types:
```sql
SELECT DISTINCT Internet_Type
FROM Customer_Data;
```
### 6. Null Value Exploration 
```sql
SELECT 
    SUM(CASE WHEN Customer_ID IS NULL THEN 1 ELSE 0 END) AS Customer_ID_Null_Count,
    SUM(CASE WHEN Gender IS NULL THEN 1 ELSE 0 END) AS Gender_Null_Count,
    SUM(CASE WHEN Age IS NULL THEN 1 ELSE 0 END) AS Age_Null_Count,
    SUM(CASE WHEN Married IS NULL THEN 1 ELSE 0 END) AS Married_Null_Count,
    SUM(CASE WHEN State IS NULL THEN 1 ELSE 0 END) AS State_Null_Count,
    SUM(CASE WHEN Number_of_Referrals IS NULL THEN 1 ELSE 0 END) AS Number_of_Referrals_Null_Count,
    SUM(CASE WHEN Tenure_in_Months IS NULL THEN 1 ELSE 0 END) AS Tenure_in_Months_Null_Count,
    SUM(CASE WHEN Value_Deal IS NULL THEN 1 ELSE 0 END) AS Value_Deal_Null_Count,
    SUM(CASE WHEN Phone_Service IS NULL THEN 1 ELSE 0 END) AS Phone_Service_Null_Count,
    SUM(CASE WHEN Multiple_Lines IS NULL THEN 1 ELSE 0 END) AS Multiple_Lines_Null_Count,
    SUM(CASE WHEN Internet_Service IS NULL THEN 1 ELSE 0 END) AS Internet_Service_Null_Count,
    SUM(CASE WHEN Internet_Type IS NULL THEN 1 ELSE 0 END) AS Internet_Type_Null_Count,
    SUM(CASE WHEN Online_Security IS NULL THEN 1 ELSE 0 END) AS Online_Security_Null_Count,
    SUM(CASE WHEN Online_Backup IS NULL THEN 1 ELSE 0 END) AS Online_Backup_Null_Count,
    SUM(CASE WHEN Device_Protection_Plan IS NULL THEN 1 ELSE 0 END) AS Device_Protection_Plan_Null_Count,
    SUM(CASE WHEN Premium_Support IS NULL THEN 1 ELSE 0 END) AS Premium_Support_Null_Count,
    SUM(CASE WHEN Streaming_TV IS NULL THEN 1 ELSE 0 END) AS Streaming_TV_Null_Count,
    SUM(CASE WHEN Streaming_Movies IS NULL THEN 1 ELSE 0 END) AS Streaming_Movies_Null_Count,
    SUM(CASE WHEN Streaming_Music IS NULL THEN 1 ELSE 0 END) AS Streaming_Music_Null_Count,
    SUM(CASE WHEN Unlimited_Data IS NULL THEN 1 ELSE 0 END) AS Unlimited_Data_Null_Count,
    SUM(CASE WHEN Contract IS NULL THEN 1 ELSE 0 END) AS Contract_Null_Count,
    SUM(CASE WHEN Paperless_Billing IS NULL THEN 1 ELSE 0 END) AS Paperless_Billing_Null_Count,
    SUM(CASE WHEN Payment_Method IS NULL THEN 1 ELSE 0 END) AS Payment_Method_Null_Count,
    SUM(CASE WHEN Monthly_Charge IS NULL THEN 1 ELSE 0 END) AS Monthly_Charge_Null_Count,
    SUM(CASE WHEN Total_Charges IS NULL THEN 1 ELSE 0 END) AS Total_Charges_Null_Count,
    SUM(CASE WHEN Total_Refunds IS NULL THEN 1 ELSE 0 END) AS Total_Refunds_Null_Count,
    SUM(CASE WHEN Total_Extra_Data_Charges IS NULL THEN 1 ELSE 0 END) AS Total_Extra_Data_Charges_Null_Count,
    SUM(CASE WHEN Total_Long_Distance_Charges IS NULL THEN 1 ELSE 0 END) AS Total_Long_Distance_Charges_Null_Count,
    SUM(CASE WHEN Total_Revenue IS NULL THEN 1 ELSE 0 END) AS Total_Revenue_Null_Count,
    SUM(CASE WHEN Customer_Status IS NULL THEN 1 ELSE 0 END) AS Customer_Status_Null_Count,
    SUM(CASE WHEN Churn_Category IS NULL THEN 1 ELSE 0 END) AS Churn_Category_Null_Count,
    SUM(CASE WHEN Churn_Reason IS NULL THEN 1 ELSE 0 END) AS Churn_Reason_Null_Count
FROM Customer_Data;
```
### 7. Data Cleaning & Transformation:
```sql
SELECT 
    Customer_ID,
    Gender,
    Age,
    Married,
    State,
    Number_of_Referrals,
    Tenure_in_Months,
    ISNULL(Value_Deal, 'None') AS Value_Deal,
    Phone_Service,
    ISNULL(Multiple_Lines, 'No') As Multiple_Lines,
    Internet_Service,
    ISNULL(Internet_Type, 'None') AS Internet_Type,
    ISNULL(Online_Security, 'No') AS Online_Security,
    ISNULL(Online_Backup, 'No') AS Online_Backup,
    ISNULL(Device_Protection_Plan, 'No') AS Device_Protection_Plan,
    ISNULL(Premium_Support, 'No') AS Premium_Support,
    ISNULL(Streaming_TV, 'No') AS Streaming_TV,
    ISNULL(Streaming_Movies, 'No') AS Streaming_Movies,
    ISNULL(Streaming_Music, 'No') AS Streaming_Music,
    ISNULL(Unlimited_Data, 'No') AS Unlimited_Data,
    Contract,
    Paperless_Billing,
    Payment_Method,
    Monthly_Charge,
    Total_Charges,
    Total_Refunds,
    Total_Extra_Data_Charges,
    Total_Long_Distance_Charges,
    Total_Revenue,
    Customer_Status,
    ISNULL(Churn_Category, 'Others') AS Churn_Category,
    ISNULL(Churn_Reason , 'Others') AS Churn_Reason
INTO [db.Churn].[dbo].[prod_Churn]
FROM [db.Churn].[dbo].[Customer_Data];
```
### View Creation for Power BI:
1. View for Churned & Stayed Customers:
```sql
CREATE VIEW vw_ChurnData AS
SELECT * 
FROM prod_Churn 
WHERE Customer_Status IN ('Churned', 'Stayed');
```
2. View for New Joiners:
```sql
CREATE VIEW vw_JoinData AS
SELECT * 
FROM prod_Churn 
WHERE Customer_Status = 'Joined';
```
## 7. Power BI Visualization

A comprehensive Power BI dashboard was created to provide a detailed view of customer churn. The dashboard is designed to give stakeholders an easy-to-understand visualization of key metrics, allowing them to quickly grasp insights and take action based on customer behavior and churn trends.

### Summary Page:

The **Summary Page** contains the following key visual elements:

#### **Top Cards**:
- **Total Customers**: The total number of active customers.
- **New Joiners**: The number of new customers who recently joined.
- **Total Churn**: The total number of customers who have churned.
- **Churn Rate**: The percentage of customers who have churned compared to the total customer base.

#### **Demographics**:
- **Gender**: Displays churn rate broken down by gender, allowing for an analysis of whether churn differs between male and female customers.
- **Age Group**: Visualizes the total number of customers and churn rate across different age groups.

#### **Account Information**:
- **Payment Method**: Visualizes the churn rate by payment method (e.g., credit card, debit card, etc.).
- **Contract Type**: Shows churn rates across different contract types (e.g., monthly, yearly contracts).
- **Tenure Group**: Displays the total number of customers and churn rate segmented by how long customers have been with the company (e.g., 6 months, 12 months, 24 months, etc.).

#### **Geographic Analysis**:
- **Top 5 States by Churn Rate**: A geographical representation of the churn rate, highlighting the top 5 states with the highest churn.

#### **Churn Distribution**:
- **Churn Category**: Visualizes the total churn broken down by churn categories (e.g., voluntary churn, involuntary churn).
- **Churn Reason (Tooltip)**: Displays the reasons for churn as a tooltip on hover, providing more insight into why customers are leaving.

### Service Usage:

The **Service Usage** section includes:

- **Internet Type**: Visualizes the churn rate segmented by the type of internet service used (e.g., broadband, DSL, fiber).
- **Other Services**: Displays how usage of different services (such as streaming TV, multiple lines, or premium support) correlates with churn.

### Churn Reason Page:

The **Churn Reason Page** provides a detailed breakdown of the reasons behind customer churn:

- **Churn Reasons**: A bar chart showing the various reasons customers have given for churning (e.g., high prices, poor customer service, competitor offering, etc.).
- **Churn Counts**: The total count of customers who have cited each reason for leaving, offering insight into the most common pain points that lead to churn.

