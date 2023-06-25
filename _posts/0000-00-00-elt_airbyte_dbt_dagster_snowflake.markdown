---
layout: post
title: End-to-End ELT with Airbyte, dbt, Dagster, Snowflake and Metabase
description:
date:   2023-06-10
image:  '/images/elt_airbyte_dbt_dagster_snowflake.jpg'
tags:   [Airbyte, dbt, Python, Snowflake, Dagster, Metabase]
---

### ⛔ This Blog post is a WIP, but you can follow along on the [Github repo here](https://github.com/oresttokovenko/RetailFlow) ⛔

---

Greetings, fellow data practitioners. If you've stumbled upon this blog, chances are you're knee-deep in a similar trench to mine - untangling the serpentine complexities of data engineering. Today, I'll be sharing my journey through building my project that I named RetailFlow - a thrilling escapade through the enchanting labyrinth of data extraction, loading, transformation, and visualization which you can see in the diagram below. 

{:refdef: style="text-align: center;"}
![Diagram]({{site.baseurl}}/images/retailflow_diagram_1.jpg)
{: refdef}

I always wanted to build an end-to-end ELT pipeline for an e-commerce platform from scratch, just to see what it's like! I was up against a seemingly impenetrable wall of data, and my trusted weapons were my favorite cloud playground, AWS, and a suite of state-of-the-art tools: Airbyte, dbt, Dagster, Snowflake, and Metabase. However, it wasn't enough to run these tools locally, I wanted to be able to create all the infrasctrue on the AWS cloud with a single command! A daunting task indeed, but one that left me positively giddy with anticipation.

## Following the data through the pipeline

The first leg of our journey, the data generation, was performed by a Python script. If you ask me, Python is to a data engineer what spinach is to Popeye, and my spinach-fuelled script was running tirelessly within an AWS Lambda function, feeding our PostgreSQL database on AWS EC2 using SQLAlchemy to push the generated data. Once provisioned, we can view the Python script using the make command below:

```bash
make print-lambda
```
<br>

Also, let's check to make sure the data made it into the PostgresDB. First we can ssh into the ec2 instance. 

```bash
ssh-ec2-postgres # the ec2 public ip will be added to the .env file during terraform provisioning, so that's what the make command will grab
```
<br>

Next we can check for the Docker container id and connect to it.

```bash
docker ps

docker exec -it <container-id> /bin/bash
```
<br>

Lastly, we can connect to the postgresql database and query the Sales table

```bash
psql -h localhost -p 5432 -U retailflow_admin -d retailflow_db # It will indicate that it requires a password, at which point psql will prompt you for the password which is `retailflow123`

```

```sql
select * from Sales;
```
<br>

Next, we took a ride on the Airbyte, moving data from PostgreSQL to our data warehouse, Snowflake. Interestingly, I discovered that Airbyte runs on Docker itself and therefore cannot be containerized! Therefore, with Airbyte running on its own EC2 instance with a simple Bash setup script, it was like having a personal chauffer dedicated to delivering data to Snowflake's frosty schema. Airbyte's load time can run as frequently as every five minutes, so I set the Lambda function to run every 3 minutes to simulate a CDC production environment. 

Then came the transformation phase, the crucible where raw data is hammered and shaped into insightful information. I chose dbt and Dagster for this task - two power tools that can make any data engineer feel like Thor with a hammer. Here again, AWS EC2 played the faithful steed, hosting both tools within one Docker container to simplify interoperability. These are the transformations that were made in dbt:

* total revenue (Quantity Sold * Price)
* total discount (Quantity Sold * Discount)
* net revenue (Total Revenue - Total Discount)
* net revenue by city, state, and country

For the sake of this project, we won't wait for a scheduled execution and we'll kick off the dbt pipeline with dagster by manually executing the job once we know the data has made its way to Snowflake. I waited half an hour so that the pipeline had time to ingest the data few times.

```python
dagster job execute -f dbt_sales_staging.py
```

<br>


Finally, we had Metabase, the artist in our process, turning raw data sculptures into beautiful canvases of analytics, providing our stakeholders with insights as crisp as an autumn morning. As you might have guessed, Metabase, too, had its exclusive EC2 instance, churning out data masterpieces. If you want to run Metabase in production, you’ll need store your application data in a production-ready database, but for the purpose of this project I used the embedded H2 database that it ships with. The reason it's necessary to have a dedicated database is because if you remove the container, you’ll lose your Metabase application data (your questions, dashboards, collections, and so on). Another interesting observation that I made is that Metabase does not support configuring data source connections through environment variables or the Dockerfile directly. This is largely due to security reasons, as connection details for databases often include sensitive information like passwords and API keys, so you'll need to open the Metabase GUI and set up the connection to Snowflake manually. 

## How does connecting EC2 instances work?

Ensuring both EC2 instances are in the same VPC and Security Group

{:refdef: style="text-align: center;"}
![Diagram]({{site.baseurl}}/images/retailflow_diagram_2.jpg)
{: refdef}

Ports, lots of ports! 

Lastly, in a production environment, consider encrypting network traffic between instances with tools such as SSL/TLS.

## Conclusion

All this would be a house of cards if not for the magic of orchestration. Here, Terraform was my sorcerer's wand, creating and managing our infrastructure, while Docker served as our trusty timekeeper, ensuring consistency across development and production. AWS ECS, on the other hand, was like an experienced symphony conductor, gracefully managing our Docker containers across multiple EC2 instances. Since Airbyte cannot be containerized, I simply had Terraform execute the installation bash script on its EC2 instance once it was provisioned.

Reflecting on this project, it becomes evident how intricate and multifaceted an end-to-end ELT pipeline truly is. Yet, the successful completion of RetailFlow is a testament to the power of effective orchestration and strategic tool selection, even in the face of substantial complexity.