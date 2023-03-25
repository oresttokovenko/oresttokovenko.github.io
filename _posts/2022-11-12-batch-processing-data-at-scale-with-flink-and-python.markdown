---
layout: post
title: End-to-end ETL with Apache Flink, Snowflake and Quicksight
description:
date:   2022-11-19
image:  '/images/end_to_end_etl_with_apache_flink_snowflake_and_quicksight.jpg'
tags:   [Snowflake, Flink, Python]
---

### ⛔ This Blog post is a WIP  ⛔

---

### Theme: 

Analyzing the sales data of a fictional e-commerce company, with a focus on geographical insights.

### Project Overview:

This project aims to create an end-to-end ETL data pipeline for e-commerce sales data, ingesting it into Snowflake using Apache Flink for batch processing, and visualizing the data using Amazon Quicksight. This project will be deployed on AWS using Terraform.

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

### Ingestion/Transformation