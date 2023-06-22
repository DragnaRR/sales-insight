
# Sales Insight using MySQL & Tableau

A company is facing challenges in understanding their sales performance and identifying key trends and patterns in their customer behavior. So, they need to gain deeper insights into their sales data to drive growth and improve the overall sales strategy. 

The challenge is to gather and analyze large amounts of data from multiple sources and present it in a meaningful way that allows to make informed decisions about the sales activities.

The goal of the sales insights project is to develop a solution that can effectively gather, process, and analyze sales data from various sources, and provide actionable insights and recommendations to improve the company's sales performance.




## Requirement

- Mysql workbench
- Tableau desktop

## Data Source

Company is extracting data from an Online Transaction Processing (OLTP) system. They will use the transactional data stored in the OLTP system for reporting, analysis, and decision-making purposes. 

Data includes information about as customer, transaction, date, market information, zone and product.

![sql](https://github.com/DragnaRR/sales-insight/assets/95096810/7ec79aa2-cb7e-4b52-a9e9-2699037b84c8)

In general, data is extracted from OLTP system, transformed into a format which that can be easily analyzed, then data is loaded loaded to a target environment like a OLAP system / data warehouse which can be analyzed using tools like Tableau.

In this project, Data source is stored in a relational database from where data would be cleaned and analyzed using Tableau to gain insights into sales performance, customer behaviour and other key metrics.  
## Data Analysis in MySQL

MySQL is open-source relational database management system (RDBMS) used for storing and retrieving data. It can also be used for analysing with the help of some basic query.

using sales database/schema
```
use sales;
```
- All the transactions that occured
```
select * from transactions;
```
- Total number of transactions occured
```
select count(*) from transactions;
```
- All the transactions with sales quantity in range of 2-10
```
select * from transactions where sales_qty between 2 and 10;
```
- All the market names and codes for chennai
```
select * from markets where markets_name = 'Chennai';
```
- Total sales amount in year 2020
```
select sum(transactions.sales_amount)
from transactions join date 
on transactions.order_date = date.date
where date.year=2020;
```
- Total sales revenue in year 2020 in Chennai
```
select sum(transactions.sales_amount)
from transactions join markets
on transactions.market_code = markets.markets_code
join date on transactions.order_date = date.date
where date.year=2020 and markets.markets_name='Chennai';
```
- combined table containing information of transactions, market and date for year 2020 in Chennai
```
select transactions.*, markets.*, date.*
from transactions join markets
on transactions.market_code = markets.markets_code
join date on transactions.order_date = date.date
where date.year=2020 and markets.markets_name='Chennai';
```
- All the products for year 2020 in Chennai
```
select transactions.product_code
from transactions join markets
on transactions.market_code = markets.market_code
join date on transactions.order_date = date.date
where date.year=2020 and markets.markets_name='Chennai';
```
- Total sales revenue in year 2020, month January
```
select sum(transactions.sales_amount) 
from transactions join date
on transactions.order_date=date.date
where date.year=2020 and date.month_name='January';
```


## Data Cleaning in Tableau

During analysis on sql, its been observed that there are some outliers/errors that needed to be removed.

- sales_amount can't be negative or zero
- markets_code Mark097 and marks999 are wrong
- currency should be in INR

- **Importing data from Mysql**
![data source](https://github.com/DragnaRR/sales-insight/assets/95096810/06f55c2c-53fa-4dc6-b696-803fcbde668d)

- **Filtering sales_amount**
![filter](https://github.com/DragnaRR/sales-insight/assets/95096810/8befca95-5c30-4d85-a155-b7982a7694d1)

- **Filtering out markets_code**
![filter](https://github.com/DragnaRR/sales-insight/assets/95096810/f155fdb2-2cf4-4d7d-a2c4-70e79b442190)

- **Converting currency to INR**
![calculated field](https://github.com/DragnaRR/sales-insight/assets/95096810/52bf38f3-7ffa-48af-9eb4-256481b0b3e5)
creating a calculated field
```
IF [Currency] = 'USD' THEN [sales_amount]*83
ELSE [sales_amount]
END
```

## Data Anlysis in Tableau

- **Market Revenue**
![Market revenue](https://github.com/DragnaRR/sales-insight/assets/95096810/4e5a4b6f-5b43-4685-a946-a566ebc3dd17)

- **Market sales quantity**
![market sales quantity](https://github.com/DragnaRR/sales-insight/assets/95096810/67398396-a558-4b9a-af35-105bca3f3598)

- **Top 5 Customers**
![top 5 customer](https://github.com/DragnaRR/sales-insight/assets/95096810/6c1623d4-4f8f-4351-8580-169c3e2f0acd)

- **Top 5 Products**
![top 5 products](https://github.com/DragnaRR/sales-insight/assets/95096810/4f39340c-15a1-43e8-a112-a417dc3d6284)

- **Revenue by year**
![revenue by year](https://github.com/DragnaRR/sales-insight/assets/95096810/c214e1dd-5d4d-44b2-bf67-7a799834ddfd)

- **Pie chart of customer type**
![pie chart](https://github.com/DragnaRR/sales-insight/assets/95096810/5615d85f-83dd-4b66-b128-972000077300)

- **Dashboard of sales insight**
![sales insight](https://github.com/DragnaRR/sales-insight/assets/95096810/b6aed0ed-ba92-469b-8da4-f4ea70b0e07b)
