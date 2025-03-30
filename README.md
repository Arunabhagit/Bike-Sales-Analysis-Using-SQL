# Bike Sales Data Analysis Using SQL

# Project Overview

**Project Title** : Bike Sales Data Analysis

**Database** : Bike Data Store

**Tool Used** : PostgreSQL


This project showcases the implementation of a Bike Sales Data Analysis using SQL. It involves creating and managing tables, performing CRUD operations, and executing advanced SQL queries. The primary objective is to demonstrate proficiency in database manipulation and querying while uncovering valuable insights from the data.

![bike-shop-illustration-with-shoppers-people-choosing-cycles-accessories-or-gear-equipment-for-riding-in-flat-cartoon-background-design-vector](https://github.com/user-attachments/assets/dec28b77-ccc9-44c4-883e-d7d8410e2e37)

# Objectives

1. **Set up the Bike Store Database** :  Create and populate the database with tables for branchs, customers , categories , orders , staffs , stores.
2. **CRUD Operation** : Perform Create, Read, Update, and Delete operations on the data.
3. **CTAS** : Utilize CTAS to create new tables based on query results.
4. **Advanced SQL Queries**: develop complex queries to analyze and retrieve specific data.

# Project Setup 

**ERD Model** 

An **Entity-Relationship Diagram (ERD)** is a visual representation of a database's structure, illustrating entities (tables), their attributes (columns), and relationships (connections) between them. It helps design, understand, and optimize databases by mapping how data is organized and related, ensuring efficient database management and querying.

![Screenshot 2025-03-30 200750](https://github.com/user-attachments/assets/5f214004-677b-4f62-9e62-28a35424e502)

**Database Creation**: Created a database named Bike Data Store

**Table Creation** : Create table brands, categories , customers , order_items , orders , products , staff ,  stocks and stores 

```sql
-- Table Name 'brands'

create table brands(
brand_id int,
brand_name varchar(20)
);

-- Table Name 'categories' 

create table categories
(
category_id int,
category_name varchar(20)
);

-- Table Name 'customers' 

create table customers
(
customer_id int,
first_name varchar(20),
last_name varchar(20),
phone varchar(30),
email varchar(50),
street varchar(50),
city varchar(50),
state varchar(10),
zip_code int
);

-- Table Name 'order_items' 

create table order_items
(
order_id int,
item_id int,
product_id int,
quantity int,
list_price float,
discount float
);

-- Table Name 'orders'


create table orders
(
order_id int,
customer_id int,
order_status int,
order_date date,
required_date date,
shipped_date date,
store_id int,
staff_id int
);

-- Table Name 'products'

create table products
(
product_id int,
product_name varchar(60),
brand_id int,
category_id int,
model_year int,
list_price float
);

-- Table Name 'staff' 

create table staff
(
staff_id int,
first_name varchar(20),
last_name varchar(30),
email varchar(50),
phone varchar(80),
active int,
store_id int,
manager_id varchar(10)
);

-- Table Name 'stocks'

create table stocks
(
store_id int,
product_id int,
quantity int
);

-- Table Name 'stores'

create table stores
(
store_id int,
store_name varchar(30),
phone varchar(50),
email varchar(40),
street varchar(50),
city varchar(40),
state varchar(10),
zip_code int
);

```
# CRUD Operations : 

**Create** – Inserting new records into a database.

**Read** – Retrieving existing records from a database.

**Update** – Modifying existing records in a database.

**Delete** – Removing records from a database.

**Create** : Create a new record of a staff name as Jhon Humphrey into the staff table

```sql
insert into staff
values (11,'Jhon',' Humphrey','jhon.humphrey@bikes.shop','(516) 480-7845',1,2,7);
```
**Read** : Retrieve the names of customers who placed an order on April 11, 2018, along with the bike they ordered.

```sql
select c.first_name, c.last_name , p.product_name 
from products p
join order_items oi
on p.product_id = oi.product_id
join orders o
on oi.order_id = o.order_id
join customers c
on o.customer_id = c.customer_id
where o.order_date = '2018-04-11'
```

**Update** : Update The Price of  Heller Shagamaw Frame - 2016 bike increased 200 dollar.

```sql

update products
set list_price = '1520.99'
where product_name = 'Heller Shagamaw Frame - 2016'

```

**Delete** : Delete the record of the staff member named Marcelene Boyer because he has resigned.

```sql

select * from staff where first_name = 'Marcelene'

delete from staff
where staff_id = 6

```
 **CTAS (Create Table As Select)**:
Create a table showing the count of orders and the total sales amount for all bike brands.

```sql
create table brands_trend as 
select b.brand_name , count(o.order_id), round(sum(oi.list_price))
from brands b
join products p
on b.brand_id = p.brand_id
join order_items oi
on oi.product_id = p.product_id
join orders o
on oi.order_id = o.order_id
group by 1
order by 3 desc

select * from brands_trend
```

# Data Analysis &  Insights

* **List Top Five customer bought most expensive Bike(dollar)**

```sql

select c.first_name,c.last_name, o.order_id,oi.list_price as Price 
from customers c
join orders o 
on c.customer_id = o.customer_id
join order_items oi
on o.order_id = oi.order_id
group by o.order_id,c.first_name,c.last_name,Price 
order by Price desc
limit 5

```
**Insights and Decision** : Identifying customers who are willing to spend the most on premium bikes.
Tailoring marketing efforts to target high-spending customers with exclusive offers or loyalty programs.

![Screenshot 2025-03-30 214552](https://github.com/user-attachments/assets/c63f9d4e-9546-4ade-a3cd-cfbd83884717)


* **Top 3 Brands of bike which got highest Orders**

```sql

select b.brand_name, count(o.order_id) as Number_of_orders
from brands b
join products pc
on b.brand_id = p.brand_id
join order_items oi
on p.product_id = oi.product_id
join orders o
on oi.order_id = o.order_id
group by 1
order by 2 desc
limit 3

```
**Insights and Decision** : Identifying the most in-demand bike brands among customers. Investing in promotions and advertising for top-performing brands.

![graph_visualiser-1743353395235](https://github.com/user-attachments/assets/937e8549-c12d-419e-a43f-450229d99a3b)

* **Name of top 10 Bikes who got most discount**

 ```sql
select b.brand_name, c.category_name, p.product_name , oi.discount * 100 as Discount_Percent
from brands b
join products p
on p.brand_id = b.brand_id
join categories c
on p.category_id = c.category_id
join order_items oi
on p.product_id = oi.product_id
group by 1,2,3,4
order by 4 desc
limit 10
 ```
**Insights and Decision** :   Understanding which bikes receive the highest discounts and whether this leads to increased sales.Identifying if high discounts are affecting overall profitability.

![Screenshot 2025-03-30 222643](https://github.com/user-attachments/assets/1aab098e-90df-4b2a-a155-d8eb5b5ffbcb)


* **Count number of Bikes has not shipped for each Brands**

```sql
select b.brand_name , count(o.order_id) as Number_of_items_not_shipped 
from brands b 
join products p
on p.brand_id = b.brand_id
join order_items oi
on oi.product_id = p.product_id
join orders o
on o.order_id = oi.order_id
where shipped_date is null
group by 1
order by 2 desc
```
**Insights and Decision** : High numbers of unshipped bikes could indicate logistical or supply chain problems.Optimize shipping processes to reduce unshipped orders.

![Screenshot 2025-03-30 231115](https://github.com/user-attachments/assets/06d07393-dfc1-444b-bf54-9ac82909e725)

* **Bike brand sales across different cities**

 ```sql
select b.brand_name , c.city , count(o.order_id) as number_of_orders,

ROW_Number() over(partition by  b.brand_name order by count(o.order_id) desc) as rank
from brands b 
join products p
on p.brand_id = b.brand_id
join order_items oi
on oi.product_id = p.product_id
join orders o
on oi.order_id = o.order_id
join customers c
on c.customer_id = o.customer_id
group by  b.brand_name,c.city
 ```

**Insights and Decision** :  Identifying which bike brands perform best in different cities.Ensuring efficient supply chain management to meet city-wise demand.Promote specific bike brands based on regional preferences.


* **Every year sales of bikes from each store**

  ```sql

  SELECT 
        s.store_name, b.brand_name ,
        TO_CHAR(o.order_date, 'YYYY') AS years, 
        ROUND(SUM(oi.list_price)) AS yearly_Sales
    from stores s
	join orders o
	on s.store_id = o.store_id
	join order_items oi
	on oi.order_id = o.order_id
	join products p
	on p.product_id = oi.product_id
	join brands b
	on b.brand_id = p.brand_id
    GROUP BY s.store_name, b.brand_name,years
	order by  s.store_name 
  ```

**Insights and Decision** : Identify stores with increasing or declining sales trends.Stock more bikes in high-performing stores and reduce excess inventory in low-performing ones.

* **Monthly sales of each bike brands and 1 year total revenue**

  ```sql

  WITH MonthlySales AS (
    SELECT 
        b.brand_name, 
        TO_CHAR(o.order_date, 'MM') AS months, 
        ROUND(SUM(oi.list_price)) AS Monthly_Sales
    FROM brands b
    JOIN products p ON p.brand_id = b.brand_id
    JOIN order_items oi ON oi.product_id = p.product_id
    JOIN orders o ON o.order_id = oi.order_id
    GROUP BY b.brand_name, TO_CHAR(o.order_date, 'MM')
)
SELECT 
    brand_name, 
    months, 
    Monthly_Sales,
    SUM(Monthly_Sales) OVER(
        PARTITION BY brand_name 
        ORDER BY months::INTEGER
    ) AS Cumulative_Sales 
FROM MonthlySales
ORDER BY brand_name, months::INTEGER;

  ```
**Insights and Decision** :
