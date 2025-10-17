# Telco Customer Churn Analysis

## Table of Content

- [Project Overview](#project-overview)
- [Data Sources](#data-sources)
- [Tools](#tools)
- [Data Cleaning](#data-cleaning)
- [Exploratory Data Analysis](#exploratory-data-analysis)
- [Data Analysis](#data-analysis)
- [Findings and Recommendations](#findings-and-recommendations)
- [Limitations](#limitations)

### Project Overview
---

The Telco Customer Churn Analysis project aims to understand the factors contributing to customer attrition (churn) within a telecommunications company. Using historical customer data, the project identifies key trends, behaviors, and patterns that influence whether a customer stays with or leaves the company.
The dataset contains customer demographics, services subscribed to, and billing information. Through data analysis and visualization, the goal is to help the company reduce churn rates and improve customer retention strategies.

[Dashboard](Telco_Customer_Churn.png)

<img width="533" height="285" alt="Telco_Customer_Churn" src="https://github.com/user-attachments/assets/51913398-47ed-4607-aafc-8ece7d781918" />

### Data Sources
---

Telco_customer_churn: The primary dataset used for this analysis is the "Telco_customer_churn.csv" file, containing detailed information about a telecommunications company’s customers, including:
- Customer demographics (gender, senior citizen, partner, dependents)
- Account information (tenure, contract type, payment method, monthly charges, total charges)
- Services subscribed (phone, internet, online security, streaming, tech support, etc.)
- Churn status (whether the customer left the company or stayed)

### Tools
---
- SQL: For data extraction, transformation, quering and key metric calculations.
- Power BI: For interactive dashboard creation.

### Data Cleaning
---

Before performing analysis, the Telco Customer Churn dataset was carefully cleaned and prepared to ensure data accuracy and consistency. The following steps were carried out:
1. Data loading and inspection.
2. Handling missing values.
3. Data cleaning and formatting.

### Exploratory Data Analysis
---

EDA Involved exploring the Telco data to answer key questions, such as:
- Total number of customers
- Whats the total number of churned customer, non churned customer and % churned rate
- Whats the average tenure by churn status.
- Whats the maximum and minimum tenure by churn status.
- Whats the churn by gender.
- Whats the churn rate by contract type.
- Whats the churn rate by payment method.
- Whats the churn by senior citizen status.
- Whats the churn by internet service type.
- Whats the effect of paperless billing on churn.

### Data Analysis
---
Includes some interesting codes/features worked with

``` sql
select 
       count(*) as "Total Customers" 
	     from telco_customer_churn;
```

``` sql
select 
    count(*) as "Total Customers",
    sum(case when Churn = 'Yes' then 1 else 0 end) as "Churned Customers",
    sum(case when Churn = 'No' then 1 else 0 end) as "Non-Churned Customers",
    ROUND((sum(case when Churn = 'Yes' then 1 else 0 end) * 100.0 / count(*)), 2) as "Churn Rate Percent"
    from telco_customer_churn;
```

``` sql
select Churn,
       round (AVG(tenure),2) as"average tenure"
       from telco_customer_churn
       group by Churn;
```

``` sql
select churn,
       min (tenure) as "minimum tenure",
	     max (tenure) as "maximum tenure"
	     from telco_customer_churn
	     group by churn;
```

``` sql
select  gender,
        count(*) as TotalCustomers,
        sum(case when Churn = 'Yes' then 1 else 0 end) as ChurnedCustomers,
        round((sum(case when Churn = 'Yes' then 1 else 0 end) * 100.0 / count(*)), 2) as ChurnRatePercent
        from telco_customer_churn
        group by gender
        order by ChurnRatePercent desc;
```

``` sql
select Contract,
       count(*)as TotalCustomers,
       sum(case when Churn = 'Yes' then 1 else 0 end) as ChurnedCustomers,
       round( (sum(case when Churn = 'Yes' then 1 else 0 end) * 100.0 / COUNT(*)), 2)as ChurnRatePercent
       from telco_customer_churn
       group by Contract
       order by ChurnRatePercent desc;
```

``` sql
select payment_method,
       count(*) as TotalCustomers,
       sum(case when Churn = 'Yes' then 1 else 0 end) as ChurnedCustomers,
       round( (sum(case when Churn = 'Yes' then 1 else 0 end) * 100.0 / count(*)), 2) as ChurnRatePercent
       from telco_customer_churn
       group by Payment_Method
       order by ChurnRatePercent desc;
```

``` sql
select case
       when senior_citizen = 1 then 'Senior Citizen'
       else 'Non-Senior'
       end as CustomerType,
       count(*) as TotalCustomers,
       sum(case when Churn = 'Yes' then 1 else 0 end) as ChurnedCustomers,
       round( (sum(case when Churn = 'Yes' then 1 else 0 end) * 100.0 / count(*)), 2) as ChurnRatePercent
       from telco_customer_churn
       group by
       case
       when Senior_Citizen = 1 then 'Senior Citizen'
       else 'Non-Senior'
       end
       order by ChurnRatePercent desc;
```

``` sql
select internet_service,
       count(*) as TotalCustomers,
       sum(case when Churn = 'Yes' then 1 else 0 end) as ChurnedCustomers,
       round( (sum(case when Churn = 'Yes' then 1 else 0 end) * 100.0 / count(*)), 2) as ChurnRatePercent
       from telco_customer_churn
       group by Internet_Service
       order by ChurnRatePercent desc;
```

``` sql
select
	    paperless_billing,
        Churn,
        count (*) as CustomerCount,
        round(100.0 * count(*) / sum(count(*)) over (partition by paperless_billing), 2) as Percentage
        from Telco_Customer_Churn
        group by paperless_billing, Churn
        order by paperless_billing, Churn;
```

### Findings and Recommendations
---

1. Customers with shorter tenure (new customers) are more likely to churn compared to long-term loyal customers.
- Recommendation: Focus on early engagement, and welcome offers during the first 6 months.
2. Customers with higher monthly charges show a higher tendency to churn compared to those on lower-cost plans.
- Recommendation: Consider loyalty discounts, or value-added bundles for premium customers to justify cost.
3. Month-to-month contracts have the highest churn rate, while 2-year contracts have the lowest churn.
- Recommendation: Encourage 1–2-year contracts with discounts or perks to lock in retention.
4. Customers paying with electronic check churn the most, compared to those using credit card or bank transfer.
- Recommendation: Motivate customers to switch to automatic payments (credit card / bank transfer).
5. Senior citizens show a higher churn rate compared to younger customers.
- Recommendation: Offer simplified plans, better support, and senior-friendly packages to reduce churn.
6. Customers using Fiber Optic internet churn more than those on DSL or without internet service.
- Recommendation: Investigate service quality issues or pricing competitiveness in fiber plans.
7. Customers with paperless billing churn more than those receiving mailed bills.
- Recommendation: Ensure digital billing experience is clear and easy to use.
8. Average Monthly Charges is around $65–70, but churned customers tend to have higher monthly bills.
- Recommendation: Run price sensitivity analysis and introduce loyalty discounts for higher spenders.

### Limitations
---

Possible Data Entry or Recording Errors:
- There were blank values in the `TotalCharges` column.




