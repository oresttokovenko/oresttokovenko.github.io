---
layout: post
title:  Study Guide for AWS Data Analytics Specialty Certification
description:
date:   30 December 2021
image:  '/images/aws-das.jpg'
tags:   [Cloud Computing, AWS, Study Guide]
---

# ⛔️ ️This Blog post is a WIP ⛔️

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
  * ##### Fault Tolerance and Data Persistence
    * The Kinesis Producer library can send a group of multiple records in each request to your shards
      * If a record fails, it's put back into the KPL buffer for a retry (Fault Tolerance)
      * One record's failure doesn't fail a whole set of records
      * The KPL also has rate limiting
        * Limits per-shard throughput sent from a single producer, can help prevent excessive retries 
        {:refdef: style="text-align: center;"}
        ![Badge]({{site.baseurl}}/images/kpl.jpg)
        {: refdef}
  * ##### Availability and Durablity of your Ingestion Components
    * Kinesis Data Streams replicates your data synchronously across three AZs in one region
    * Don't use Kinesis Data Streams for protracted data persistence
      * Your data is retained for 24 hours, which can be extended to 7 days
    * Kinesis Data Firehose streams your data directly to a data destination, no retention
       {:refdef: style="text-align: center;"}
       ![Badge]({{site.baseurl}}/images/aws_das_1.1.jpg)
       {: refdef}
      * Destinations: S3, Redshift, Elasticsearch, Splunk and Kinesis Data Analytics
      * Can transform your data, using a Lambda function, prior to delivering the data
  * ##### Fault Tolerance of your Ingestion Components
    * The Kinesis Consumer Library processes your data from your Kinesis Data Stream
      * Uses checkpointing using DynamoDB to track which records have been read from a shard
        * If a KCL read fails, the KCL uses the checkpoint cursos to resume at the failed record
    * Important facts
      * Use unique names for your application in the KCL, since DynamoDB tables use names
      * Watch out for provisioning throughput exception in DynamoDB: Too many shards or frequent checkpoint
    * Alternatives to the KPL
      {:refdef: style="text-align: center;"}
      ![Badge]({{site.baseurl}}/images/aws_das_1.2.jpg)
      {: refdef}
      * Use the Kinesis API instead of KPL when you need the fastest procesing time
        * KPL uses RecordMaxBufferedTime to delay processing to accommodate aggregation
      * Kinesis Agent
        * Kinesis Agent installs on your EC2 instance
        * Monitors files, such as log files, and streams new data to your Kinesis stream
        * Emits CloudWatch metrics to help with monitoring and error handling
  * ##### Summary - Determine the operational characteristics of the collection system
    * Two key concepts to remember for the exam
      * Fault tolerance
      * Data persistence
    * Kinesis Data Streams vs. Kinesis Data Firehose
      * Data persistence is the key difference
    * Kinesis Producer Library vs. Kinesis API vs. Kinesis Agent
      * Fault tolerance and appropriate tools for your data collection problem
  * #### Data Collection through Real-Time Streaming Data
    * Kinesis Data Firehose is fully managed
    * Destinations: S3, Redshift, Elasticsearch, Splunk, Kinesis Data Analytics
    * Can optionally transform data, using Lambda, before deliveirng it to its destination
      {:refdef: style="text-align: center;"}
      ![Badge]({{site.baseurl}}/images/aws_das_1.3.jpg)
      {: refdef}
    * Firehose to Redshift
      * Delivers directly to S3 first
      * Firehose then runs a Redshift COPY command
      * Can optionally transform your data, using Lambda, before delivering it to its destination
    * Firehose to Elasticsearch Cluster
      * Firehose delivers directly to Elasticsearch cluster
      * Can optionally backup to S3 concurrently
    * Firehose to Splunk
      * Firehose delivers directly to Splunk instance
      * Can optionally backup to S3 concurrently
    * Firehose Producers
      * Firehose producers send records to Firehose
        * Web server logs data
        * Kinesis Data Stream
        * Kinesis Agent
        * Kinesis Firehose API using the AWS SDK
        * CloudWatch logs and/or events
        * AWS IoT
        * Firehose buffers incoming streaming data for a set buffer size (MBs) and a buffer interval (seconds). You can manipulate the buffer size in the buffer interval to speed up or slow down your firehose delivery speed
          * use case: you're delivering streaming data from Firehose to an S3 bucket, how might you speed up the delivery of your Kinesis Data? Lower the buffer size and lower the buffer interval
* #### Select a collection system that handles the frequency, volume, and the source of data
  * ##### The Four Ingestion Services
    * Kinesis Data Streams
      * Use cases needing custom processing and different stream processing frameworks where sub-second processing latency is needed
    * Kinesis Data Firehose
      * Use cases needing managed service streaming to S3, Redshift, Elasticsearch, or Splunk where data latency of 60 seconds or higher is acceptable
    * AWS Database Migration Service
      * Use cases needing one-time migration and/or continuous replication of database records and structures to AWS services
    * AWS Glue
      * Use cases needing ETL batch-oriented jobs where scheduling of ETL jobs is required
    * #### Kinesis Data Streams
      * Each shard supports
        * 1,000 RPS for writes with max of 1 MB/sec
        * 5 TPS for reads at a max of 2MB/sec using GetRecords API Call
        * Stream total capacity equals the sum of the capacity of the shards
        * No limit to the number of shards you can provision
          {:refdef: style="text-align: center;"}
          ![Badge]({{site.baseurl}}/images/aws_das_1.4.jpg)
          {:refdef}
    * #### Kinesis Data Firehose
      * Firehose automatically delivers to specified destination
      * Buffers incoming streaming data to specified size or for a specified period of time before delivering to destinations
        * Buffer size is in MBS AND buffer interval is in seconds
      * Automatically scales to match data throughput
        * No manual intervention or developer overhead required
        {:refdef: style="text-align: center;"}
        ![Badge]({{site.baseurl}}/images/aws_das_1.5.jpg)
        {:refdef}
    * #### Data Migration Service
      * DMS is used to take data from a source database to a destination target (database, data lake, data warehouse)
      * Source database remains fully operational during the migration
      * Supports several data engines as a source and target for data replication
      * Uses tasks to capture ongoing changes after initial migration to a supported target data store
        * Ongoing replication or Change Data Capture
      * Uses databse engine's APIs to read changes from transaction log then replicate to target database
      * EC2 replication instance, scale to meet utilization requirements
        {:refdef: style="text-align: center;"}
        ![Badge]({{site.baseurl}}/images/aws_das_1.6.jpg)
        {:refdef}
    * #### AWS Glue
      * Key Point: Batch oriented
        * Micro-batches but no streaming data
      * Does not support NoSQL databases as data source
      * Crawl data source to populate data catalog
      * Generate a script to transform data or write your own in console or API
      * Run jobs on demand or via a trigger event
      * Glue catalog tables contain metadata not data from the data source
      * Uses a scale-out Apache Spark environment when loading data to destination
        * Allocate data processing units (DPUs) to ETL jobs
      {:refdef: style="text-align: center;"}
      ![Badge]({{site.baseurl}}/images/aws_das_1.7.jpg)
      {:refdef}
  * ##### The Three Types of Data to Ingest
    * Batch Data
      * Applications logs, video files, audio files, etc.
      * Larger event payloads ingested in an hourly, daily, or weekly daily
      * Ingested in intervals from aggregated data
    * Streaming Data
      * Click-stream data, IoT sensor data, stock ticker data, etc.
      * Ingesting large amounts of small records continuously and in real-time
    * Transactional Data
      * Initially load and receive continuous updates from data stores used as operational business databases
      * Similar to batch data but with a continous update flow
      * Ingested from databases storing transactional data
    * #### Batch Data
      * Data is usually 'colder' and can be processed on less frequent intervals
        * Use AWS batch-oriented services like Glue
        * EMR support batch processing on a large scale
      * Latency is minutes to hours
      * Complex analytics across big data sets
    * #### Streaming Data
      * Often bound by time or event sets in order to produce real-time outcomes
      * Data is usually 'hot' arriving at a high frequency that you need to analyze in real-time
        * Use Kinesis Data Streams, Kinesis Data Firehose, Kinesis Data Analytics
        * Can load data into data lake or data warehouse
      * Individual records or aggregated micro-batches 
      * low-latency, i.e. milliseconds
      * Simpler analytics, rolling metrics, aggregations
    * #### Transactional Data
      * Data stored at low latency and quikcly accessible
      * Load data from a database on-prem or in AWS
      * Use Database Migration Service
      * Can load data from relational databases, data warehouses and NoSQL databases
      * Can convert to different database systems using the AWS Schema Conversion Tool (AWS SCT) then use DMS to migrate your data
  * ##### Comparing the Data Collection Systems
    * Understand how each ingestion approach is best used
      * Throughput, bandwidth, scalability
      * Availability and fault tolerance
      * Cost of running the services
    * Understand Differences between Services
    <div class="table-container">
      <table>
        <tr><th>Kinesis Data Streams</th><th>Kinesis Data Firehose</th><th>Data Migration Service</th><th>Glue</th></tr>
        <tr><td>Use when you need custom producers and consumers</td><td>Use cases where you want to deliver directly to S3, Redshift, Elasticsearch, or Splunk</td><td>Use cases when you need to migrate data from one database to another</td><td>Batch-oriented use cases where you want to perform an Extract Transform Load(ETL) process</td></tr>
        <tr><td>Use cases that require sub-second processing</td><td>Use cases where you can tolerate latency of 60 seconds or greater</td><td>Use cases where you want to migrate a database to a different database engine</td><td>Not for use with streaming use cases</td></tr>
        <tr><td>Use cases that require unlimited bandwidth</td><td>Use cases where you wish to transform your data or convert the data format</td><td></td><td></td></tr>
      </table>
    </div>
    * Throughput, Bandwidth, Scalability
    <div class="table-container">
      <table>
        <tr><th>Kinesis Data Streams</th><th>Kinesis Data Firehose</th><th>Data Migration Service</th><th>Glue</th></tr>
        <tr><td>Shards can handle up to 1,000 PUT records per second</td><td>Automatically scales to accommodate the throughput of your stream</td><td>EC2 instances used for the replication instance</td><td>Runs in a scale-out Apache Spark environment to move data to target system</td></tr>
        <tr><td>Can increase the number of shards in a stream without limit</td><td></td><td>You need to scale your replication instance to accommodate your throughput</td><td>Scales via Data Processing Units (DPU) for your ETL jobs</td></tr>
        <tr><td>Each shard has a capacity of 1 MB/s for input and 2 MB/s output</td><td></td><td></td><td></td></tr>
      </table>
    </div>
    * Availability and Fault Tolerance
    <div class="table-container">
      <table>
        <tr><th>Kinesis Data Streams</th><th>Kinesis Data Firehose</th><th>Data Migration Service</th><th>Glue</th></tr>
        <tr><td>Synchronously replicates your shard data across 3 Availability Zones</td><td>Synchronously replicates your shard data across 3 Availability Zones</td><td>Can use multi-AZ for replication that gives you fault tolerance via redundant replication servers</td><td>Retries 3 times before marking an error condition</td></tr>
        <tr><td></td><td>For S3: Firehose retries for 24 hours,if failure persists past 24 hours your data is lost</td><td></td><td>Create a CloudWatch alert for failures that triggers an SNS message</td></tr>
        <tr><td></td><td>For Redshift, you can specify a retry duration from 0 to 7,200 seconds</td><td></td><td></td></tr>
        <tr><td></td><td>For Elastic Search, you can specify a retry duration from 0 to 7,200 seconds</td><td></td><td></td></tr>
        <tr><td></td><td>For Splunk, you can use a retry duration counter. Firehose retires until counter expires, then backs up your data to S3</td><td></td><td></td></tr>
        <tr><td></td><td>Retries may cause duplicate records</td><td></td><td></td></tr>
      </table>
    </div>
    * Understand how each service incurs cost
    <div class="table-container">
      <table>
        <tr><th>Kinesis Data Streams</th><th>Kinesis Data Firehose</th><th>Data Migration Service</th><th>Glue</th></tr>
        <tr><td>Pay per shard hour and PUT payload unit</td><td>Pay for the volume of data ingested</td><td>Pay for the EC2 compute resources you use when migrating</td><td>Pay an hourly rate at a billing per second for both crawlers and ETL jobs</td></tr>
        <tr><td>Extended data retention and enhanced fanout incur additional cost</td><td>Pay for data conversions</td><td>Pay for log storage</td><td>Monthly fee for storing and accessign data in your Glue data catalogue</td></tr>
        <tr><td></td><td></td><td>Data transfer fees</td><td></td></tr>
      </table>
    </div>
* #### Select a collection system that addresses the key properties of data, such as order, format, and compression
    * Managing Data Order, Format and Compression
      * Problems with your streaming data
        * Data that is out of order
        * Data that is duplicated
        * Data where we need to change the format
        * Data that needs to be compressed
      * Methods to address these problems
        * Choose an ingestion service that has guaranteed ordering
          * Kinesis Data Streams
          * DynamoDB Streams
        * Choose an ingestion service that has deduping capacity
          * DynamoDB Streams - exactly-once
          * Kinesis Data Streams - at-least-once
            * Embed a primary key in data records and remove duplicates later when processing
          * Kinesis Data Firehose - at-least-once
            * Crawl target data with Glue ML FindMatches Transform
        * Use conversion or compression feature of ingestion service
          * Kinesis Data Streams
            * Use Lambda consumer to format or compress
            * Use KCL application to format or compress
          * Kinesis Data Firehose
            * Use format conversion feature if data in JSON
            * Use Lambda transform to preprocess format conversion feature if data not JSON
            * Use S3 compression (GZIP, Snappy, or Zip)
            * Use GZIP COPY command option for Redshift compression
          {:refdef: style="text-align: center;"}
          ![Badge]({{site.baseurl}}/images/aws_das_1.8.jpg)
          {:refdef}
    * Transforming Data when Ingesting
      * Kinesis Data Firehose
        * Using Lambda, Firehose sends buffered batches to Lambda for transformation
        * Batch, encrypt, and/or compress data
      * Lambda
        * Convert the format of your data, e.i. GZIP -> JSON
        * Transform your data, e.g. expand strings into individual columns
        * Filter your data to remove extraneous info
        * Enrich your data, e.g. error correction
      * Database Migration Service
        * Table and schema transformations, e.g. change table, schema and/or column names
### Storage and Data Management
* #### Determine the operational characteristics of the storage solution for analytics
  * Selecting your storage components
    * Take into account the cost, performance, latency, durability, consistency, and shelf-life or your data
      * Choose the correct storage system
        * Operational
          {:refdef: style="text-align: center;"}
          ![Badge]({{site.baseurl}}/images/aws_das_1.9.jpg)
          {:refdef}
          * Data stored as rows
          * Low latency
          * High throughput
          * Highly concurrent
          * Frequent changes
          * Benefits from caching
          * Often used in enterprise critical applications
        * Analytic
          {:refdef: style="text-align: center;"}
          ![Badge]({{site.baseurl}}/images/aws_das_1.10.jpg)
          {:refdef}
          * Two types:
            * OLAP: ad-hoc queries
            * DSS: long running aggregations
          * Data stored as columns
          * Large datasets that take advantage of partitioning
          * Frequent complex aggregations
          * Loaded in bulk or via streaming
          * Less frequent changes
    * RDS - distributed relational database service
      * Use cases
        * E-Commerce, web, mobile
      * Fast OLTP database options
        * SSD-backed storage options
      * Scale
        * Vertical scaling
        * Instance and storage size determine scale
      * Reliability and durability
        * Multi-AZ
        * Automated backups and snapshots
        * Automated failover
    * DynamoDB- fully managed NoSQL database
      * Use cases
        * Ad Tech, Gaming, Retail, Banking, and Finance
      * Fast NoSQL database options
        * Single-digit millisecond latency at scale
      * Scale
        * Horizontal scaling
        * Can store data without bounds
        * High performance and low cost even at extreme scale
      * Reliability and durability
        * Data replicated across three AZs
        * Global-tables for multi region replication
    * Elasticache - fully managed Redis and Memcahced
      * Use cases
        * Caching, session stores, gaming, real-time analytics
      * Sub-millisecond response time from in-memory data store
        * Single digit millisecond latency at scale
      * Reliability and durability
        * Redis Elasticache offers Multi-AZ with automatic failover
    * Timestream - fully managed time series database
      * Use cases
        * IoT applications, industrial telemetry, application monitoring
      * Fast: analyze trillions of events per day
        * One tenth the cost of relational database
      * Scale
        * Vertical scaling
        * Timestream scales up or down depending on your load
      * Reliability and durability
        * Managed service takes care of provisioning, patching, etc.
        * Retention policies to manage reliability and durability
    * Redshift - Cloud Data Warehouse
      * Use cases
        * Data science queries, marketing analysis
      * Fast: columnar storage technology that parallelize queries
        * Milisecond latency queries
      * Reliability and durability
        * Data replicated within the Redshift cluster
        * Continuous backup to S3
    * S3
      * Use cases:
        * Data lake, analytics, data archiving, static websites
      * Fast: query structured and structured data
        * Use Athena and Redshift Spectrum to query at low latency
      * Reliability and Durability
        * Data replicated across three AZs in a region
        * Same-region or cross-region replication
  * Data Freshness
    * Considering your data freshness when selecting your storage system components
      * Place hot data in cache (Elasticache or DAX) or NoSQL (DynamoDB)
      * Place warm data in SQL data stores (RDS)
      * Can use S3 for all types (hot, warm, cold)
      * Use S3 Glacier for cold data
  * Operational Characteristics of DynamoDB
    * DynamoDB Tables
      * Attributes - Columns
      * Items - Rows
      * Must have a primary key, two types
        * Partition Key: primary key with one attribute called the hash attribute
          * DynamoDB hash function determines the partition where an item is located
        * Composite Key: partition key plus sort key where all items with the same sort key are located together ordered by sort key value
      * No limit to number of items in a table
      * Maximum item size is 400KB
    * DynamoDB Consistency
      * DynamoDB has eventual and strong consistency
        * Eventually consistent reads (default)
          * Achieves maximum read throughput
          * May return stale data
        * Strongly consistent reads
          * Returns result representing all prior write operations
          * Always accurate data returned (no stale data)
          * Increased latency with this option
        {:refdef: style="text-align: center;"}
        ![Badge]({{site.baseurl}}/images/aws_das_1.11.jpg)
        {:refdef}
    * DynamoDB Capacity
      * Cost versus performance - two capacity modes
        * On-demand capacity
          * DynamoDB automatically provisions capacity based on your workload, up or down
        * Provisioned capacity
          * Specify read capacity units (RCUs) and write capacity units (WCUs) per second for your application
          * One RCU equals one strongly consistent read or two eventaully consistent reads per second for items up to 4KB
          * One WCU equals one write per second for items up to 1KB
          * Can use auto scaling to automatically calibrate your table's capacity
    * DynamoDB Global Tables
      * Specify multiple regions in which your table(replica) is available
      * DynamoDB propagates all changes accross all regions
      * Any change to an item in any replica is propagated to all other replicas
      * New items propagated within seconds
      * User last-writer-wins reconciliation
      * When a table in a region has issues, application directs to a different region
  * Redshift
    * Redshift Overview
      * Enterprise-class data warehouse and relational database query and management system
      * Connect using many types of client applications
        * Business Intelligence
        * Reporting
        * Analytics
      * Build multi-stage query operations that retrieve, compare and evaluate large amounts of data
      * Efficient storage and optimum query performance
        * Massively parallel processing
        * Columnar data storage
        * Very efficient, targeted data compression encoding schemes
    * Redshift Architecture
      * Based on PostgreSQL
      * Clients connect via JDBC and ODBC
      * Built upon clusters
        * One or more compute nodes
        * If greater than 1 compute nodes, a leader node coordinates the compute nodes and communicates with external client apps
      * Leader node
        * Builds execution plans to execute database operations - complex queries
        * Complies code and distributes it to the compute nodes, also assigns a portion of data to each compute node
      * Compute nodes
        * Executes the compiled code and sends intermediate results back to the leader node for final aggregation
        * Has dedicated CPU, memory and attached disk storage, which are determined by the node type
          * Node types
            * RA3
            * DC2
            * DS2
        * User data stored on compute nodes
      * Node Slices
        * Compute nodes are partitioned into slices
        * Slices are allocated a portion of the node's memory and disk space
        * Processes a part of the workload assigned to the node
        * Leader node distributes data to the slices, divides query workload to the slices
        * Slices work in parallel to complete your queries
        * Assign a column as a distribution key when you create your table on Redshift
        * When you load data into your table, rows are distributed to the node slices by the table distribution key - facilitates parallel processing
      * Columnar Storage
        * Drastically reduces the overall disk I/O requirements and reduces the amount of data you need to load from disk
          * In relational databases, data blocks store values sequentially for each consecutive column making up the entire row
          * In a columnar databases, each data block stores values of a single column for multiple rows
      * Moving data in Redshift
        * Redshift integrates well with AWS services to move, transform, and load your data quickly and reliably
          * S3
            * Leverages parallel processing to export data from Redshift to S3
          * DynamoDB
            * Use COPY command to move tables
          * SSH / Remote Host
            * Execute COPY command to load data from remote hosts such as EC2 and EMR
          * Data Pipeline
            * You can automate data movement and transformation into and out of Redshift using the scheduling capabilities
          * DMS
            * Move data back and forth between Redshift and other relational databases
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