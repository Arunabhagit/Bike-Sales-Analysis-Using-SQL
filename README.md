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

**CRUD Operations** : 

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

**Update** : Update The Price of  Heller Shagamaw Frame - 2016 bike increased 200 dollar

```sql

update products
set list_price = '1520.99'
where product_name = 'Heller Shagamaw Frame - 2016'

'''





