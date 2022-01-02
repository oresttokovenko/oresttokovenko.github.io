---
layout: post
title:  Study Guide for AWS Data Analytics Specialty Certification
description:
date:   30 December 2021
image:  '/images/aws-das.jpg'
tags:   [Cloud Computing, AWS, Study Guide]
---

This study guide covers AWS Certification for Data Analytics Specialty. This exam replaces the former AWS Big Data Certification, but otherwise covers the same topics. The exam consists of 65 questions, and you have 180 minutes to write it. The study guide below covers everything you need to know for it. To study for this exam I did the following:
 * Watched the official AWS Digital Training course
 * Read the *AWS Data Analytics Specialty Exam Guide* accessible [here](https://d1.awsstatic.com/training-and-certification/docs-data-analytics-specialty/AWS-Certified-Data-Analytics-Specialty_Exam-Guide.pdf)
 * Read the *AWS Certified Data Analytics Study Guide* by Sybex
 * Watched the *AWS Certified Data Analytics Specialty 2022 - Hands On!* Udemy course by Stéphane Maarek
 * Watched the *AWS Certified Data Analytics - Specialty* Whizlabs Course
 * Read the following FAQ papers linked below:
   * [Redshift FAQ](https://aws.amazon.com/redshift/faqs/)
   * [EMR FAQ](https://aws.amazon.com/emr/faqs/)
   * [Athena FAQ](https://aws.amazon.com/athena/faqs/)
   * [CloudSearch FAQ](https://aws.amazon.com/cloudsearch/faqs/)
   * [Kinesis Video Streams FAQ](https://aws.amazon.com/kinesis/video-streams/faqs/)
   * [Kinesis Data Streams FAQ](https://aws.amazon.com/kinesis/data-streams/faqs/)
   * [Kinesis Data Firehose FAQ](https://aws.amazon.com/kinesis/data-firehose/faqs/)
   * [Kinesis Data Analytics FAQ](https://aws.amazon.com/kinesis/data-analytics/faqs/)
   * [Elasticsearch FAQ](https://aws.amazon.com/kinesis/data-analytics/faqs/)
   * [Managed Kafka FAQ](https://aws.amazon.com/msk/faqs/)
   * [Quicksight FAQ](https://aws.amazon.com/quicksight/resources/faqs/)
   * [Data Exchange FAQ](https://aws.amazon.com/data-exchange/faqs/)
   * [Glue FAQ](https://aws.amazon.com/glue/faqs/)
   * [Lake Formation FAQ](https://aws.amazon.com/lake-formation/faqs/)
   * [Data Pipeline FAQ](https://aws.amazon.com/datapipeline/faqs/)
 * Completed the official AWS practice test that I received after passing the AWS Certified Cloud Practitioner exam

According to Amazon Web Services, this exam will test the following services and features

 * **Analytics**:
   * Amazon Athena 
   * Amazon CloudSearch 
   * Amazon Elasticsearch Service (Amazon ES)
   * Amazon EMR 
   * AWS Glue 
   * Amazon Kinesis (excluding Kinesis Video Streams)
   * AWS Lake Formation 
   * Amazon Managed Streaming for Apache Kafka 
   * Amazon QuickSight 
   * Amazon Redshift
 * **Application Integration**:
   * Amazon MQ
   * Amazon Simple Notification Service (Amazon SNS)
   * Amazon Simple Queue Service (Amazon SQS)
   * AWS Step Functions 
 * **Compute**:
   * Amazon EC2 
   * Elastic Load Balancing 
   * AWS Lambda
 * **Customer Engagement**:
   * Amazon Simple Email Service (Amazon SES)
 * **Database**:
   * Amazon DocumentDB (with MongoDB compatibility)
   * Amazon DynamoDB 
   * Amazon ElastiCache 
   * Amazon Neptune 
   * Amazon RDS 
   * Amazon Redshift 
   * Amazon Timestream 
 * **Management and Governance**:
   * AWS Auto Scaling 
   * AWS CloudFormation 
   * AWS CloudTrail 
   * Amazon CloudWatch 
   * AWS Trusted Advisor 
 * **Machine Learning**:
   * Amazon SageMaker 
 * **Migration and Transfer**:
   * AWS Database Migration Service (AWS DMS)
   * AWS DataSync 
   * AWS Snowball 
   * AWS Transfer for SFTP 
 * **Networking and Content Delivery**:
   * Amazon API Gateway 
   * AWS Direct Connect 
   * Amazon VPC (and associated features)
 * **Security, Identity, and Compliance**:
   * AWS AppSync 
   * AWS Artifact
   * AWS Certificate Manager (ACM)
   * AWS CloudHSM 
   * Amazon Cognito 
   * AWS Identity and Access Management (IAM)
   * AWS Key Management Service (AWS KMS)
   * Amazon Macie 
   * AWS Secrets Manager 
   * AWS Single Sign-On 
 * **Storage**:
   * Amazon Elastic Block Store (Amazon EBS)
   * Amazon S3 
   * Amazon S3 Glacier

---

### _Study Guide_

### Collection
* #### Determine the operational characteristics of the collection system
  * **Fault Tolerance and Data Persistence**
    * The Kinesis Producer library can send a group of multiple records in each request
      * If a record fails, it's put back into the KPL buffer for a retry
      * One record's failure doesn't fail a whole set of records
        {:refdef: style="text-align: center;"}
        ![Badge]({{site.baseurl}}/images/kpl.jpg)
        {: refdef}
* #### Select a collection system that handles the frequency, volume, and the source of data
* #### Select a collection system that addresses the key properties of data, such as order, format, and compression
### Storage and Data Management
* #### Determine the operational characteristics of the storage solution for analytics
* #### Determine data access and retrieval patterns
* #### Select appropriate data layout, schema, structure, and format
* #### Define data lifecycle based on usage patterns and business requirements
* #### Determine the appropriate system for cataloging data and managing metadata
### Processing
* #### Determine appropriate data processing solution requirements
* #### Design a solution for transforming and preparing data for analysis
* #### Automate and operationalize data processing solutions
### Analysis and Visualization
* #### Determine the operational characteristics of the analysis and visualization solution
* #### Select the appropriate data analysis solution for a given scenario
* #### Select the appropriate data visualization solution for a given scenario
### Security
* #### Select appropriate authentication and authorization mechanisms
* #### Apply data protection and encryption techniques
* #### Apply data governance and compliance controls

Once you pass, you'll earn the beautiful badge below! 

{:refdef: style="text-align: center;"}
![Badge]({{site.baseurl}}/images/data-analytics-specialty-badge.jpg)
*AWS DAS Badge*
{: refdef}