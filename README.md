# Data Analysis Project:Sales Insights of AtliQ Hardware
## Table Of Contents:
* [Problem Statement](https://github.com/Ramya-Mahi/Sales-Insights-Data-Analysis/edit/main/README.md#problem-statement)<br>
* [Data Discovery](https://github.com/Ramya-Mahi/Sales-Insights-Data-Analysis/edit/main/README.md#data-discovery)<br>
* [Data Analysis using MySQL](https://github.com/Ramya-Mahi/Sales-Insights-Data-Analysis/edit/main/README.md#data-analysis-using-mysql)<br>
* [Data Cleaning and ETL](https://github.com/Ramya-Mahi/Sales-Insights-Data-Analysis/edit/main/README.md#data-cleaning-and-etl-extract-transform-load)<br>
* [Data Modeling](https://github.com/Ramya-Mahi/Sales-Insights-Data-Analysis/edit/main/README.md#data-modeling)<br>
* [Dashboard](https://github.com/Ramya-Mahi/Sales-Insights-Data-Analysis/edit/main/README.md#dashboard)<br>
* [Tools, Software and Libraries](https://github.com/Ramya-Mahi/Sales-Insights-Data-Analysis/edit/main/README.md#tools-software-and-libraries)<br>
## Problem Statement
AtliQ Hardware is a computer hardware company that is navigating the ever-evolving market landscape. In this case study, the Sales Director has decided to invest in a data analysis project in order to construct a Power BI dashboard that would provide him with real-time sales insights.
## Data Discovery
* ### Project Planning using AIMS Grid -
  It is a project management tool which consists of four components:<br>
  
  * Purpose - (What to do exactly)<br>
  * Stakeholder - (Who will be involved)<br>
  * End result - (What do you want to achieve)<br>
  * Success Criteria - (Cost optimization and time save)<br>
* ### AIMS Grid
  **1. Purpose** :- To unlock sales insights that are not visible before for the sales them for decision support and automate them to reduced manual time spent in data gathering.

  **2. Stakeholders** :-

  * Sales Director
  * Marketing Team
  * Customer Service Team
  * Data and Analytics Team
  * IT
  
  **3. End result** :- An automated dashboard providing quick and latest sights in order to support Data driven decision making.

  **4. Success Criteria** :-

  * Dashboard uncovering sales order insights with latest data available
  * Sales Team able to take better data-driven decisions.
  * Sales Analysts stop data gathering manually in order to save business time.

## Data Analysis using MySQL
1.Show all customers records<br>

`
  SELECT * FROM customers;
`

2.Show total number of customers<br>

`
  SELECT count(*) FROM customers;
`

3.Show all transactions records<br>

`
  SELECT * FROM transactions;
`

4.Show total number of transactions<br>

`
  SELECT count(*) FROM transactions;
`

5.Show transactions where currency is US dollars<br>

`
  SELECT * from transactions where currency="USD"
`

6.Show transactions for Chennai market (market code for Chennai is Mark001)<br>

`
  SELECT * FROM transactions where market_code='Mark001';
`

7.Show transactions in 2020 join by date table<br>

`
  SELECT transactions.*, date.* FROM transactions INNER JOIN date ON transactions.order_date=date.date where date.year=2020;
`

8.Show total revenue in year 2020<br>

`
  SELECT SUM(transactions.sales_amount) FROM transactions INNER JOIN date ON transactions.order_date=date.date where date.year=2020 and transactions.currency="INR" or transactions.currency="USD";
`

9.Show total revenue in year 2020, January Month<br>

`
  SELECT SUM(transactions.sales_amount) FROM transactions INNER JOIN date ON transactions.order_date=date.date where date.year=2020 and and date.month_name="January" and (transactions.currency="INR" or transactions.currency="USD");
`

10.Show total revenue in year 2020 in Chennai<br>

`
  SELECT SUM(transactions.sales_amount) FROM transactions INNER JOIN date ON transactions.order_date=date.date where date.year=2020 and transactions.market_code="Mark001";
`

## Data Cleaning and ETL (Extract, Transform, Load)
Our data is initially extracted from the SQL database into Power BI, and then the subsequent Extract, Transform, and Load (ETL) is executed in Power Query:<br>

1.Removing records of sales amount in transactions table where sales amount is 0 and -1<br>

`
  = Table.SelectRows(sales_transactions, each ([sales_amount] <> -1 and [sales_amount] <> 0))
`

2.Currency Cleaning<br>

`
  = Table.SelectRows(#"remove -1 and 0", each ([currency] = "INR" or [currency] = "USD"))
`

3.Adding normalized sales amount column<br>

`
  = Table.AddColumn(#"clean up currency", "norm_sales_amount", each if [currency]="USD" then [sales_amount]*75 else [sales_amount])
`

## Data Modelling
The Data Model illustrates the correlation between various tables. The following is the Data Model of sales database:<br>

![Screenshot 2023-10-12 122803](https://github.com/Ramya-Mahi/Sales-Insights-Data-Analysis/assets/110104347/f93ed141-9721-42f0-beea-310678a812d2)

## Data Analysis using DAX

Some of the measures done using DAX are:<br>

1. Revenue=` SUM('sales transactions'[norm_sales_amount])`<br>
2. Sales Qty=`SUM('sales transactions'[sales_qty])`<br>
3. Total Profit Margin=`SUM('sales transactions'[profit_margin])`<br>
4. Profit Margin %=`DIVIDE([Total profit margin],[Revenue],0)`<br>
5. Profit Margin Contribution %=`DIVIDE([Total profit margin],CALCULATE([Total profit margin],ALL('sales products'),ALL('sales customers'),ALL('sales markets')))`<br>
6. Net Profit=`SUM('sales transactions'[profit_margin])`<br>
7. Revenue Contribution % =`DIVIDE([Revenue],CALCULATE([Revenue],ALL('sales products'),ALL('sales customers'),ALL('sales markets')))`<br>
8. Revenue LY =`CALCULATE([Revenue],SAMEPERIODLASTYEAR('sales date'[date]))`<br>

Profit Target Measures:<br>

1. Profit Target Value =`SELECTEDVALUE('Profit Target'[Profit Target])`<br>
2. Target Diff =`[Profit margin %]-'Profit Target'[Profit Target Value]`<br>

## Dashboard 

 |  Key Insights  |
 | -------------- |
 |![Screenshot 2023-10-12 123505](https://github.com/Ramya-Mahi/Sales-Insights-Data-Analysis/assets/110104347/f7da21fd-617d-4e85-82b6-d25e41af2514)|

 |  Profit Analysis  |
 |-------------------|
 | ![Screenshot 2023-10-12 123641](https://github.com/Ramya-Mahi/Sales-Insights-Data-Analysis/assets/110104347/9f155adc-ba65-4978-9404-66dae8d2f44b)|

 |  Performance Insights |
 |---------------------- |
 |![Screenshot 2023-10-12 123744](https://github.com/Ramya-Mahi/Sales-Insights-Data-Analysis/assets/110104347/f67a1c31-583a-4cc7-8f04-d26b30f17a02)|
 
## Tools, Software and Libraries

 1. MySQL
 2. Microsoft Power BI
 
