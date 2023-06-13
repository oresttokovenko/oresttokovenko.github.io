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

Greetings, fellow data aficionados. If you've stumbled upon this blog, chances are you're knee-deep in a similar trench to mine - untangling the serpentine complexities of data engineering. Today, I'll be sharing my journey through building my project that I named RetailFlow- a thrilling escapade through the enchanting labyrinth of data extraction, loading, transformation, and visualization.

I always wanted to build an end-to-end ELT pipeline for an e-commerce platform from scratch, just to see what it's like! I was up against a seemingly impenetrable wall of data, and my trusted weapons were my favorite cloud playground, AWS, and a suite of state-of-the-art tools: Airbyte, dbt, Dagster, Snowflake, and Metabase. A daunting task indeed, but one that left me positively giddy with anticipation.

The first leg of our journey, the data generation, was performed by a Python script. If you ask me, Python is to a data engineer what spinach is to Popeye, and my spinach-fuelled script was running tirelessly within an AWS Lambda function, feeding our PostgreSQL database on AWS EC2 with the lifeblood of our project: data.

Next, we took a ride on the Airbyte, moving data from PostgreSQL to our data warehouse, Snowflake. Initerstly I discovered that Airbyte runs on Docker so it itself cannot be containerized! Therefore, with Airbyte running on its own EC2 instance, it was like having a personal chauffer dedicated to delivering data to our Snowflake's frosty domain.

Then came the transformation phase, the crucible where raw data is hammered and shaped into insightful information. I chose dbt and Dagster for this task - two power tools that can make any data engineer feel like Thor with a hammer. Here again, AWS EC2 played the faithful steed, hosting both tools.

Finally, we had Metabase, the artist in our process, turning raw data sculptures into beautiful canvases of analytics, providing our stakeholders with insights as crisp as an autumn morning. As you might have guessed, Metabase, too, had its exclusive EC2 instance, churning out data masterpieces.

All this would be a house of cards if not for the magic of orchestration. Here, Terraform was my sorcerer's wand, creating and managing our infrastructure, while Docker served as our trusty timekeeper, ensuring consistency across development and production. AWS ECS, on the other hand, was like an experienced symphony conductor, gracefully managing our Docker containers across multiple EC2 instances.

Reflecting on this project, it becomes evident how intricate and multifaceted an end-to-end ELT pipeline truly is. Yet, the successful completion of RetailFlow is a testament to the power of effective orchestration and strategic tool selection, even in the face of substantial complexity.