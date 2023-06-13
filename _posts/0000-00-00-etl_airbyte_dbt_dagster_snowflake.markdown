---
layout: post
title: End-to-End ETL with Airbyte, dbt, Dagster, Snowflake and Metabase
description:
date:   2023-06-10
image:  '/images/etl_airbyte_dbt_dagster_snowflake.jpg'
tags:   [Airbyte, dbt, Python, Snowflake, Dagster, Metabase]
---

### ⛔ This Blog post is a WIP  ⛔

---

You can follow along on my [Github repo here](https://github.com/oresttokovenko/RetailFlow)

### Theme: 

Retail sales data for an e-commerce store 

### Project Overview:

RetailFlow is a comprehensive ELT (Extract, Load, Transform) project designed to simulate the flow of retail sales data for an e-commerce platform. The infrastructure is provisioned and managed on AWS, with each service optimized for its specific role in the pipeline.

The data simulation is handled by a Python script executing within an AWS Lambda function. The generated data is then pushed to a PostgreSQL database instance deployed on AWS EC2.

Data is ingested using Airbyte into the data warehousing solution, Snowflake. Airbyte operates on its own EC2 instance, ensuring dedicated resources for the critical task of data synchronization.

For the transformation phase, we utilize a combination of Dagster and dbt, two cutting-edge tools in the data engineering ecosystem. These tools are deployed on an EC2 instance, allowing for a flexible and powerful transformation process.

The final piece of the pipeline is data visualization, which is handled by Metabase. Running on a dedicated EC2 instance, Metabase provides intuitive and insightful data analytics, allowing stakeholders to extract meaningful conclusions from the data.

The entire system is orchestrated using Terraform, an Infrastructure as Code (IaC) tool that simplifies and standardizes infrastructure deployment. On the application level, we utilize Docker for containerization, ensuring consistency across all stages of development and production. Finally, the orchestration of our Docker containers across multiple EC2 instances is managed by AWS ECS (Elastic Container Service), providing a robust, scalable, and efficient solution to our multi-container deployment needs.


### Data: 

The fake data will consist of the following attributes:

```
order_id
cust_name
product
product_category
quantity
price
purchased_at
product_id
customer_id
order_date_at
discount
shipping_cost
city
province
country
```
<br><br/>
### Transformations:

These are the transformations that will be made in dbt

* Calculate the total revenue (Quantity Sold * Price)
* Calculate the total discount (Quantity Sold * Discount)
* Calculate the net revenue (Total Revenue - Total Discount)
* Aggregate net revenue by city, state, and country

### To be added

- Metabase does not support configuring data source connections through environment variables or the Dockerfile directly. This is largely due to security reasons, as connection details for databases often include sensitive information like passwords and API keys.

- Encountered this [issue](https://discourse.getdbt.com/t/invalid-value-for-profiles-dir-path-dbt-does-not-exist-error-gets-thrown-despite-this-path-existing/8228)


- How does connecting EC2 instances work?

If you want to transfer data between two EC2 instances, there are several approaches you could consider. However, since your scenario involves Dockerized applications - specifically, a Postgres DB and Airbyte - the "data transfer" is actually more about enabling network communication between the two applications rather than physically moving data between instances.

Ensure both EC2 instances are in the same VPC and Security Group: This allows network traffic between the instances. You should set up your Security Group rules to allow incoming connections on the necessary ports for your applications. For Postgres, this is usually port 5432. For Airbyte, refer to their documentation for necessary ports.
Ensure Postgres is set up to allow connections from other instances: By default, Postgres only accepts connections from the same machine it's running on. You'll need to modify the pg_hba.conf file to accept connections from other IP addresses.
Ensure your Dockerized Postgres is exposing the correct port: When you run your Postgres Docker container, you need to use the -p flag to map the container's port to the host's port, like so: docker run -p 5432:5432 .... This way, other instances can connect to Postgres on its standard port.
Connect Airbyte to Postgres: Now, you should be able to set up a connection in Airbyte using the private IP address of your Postgres EC2 instance and the standard Postgres port (5432). You'll also need to provide the username, password, and database name.
Remember to ensure your data is secure during transit. In a production environment, consider encrypting network traffic between instances with tools such as SSL/TLS.