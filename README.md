
Customer Churn Analysis 
1. Introduction to Churn Analysis
Objective:
In today’s competitive business environment, retaining customers is crucial for long-term success. Churn analysis helps identify why customers leave and predict which ones are at risk of leaving. Businesses can utilize this information to develop strategies that improve customer satisfaction and loyalty, ultimately reducing churn.

Business Value:
By analyzing customer data, businesses can proactively take steps to improve customer retention. Understanding the factors that lead to churn allows companies to address these issues and reduce customer turnover. Predictive models can help businesses target customers who are at risk of leaving, providing them with an opportunity to retain them through tailored interventions.

Tools Used:

SQL Server: For managing, cleaning, and preparing customer data.
Power BI: For visualizing insights derived from customer data.

2. Data & Resources Used
Data Source:
Customer data from the telecom industry was used for this project. The data included various demographic, geographic, account, and service-related variables that provided a comprehensive view of the customer base. The analysis and methodologies used in this project, however, are applicable to any industry where customer retention is important.

Other Resources:

SQL Server: For data transformation and analysis.
Power BI: For creating dashboards and visualizing data insights.
Target Audience:
This analysis benefits businesses from any industry seeking to reduce customer churn and improve customer retention. The techniques and insights provided in this portfolio are applicable across various sectors such as retail, finance, telecom, healthcare, and more.

3. Project Target
Goals:
Build an ETL (Extract, Transform, Load) process in SQL Server to manage and process customer data.
Create a Power BI dashboard to visualize demographic, geographic, and customer account data.
Identify patterns in customer churn and suggest actionable insights.
Predict customer churn using machine learning techniques.
Metrics to Track:

Total Customers
Total Churn & Churn Rate
New Joiners

4. ETL Process in SQL Server
Data Loading:
The first step was to load the customer data into SQL Server using a CSV file. This data was imported into a staging table, Customer_Data, using the Import Wizard in SQL Server Management Studio (SSMS).

Data Exploration:
Before analyzing the data, it’s essential to understand its structure, characteristics, and potential issues. The following SQL queries were used to explore distinct values and check the data for consistency:

1. Check Distribution by Gender:

SELECT Gender, COUNT(Gender) AS TotalCount,
COUNT(Gender)*100.0 / (SELECT COUNT(*) FROM Customer_Data) AS Percentage
FROM Customer_Data
GROUP BY Gender;
2. Check Distribution by Contract:

sql
Copy code
SELECT Contract, COUNT(Contract) AS TotalCount,
COUNT(Contract)*100.0 / (SELECT COUNT(*) FROM Customer_Data) AS Percentage
FROM Customer_Data
GROUP BY Contract;

3. Check Customer Status & Revenue Distribution:

SELECT Customer_Status, COUNT(Customer_Status) AS TotalCount, SUM(Total_Revenue) AS TotalRev,
SUM(Total_Revenue) / (SELECT SUM(Total_Revenue) FROM Customer_Data) * 100 AS RevPercentage
FROM Customer_Data
GROUP BY Customer_Status;

4. Check Distribution by State:

SELECT State, COUNT(State) AS TotalCount,
COUNT(State)*100.0 / (SELECT COUNT(*) FROM Customer_Data) AS Percentage
FROM Customer_Data
GROUP BY State
ORDER BY Percentage DESC;

5. Identify Unique Internet Types:

SELECT DISTINCT Internet_Type
FROM Customer_Data;

Null Value Exploration:
Identifying and handling missing or null values is crucial for maintaining data integrity. The following query helps in checking for null values across different columns:

SELECT 
    SUM(CASE WHEN Customer_ID IS NULL THEN 1 ELSE 0 END) AS Customer_ID_Null_Count,
    SUM(CASE WHEN Gender IS NULL THEN 1 ELSE 0 END) AS Gender_Null_Count,
    SUM(CASE WHEN Age IS NULL THEN 1 ELSE 0 END) AS Age_Null_Count,
    SUM(CASE WHEN Married IS NULL THEN 1 ELSE 0 END) AS Married_Null_Count,
    -- Other columns here...
FROM Customer_Data;


Data Cleaning & Transformation:
To prepare the data for analysis, null values were replaced, and data was cleaned. The following SQL query was used to insert clean data into the production table:

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
View Creation:
Two views were created for further analysis in Power BI:

View for Churned & Stayed Customers:

CREATE VIEW vw_ChurnData AS
SELECT * FROM prod_Churn WHERE Customer_Status IN ('Churned', 'Stayed');
View for New Joiners:

CREATE VIEW vw_JoinData AS
SELECT * FROM prod_Churn WHERE Customer_Status = 'Joined';
5. Power BI Transformation
Once the data was processed in SQL Server, the next step was to load it into Power BI for transformation and visualization.

Additional Data Transformations:
Churn Status:

A new column was created to represent churned customers with a 1 and those who stayed with a 0.
Monthly Charge Range:

A column was created to classify customers based on their monthly charge into ranges: <20, 20-50, 50-100, and >100.
Age Group Classification:

The Age column was transformed into categories: <20, 20-35, 36-50, >50.
Tenure Grouping:

Customers were grouped by tenure in months: <6, 6-12, 12-18, 18-24, and >=24 months.
Unpivot Service Columns:

Service columns were unpivoted to simplify analysis of service usage across different customer segments.
6. Power BI Measures
Power BI measures were created to track key metrics:

Total Customers:
Total Customers = COUNT(prod_Churn[Customer_ID]);

New Joiners:
New Joiners = CALCULATE(COUNT(prod_Churn[Customer_ID]), prod_Churn[Customer_Status] = "Joined");

Total Churn:
Total Churn = SUM(prod_Churn[Churn Status]);

Churn Rate:
Churn Rate = [Total Churn] / [Total Customers];

7. Power BI Visualization
A comprehensive Power BI dashboard was created to provide a detailed view of customer churn.

Summary Page:
Top Cards:
Total Customers, New Joiners, Total Churn, Churn Rate.
Demographics:
Gender & Age Group vs Churn Rate.
Account Information:
Payment Method, Contract Type, Tenure Group vs Churn Rate.
Geographic Analysis:
Churn Rate by top 5 states.
Churn Distribution:
Churn Category and Churn Reason (as tooltip).
Service Usage:
Internet Type and other services vs Churn Rate.
Churn Reason Page:
Breakdown of churn reasons and churn counts.# Portfolio
