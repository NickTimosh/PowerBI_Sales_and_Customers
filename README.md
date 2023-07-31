# Power BI Sales & Cohort Analysis Dashboard 📊🚀

Welcome to the GitHub repository for my exciting Power BI project - a Sales and Cohort Analysis Dashboard!

![Power BI Dashboard1](https://github.com/NickTimosh/PowerBI_Sales_and_Customers/assets/116592259/44346a14-20fb-405a-9005-7e38e49a9db6)

![Power BI Dashboard2](https://github.com/NickTimosh/PowerBI_Sales_and_Customers/assets/116592259/e40e8271-a3d0-47c8-b52d-751bb28bd29f)

![Power BI Dashboard3](https://github.com/NickTimosh/PowerBI_Sales_and_Customers/assets/116592259/f779e391-78b6-4d47-b3bb-3f9fbcf4e125)

![Power BI Dashboard4](https://github.com/NickTimosh/PowerBI_Sales_and_Customers/assets/116592259/e78003b0-064e-4ea1-8003-1496d96ab2b2)

## Data Source Description 📂

This Online Retail II data set contains all the transactions occurring for a UK-based and registered, non-store online retail between 01/12/2009 and 09/12/2011.The company mainly sells unique all-occasion gift-ware. Many customers of the company are wholesalers. The data comes from a CSV file with approximately 1 million rows. 

Explore dataset: [UC Irvine Machine Learning Repository](https://archive.ics.uci.edu/dataset/502/online+retail+ii)

## Data Modeling Process 🛠️

I created dimension tables for customers, products, regions and time to facilitate efficient querying and visualization. By establishing relationships between these tables, we can easily navigate and analyze data across various dimensions.

![data_model](https://github.com/NickTimosh/PowerBI_Sales_and_Customers/assets/116592259/4cd068ca-7d4e-4419-abb9-4b022a2163c6)


## DAX Formulas 💼

DAX allowed me to calculate aggregated measures, perform cohort analysis, and generate trend analyses. Some examples:

<details>
<summary>KPI last_year_diff ▲</summary>

---

```
% Total_Sales_diff_prev_year = 

VAR _this_year = [Total_Sales]
VAR _prev_year = 
    CALCULATE(
        [Total_Sales],
        SAMEPERIODLASTYEAR(Dim_Calendar[Date])
    )
VAR _diff = _this_year - _prev_year
VAR _sign = IF(_diff>0,"▲", "▼")


RETURN

_sign & FORMAT(
    COALESCE(
        DIVIDE(_diff,_prev_year),
        0
        ),
        "#0.0%"
)
& " | "
& 
_sign & FORMAT(
    COALESCE(
        DIVIDE(_diff,1000),
        0
        ),
        "$#,##0.0K"
)
```

---
</details>


<details>
<summary>Customer Retention</summary>

---

-- Resurrected Customers
```
Resurrected_Customers = 
    
VAR _CustomersThisMonth = 
    VALUES(Fact_Retail[Customer ID])

VAR _CustomersLastMonth = 
    CALCULATETABLE(
        VALUES(Fact_Retail[Customer ID]),
        PREVIOUSMONTH((Dim_Calendar[Start of Month]))
    )

VAR _NewCustomers = 
    CALCULATETABLE(
        VALUES(Fact_Retail[Customer ID]),
        Fact_Retail[Months Since first Transaction] = 0
    )

VAR _ResurrectedCustomers = 
    EXCEPT(
        EXCEPT(
            _CustomersThisMonth,
            _CustomersLastMonth
        ), -- remove last month`s customers
        _NewCustomers
    ) -- remove new customers

RETURN
    COUNTROWS(_ResurrectedCustomers)
```

-- Cohort Performance

```
Cohort_Performance = 
    
    VAR _MinDate = MIN(Dim_Calendar[Start of Month])

    VAR _MaxDate = MAX(Dim_Calendar[Start of Month])

    RETURN
        CALCULATE(
            [Active_Customers],
            REMOVEFILTERS(Dim_Calendar[Start of Month]),
            RELATEDTABLE(Dim_Customers),
            Dim_Customers[First_Transaction_Month] >= _MinDate 
                && Dim_Customers[First_Transaction_Month] <= _MaxDate
        )
```


## Dashboard Screenshots 📈🔍

Here are some snapshots from the Power BI dashboard:

![Sales Overview](dashboard_sales_overview.png)

![Product-wise Analysis](dashboard_product_analysis.png)

![Customer Cohorts](dashboard_customer_cohorts.png)

![Revenue Trends](dashboard_revenue_trends.png)

Please note that these screenshots provide just a glimpse of the dashboard's capabilities. To experience the full interactivity and insights, I encourage you to clone the repository and explore the Power BI file yourself.

## Feedback and Collaboration 🙌

I'm eager to hear your thoughts and suggestions on the project. If you have any feedback regarding the data modeling, DAX formulas, or visualization choices, please don't hesitate to open an issue or reach out to me directly. I'm also open to collaboration and welcome any contributions that could enhance the dashboard's functionalities.

Thank you for your interest in this project, and I hope this dashboard inspires you to delve deeper into the world of data visualization and analytics!

Happy analyzing! 🚀📊

**Note:** To access the interactive dashboard and explore the full project, download the Power BI file from the [Releases](link-to-releases) section.

