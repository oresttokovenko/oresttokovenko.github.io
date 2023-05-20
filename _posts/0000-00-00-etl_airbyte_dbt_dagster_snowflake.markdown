---
layout: post
title: End-to-End ETL with Airbyte, dbt, Dagster, Snowflake and Metabase
description:
date:   2022-11-19
image:  '/images/etl_airbyte_dbt_dagster_snowflake.jpg'
tags:   [Airbyte, dbt, Python, Snowflake, Dagster, Metabase]
---

### ⛔ This Blog post is a WIP  ⛔

---

You can follow along on my [Github repo here](https://github.com/oresttokovenko/RetailFlow)

### Theme: 

Retail sales data for an e-commerce store 

### Project Overview:

RetailFlow is an end-to-end data engineering project that aims to generate fake retail sales data for an e-commerce store, ingest it into Snowflake using Airbyte, orchestrate and perform the data transformations using Dagster and dbt, and visualize the data using Metabase. This project will be deployed on AWS infrastructure using Terraform. 

### Data: 

The fake data will consist of the following attributes:

```
order_id
product_id
customer_id
order_date
quantity_sold
price
discount
shipping_cost
city
state
country
latitude
longtitude
```
<br><br/>
### Transformations:

* Calculate the total revenue (Quantity Sold * Price)
* Calculate the total discount (Quantity Sold * Discount)
* Calculate the net revenue (Total Revenue - Total Discount)
* Aggregate net revenue by city, state, and country

### Steps

1. Generate Fake Data: Use Faker in a Lambda function to generate a dataset of fake retail sales data hourly to simulate a production environment
2. Set up IaC using Terraform to set up all the AWS instances and deploy it
3. Create Snowflake Roles for Metabase, Airbyte, dbt
4. Ingest Data into Snowflake using Airbyte
5. Set up a dbt project and create dbt models to transform the raw retail data into aggregated, analytical tables
6. Set up a Dagster instance on AWS, and use Dagster's dbt and Snowflake integrations to manage the data transformation process.
7. Set up a Metabase instance on AWS using EC2, and configure it to connect to your Snowflake database using the previously created metabase_role. Create dashboards and visualizations based on the transformed data, such as Top-selling products, Sales trends over time, Sales performance by category