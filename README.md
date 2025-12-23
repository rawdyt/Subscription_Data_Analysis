# Subscription Data - Dashboard

### Dashboard Link : https://app.powerbi.com/groups/me/reports/af9b6485-bbb8-4ca4-889c-f915607a0750/58c910ddfd046d8a8d9f?experience=power-bi

### Project Overview

This project focuses on analyzing SaaS (Software as a Service) subscription data to understand customer behavior, churn patterns, revenue trends, and support usage. The goal is to convert raw operational data into clear business insights using SQL, ETL concepts, and Power BI dashboards.

### Buisness Objectives

The project aims to answer key business questions such as:

* How many customers are active vs churned?

* When do customers churn and how long do they stay?

* Which customers require high support?

* How does churn impact revenue?

These insights help stakeholders make decisions related to customer retention, support optimization, and revenue growth.

### Steps:

1) Loded the data to SQL

2) Handled missing and invalid values

3) Created Column:
    Support Level (high & low)

    Customer lifetime
4) Removing Duplicates

5) Customers with 3 or more tickets classified as High support

6) All the customer with Churned date marked as Churned.

### SQL queries used 

1) To update the Characters to identical and uppercase formate

        UPDATE SUBSCRIPTION 
        SET plan_type = upper(LTRIM(rtrim(plan_type))),
        payment_mode = upper(LTRIM(rtrim(payment_mode))),
	    account_status = upper(LTRIM(rtrim(account_status)));

2) To Handle the null values in the Discount Column

         update subscription 
         set discount_pct = 0
         where discount_pct is null;

3) Changin the column type 

        alter table subscription 
        add net_revenue float;

4) Calculating the Net revenue

         update subscription
         set net_revenue = monthly_fee -    
         (monthly_fee*discount_pct/100);

5) Calculating the life time

       SELECT customer_id, DATEDIFF ( DAY, signup_date , ISNULL  
       (churn_date, getdate())) as lifetime_days
       FROM subscription;

6) Calculating the revenue as per Plan type and ranking as per the revenue

       select plan_type , 
       round(SUM(net_revenue),2) AS revenue, 
       RANK() OVER (ORDER BY SUM(net_revenue) DESC) AS 
       revenue_rank
	 FROM subscription
	 Group by plan_type;

7) To add the support level colun first created the column with name support_level and then run the update statement.

         Alter table subscription 
         add  Support_Level Varchar(30);

       update subscription
       set Support_level=
       (case
       when support_tickets>= 3 then 'High_Support'
	 else 'Low_support'
	 End 
	 ) ;

### Key KPIs

Total Customers

Active Customers

Churned Customers

Churn Rate (%)

Average Customer Lifetime

Average Revenue Per User (ARPU)


### Dashboard Image

![Dashboard Page 1](https://raw.githubusercontent.com/rawdyt/Subscription_Data_Analysis/main/Screenshot%202025-12-23%20092517.png)

![Dashboard Page 2](https://raw.githubusercontent.com/rawdyt/Subscription_Data_Analysis/main/Screenshot%202025-12-23%20092552.png)



### Key Insights from the Analysis

Customers with high support ticket volume show a higher churn tendency

Certain subscription plans have better retention rates

Longer customer lifetime directly contributes to higher revenue stability

Early churn often occurs within the first few months of subscription

These insights can help businesses:

Improve onboarding

Reduce churn

Optimize support operations





