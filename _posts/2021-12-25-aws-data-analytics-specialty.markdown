---
layout: post
title:  Study Guide for AWS Data Analytics Specialty Certification
description:
date:   10 January 2022
image:  '/images/aws-das.jpg'
tags:   [Cloud Computing, AWS, Study Guide]
---

This study guide covers AWS Certification for Data Analytics Specialty. This exam replaces the former AWS Big Data Certification, but otherwise covers the same topics. The exam consists of 65 questions, and you have 180 minutes to write it. The study guide below covers everything you need to know for it. To study for this exam I did the following:
 - Watched the official *AWS Digital Training* course
 - Read the *AWS Data Analytics Specialty Exam Guide* accessible [here](https://d1.awsstatic.com/training-and-certification/docs-data-analytics-specialty/AWS-Certified-Data-Analytics-Specialty_Exam-Guide.pdf)
 - Read the *AWS Certified Data Analytics Study Guide* by Sybex
 - Watched the *AWS Certified Data Analytics Specialty 2022 - Hands On!* Udemy course by Stéphane Maarek
 - Watched the *AWS Certified Data Analytics - Specialty* Whizlabs Course
 - Read the following FAQ papers linked below:
   - [Redshift FAQ](https://aws.amazon.com/redshift/faqs/)
   - [EMR FAQ](https://aws.amazon.com/emr/faqs/)
   - [Athena FAQ](https://aws.amazon.com/athena/faqs/)
   - [CloudSearch FAQ](https://aws.amazon.com/cloudsearch/faqs/)
   - [Kinesis Video Streams FAQ](https://aws.amazon.com/kinesis/video-streams/faqs/)
   - [Kinesis Data Streams FAQ](https://aws.amazon.com/kinesis/data-streams/faqs/)
   - [Kinesis Data Firehose FAQ](https://aws.amazon.com/kinesis/data-firehose/faqs/)
   - [Kinesis Data Analytics FAQ](https://aws.amazon.com/kinesis/data-analytics/faqs/)
   - [Opensearch FAQ](https://aws.amazon.com/kinesis/data-analytics/faqs/)
   - [Managed Kafka FAQ](https://aws.amazon.com/msk/faqs/)
   - [Quicksight FAQ](https://aws.amazon.com/quicksight/resources/faqs/)
   - [Data Exchange FAQ](https://aws.amazon.com/data-exchange/faqs/)
   - [Glue FAQ](https://aws.amazon.com/glue/faqs/)
   - [Lake Formation FAQ](https://aws.amazon.com/lake-formation/faqs/)
   - [Data Pipeline FAQ](https://aws.amazon.com/datapipeline/faqs/)
 - Completed the official AWS practice test that I received after passing the AWS Certified Cloud Practitioner exam

According to Amazon Web Services, this exam will test the following services and features

 - **Analytics**:
   - Amazon Athena 
   - Amazon CloudSearch 
   - Amazon Opensearch Service (Amazon ES)
   - Amazon EMR 
   - AWS Glue 
   - Amazon Kinesis (excluding Kinesis Video Streams)
   - AWS Lake Formation 
   - Amazon Managed Streaming for Apache Kafka 
   - Amazon QuickSight 
   - Amazon Redshift
 - **Application Integration**:
   - Amazon MQ
   - Amazon Simple Notification Service (Amazon SNS)
   - Amazon Simple Queue Service (Amazon SQS)
   - AWS Step Functions 
 - **Compute**:
   - Amazon EC2 
   - Elastic Load Balancing 
   - AWS Lambda
 - **Customer Engagement**:
   - Amazon Simple Email Service (Amazon SES)
 - **Database**:
   - Amazon DocumentDB (with MongoDB compatibility)
   - Amazon DynamoDB 
   - Amazon ElastiCache 
   - Amazon Neptune 
   - Amazon RDS 
   - Amazon Redshift 
   - Amazon Timestream 
 - **Management and Governance**:
   - AWS Auto Scaling 
   - AWS CloudFormation 
   - AWS CloudTrail 
   - Amazon CloudWatch 
   - AWS Trusted Advisor 
 - **Machine Learning**:
   - Amazon SageMaker 
 - **Migration and Transfer**:
   - AWS Database Migration Service (AWS DMS)
   - AWS DataSync 
   - AWS Snowball 
   - AWS Transfer for SFTP 
 - **Networking and Content Delivery**:
   - Amazon API Gateway 
   - AWS Direct Connect 
   - Amazon VPC (and associated features)
 - **Security, Identity, and Compliance**:
   - AWS AppSync 
   - AWS Artifact
   - AWS Certificate Manager (ACM)
   - AWS CloudHSM 
   - Amazon Cognito 
   - AWS Identity and Access Management (IAM)
   - AWS Key Management Service (AWS KMS)
   - Amazon Macie 
   - AWS Secrets Manager 
   - AWS Single Sign-On 
 - **Storage**:
   - Amazon Elastic Block Store (Amazon EBS)
   - Amazon S3 
   - Amazon S3 Glacier

---

# _Study Guide_

## Collection
- ### Determine the operational characteristics of the collection system
  - ### _Fault Tolerance and Data Persistence_
    - The Kinesis Producer library can send a group of multiple records in each request to your shards
      - If a record fails, it's put back into the KPL buffer for a retry (Fault Tolerance)
      - One record's failure doesn't fail a whole set of records
      - The KPL also has rate limiting
        - Limits per-shard throughput sent from a single producer, can help prevent excessive retries 
        {:refdef: style="text-align: center;"}
        ![Image]({{site.baseurl}}/images/kpl.jpg)
        {: refdef}
        
  - ### _Availability and Durablity of your Ingestion Components_
    - Kinesis Data Streams replicates your data synchronously across three AZs in one region
    - Streams are divided in ordered Shards/Partitions
      - One stream is made of many shards
      - The number of shards can evolve over time (reshard/merge)
      - Records are ordered per shard
    - Multiple applications can consume the same stream, ability to reprocess/replay data
    - Don't use Kinesis Data Streams for protracted data persistence
      - Your data is retained for 24 hours, which can be extended to 365 days
    - Kinesis Data Firehose streams your data directly to a data destination, no retention
       {:refdef: style="text-align: center;"}
       ![Image]({{site.baseurl}}/images/aws_das_1.1.jpg)
       {: refdef}
      - Destinations: S3, Redshift, Opensearch, Splunk and Kinesis Data Analytics
      - Can transform your data, using a Lambda function, prior to delivering the data
      
  - ### _Fault Tolerance of your Ingestion Components_
    - The Kinesis Consumer Library processes your data from your Kinesis Data Stream
      - Uses checkpointing using DynamoDB to track which records have been read from a shard
        - If a KCL read fails, the KCL uses the checkpoint cursos to resume at the failed record
    - **Important facts**
      - Use unique names for your application in the KCL, since DynamoDB tables use names
      - Watch out for provisioning throughput exception in DynamoDB: Too many shards or frequent checkpoint
    - **Alternatives to the KPL**
      {:refdef: style="text-align: center;"}
      ![Image]({{site.baseurl}}/images/aws_das_1.2.jpg)
      {: refdef}
      - Use the Kinesis API instead of KPL when you need the fastest processing time
        - KPL uses RecordMaxBufferedTime to delay processing to accommodate aggregation
      - **Kinesis Agent**
        - Kinesis Agent installs on your EC2 instance
        - Monitors files, such as log files, and streams new data to your Kinesis stream
        - Emits CloudWatch metrics to help with monitoring and error handling
        
  - ### _Summary - Determine the operational characteristics of the collection system_
    - **Two key concepts to remember for the exam**
      - Fault tolerance
      - Data persistence
    - **Kinesis Data Streams vs. Kinesis Data Firehose**
      - Data persistence is the key difference
      - Kinesis Data Streams
        - Going to write custom code (producer/consumer)
        - Real time(~200 MS latency for classic, ~70 MS latency for enhanced fan-out)
        - Must manage scaling (shard splitting/merging)
        - Data Storage up to 365 days
        - Use with Lambda to insert data into Opensearch
      - Kinesis Data Firehose
        - Fully managed
        - Serverless data transformation with Lambda
        - Near real time (lowest buffer time is 1 minute)
        - Automated Scaling
        - No Data storage
    - **Kinesis Producer Library vs. Kinesis API vs. Kinesis Agent**
      - Fault tolerance and appropriate tools for your data collection problem
    - **Kinesis Data Streams vs SQS**
      - Kinesis Data Streams
        - Data can be consumed many times
        - Data is deleted after the retention period
        - Ordering of records is preserved (at the shard level) - even during replays
        - Build multiple applications reading from the same stream independently
        - Checkpointing to track progress of consumption
        - Shards(capacity) must be provided ahead of time
      - SQS
        - Queue, decouple applications
        - One application per queue
        - Records are deleted after consumptions
        - Messages are processed interdependently for standard queue
        - Ordering for FIFO queues
        - Capability to 'delay' messages
        - Dynamic scaling of load
    - **Kinesis Data Streams vs MSK**
      - Kinesis Data Streams
        - 1 MB message size limit
        - Data Streams with Shards
        - Shard Splitting and Merging
        - TLS in-flight encryption 
        - KMS at-rest encryption
        - Security
          - IAM policies for AuthN/AuthZ
      - MSK
        - 1 MB default, configure for higher
        - Kafka Topics with Partitions
        - Can only add partitions to a topic
        - plaintext or TLS in-flight encryption
        - KMS at-rest encryption
        - Security
          - MutualTLS(AuthN) + Kafka ACLs(AuthZ)
          - SASL/SCRAM(AuthN) + Kafka ACLs(AuthZ)
          - IAM Access Control (AuthN + AuthZ)
      
  - ### _Data Collection through Real-Time Streaming Data_
    - Kinesis Data Firehose is fully managed
    - Destinations: S3, Redshift, Opensearch, Splunk, Kinesis Data Analytics
    - Can optionally transform data, using Lambda, before delivering it to its destination
      {:refdef: style="text-align: center;"}
      ![Image]({{site.baseurl}}/images/aws_das_1.3.jpg)
      {: refdef}
    - **Firehose to Redshift**
      - Delivers directly to S3 first
      - Firehose then runs a Redshift COPY command
      - Can optionally transform your data, using Lambda, before delivering it to its destination
    - **Firehose to Opensearch Cluster**
      - Firehose delivers directly to Opensearch cluster
      - Can optionally backup to S3 concurrently
    - **Firehose to Splunk**
      - Firehose delivers directly to Splunk instance
      - Can optionally backup to S3 concurrently
    - **Firehose Producers**
      - Firehose producers send records to Firehose
        - Web server logs data
        - Kinesis Data Stream
        - Kinesis Agent
        - Kinesis Firehose API using the AWS SDK
        - CloudWatch logs and/or events
        - AWS IoT
        - Firehose buffers incoming streaming data for a set buffer size (MBs) and a buffer interval (seconds). You can manipulate the buffer size in the buffer interval to speed up or slow down your firehose delivery speed
          - use case: you're delivering streaming data from Firehose to an S3 bucket, how might you speed up the delivery of your Kinesis Data? Lower the buffer size and lower the buffer interval
          
- ### Select a collection system that handles the frequency, volume, and the source of data
  - ### _The Four Ingestion Services_
    - **Kinesis Data Streams**
      - Use cases needing custom processing and different stream processing frameworks where sub-second processing latency is needed
    - **Kinesis Data Firehose**
      - Use cases needing managed service streaming to S3, Redshift, Opensearch, or Splunk where data latency of 60 seconds or higher is acceptable
    - **AWS Database Migration Service**
      - Use cases needing one-time migration and/or continuous replication of database records and structures to AWS services
    - **AWS Glue**
      - Use cases needing ETL batch-oriented jobs where scheduling of ETL jobs is required
      
  - ### _Kinesis Data Streams_
    - **Each shard supports**
      - 1,000 RPS for writes with max of 1 MB/sec
      - 5 TPS for reads at a max of 2MB/sec using GetRecords API Call
      - Stream total capacity equals the sum of the capacity of the shards
      - No limit to the number of shards you can provision
    - **Data Blob**
      - data being sent, serialized as bytes
    - **Record Key** 
      - Sent alongside a record, helps to group records in shards. Same key = same shard
      - Use highly distributed key to avoid the "hot partition"
    - **Sequence number**
      - Unique identifier for each record put in shards. Added by Kinesis after ingestion
    - **Producer**
      - *Kinesis Producer SDK - PutRecords(s)*
        - APIs that are used are `PutRecord` and `PutRecords`
        - `PutRecords` uses batching and increases throughput => less HTTP requests
        - `ProvisionedThroughputExceeded` if we go over the limit
          - Happens when sending more data(exceeding MB/sec or TPS for any shard)
          - Make sure you don't have a hot shard(such as your partition key is bad and too much data goes into that partition)
          - Solution
            - Retries with backoff (2,4,8 seconds)
            - Increase shards(scaling)
            - Ensure your partition key is a good one
        - Use case: low throughput, high latency, simple API, AWS Lambda
        - Anti-Pattern: applications that cannot tolerate `RecordMaxBufferedTime` delay, therefore use the SDK directly
        - We can influence the batching efficiency by introducing some delay with `RecordMaxBufferedTime` (default 100ms)
        - Managed AWS sources for KDS that use the SDK behind the scenes
          - CloudWatch Logs
          - AWS IoT
          - Kinesis Data Analytics
      - *Kinesis Producer Library (KPL)*
        - Easy to use and highly configurable C++/Java library
        - Used for building high performance, long-running producers
        - automated and configurable `retry` mechanism
        - Synchronous or Asynchronous API (better performance for async)
        - Batching: increases throughput and decreases cost
          - Collect
            - Records and writes to multiple shards in the same PutRecords API call
          - Aggregate
            - Increased latency
            - Capability to store multiple records in one record (go over 1000 records/sec limit)
            - Increase payload size and improve throughput (maximize 1MB/sec limit)
        - Compression must be implemented by user
      - *Kinesis Agent*
        - Monitor Log files and sends them to KDS
        - Install only in Linux based environments
        - Features
          - Write to multiple directories and write to multiple streams
          - Routing feature based on directory/log file
          - Pre-process data before sending to streams
          - Handles file rotation, checking, and retry upon failures
          - Emits metrics to CloudWatch for monitoring
    - **Consumer**
        - *Kinesis Consumer SDK - GetRecords*
          - Classic Kinesis: Records are polled by consumers from a shard
          - Each shard has 2MB total aggregate throughput
          - `GetRecords` returns up to 10MB of data (then throttle for 5 seconds) or up to 1000 records
          - Maximum of 5 `GetRecords` API calls per shard per second = 200ms latency
          - If 5 consumer applications consume from the same shard, means every consumer can poll once a second and receive less than 400 KB/sec
        - *Kinesis Client Library (KCL)*
          - Read records from Kinesis produced with the KPL (de-aggregation)
          - Share multiple shards with multiple consumers in one 'Group', shard discovery
          - Checkingpoint feature to resume progress
          - Leverages DynamoDB for coordination and checkpointing (one row per shard)
            - Make sure you provision enough WCU/RCU
            - Or use On-Demand for DynamoDB
            - Otherwise DynamoDB may slow down KCL
          - Record processors will process the data
          - `ExpiredIteratorException` > increase WCU
        - *Kinesis Connector Library*
          - Older java library (2016), leverages the KCL library
          - Must be running on an EC2 instance
          - Deprecated: Kinesis Firehose/Lambda replaces this
        - *Lambda*
          - Lambda can source records from Kinesis Data Streams
          - Has a library to de-aggregate records from the KPL
          - Lambda can be used to run lightweight ETL
          - Can be used to trigger notifications/send emails in real time
          - Configurable batch size
    - **Kinesis Enhanced Fan Out**
      - Each consumer get 2 MB/sec of provisioned throughput per shard
      - That means that if we have 20 consumers, overall we'll get 40 megabytes per second, per shard.
      - Before we had a 2 MB/sec limit per shard, but now, we get 2 MB/sec per limit per shard per consumer
      - Pushes data to consumers over HTTP/2
      - Reduced latency (~70 MS)
      - Costs a bit more
      - *Enhanced Fan-Out vs. Standard Consumers Use Cases*
        - Standard
          - Low number of consuming applications (1,2,3...)
          - Can tolerate ~200 MS latency
          - Minimize Cost
        - Enhanced Fan Out Consumers
          - Multiple Consumer applications for the same Stream
          - Low Latency requirements ~70 MS
          - Higher costs
          - Default limit of 5 consumers using enhanced fan-out per data stream
    - **Scaling**
      - *Adding Shards*
        - Also called 'Shard Splitting'
        - Can be used to increase the Stream capacity (1 MB/sec per shard)
        - Can be used to divide a 'hot shard'
        - The old shard is closed and will be deleted once the data is expired
      - *Merging Shards*
        - Decrease the Stream capacity and save costs
        - Can be used to group two shards with low traffic
        - Old shards are closed and deleted based on data expiration
      - *Out of order records after resharding*
        - After a reshard, you can read from child shards
        - However, data you haven't read yet could still be in the parent
        - If you start reading the child before completing reading the parent, you could read data for a particular hash key out of order
        - After a reshard, read entirely from the parent until you don't have new records
        - The Kinesis Client Library has this logic already built-in, even after resharding operations
      - *Auto Scaling*
        - Auto Scaling is not a native feature of Kinesis
        - Can be implemented with Lambda
      - *Handling Duplicate Records*
        - Producer side
          - Producer retries can create duplicates due to network timeouts
          - Although the two records have identical data, they also have unique sequence numbers
          - Fix: Embed unique record ID in the data to de-duplicate on the consumer side
        - Consumer side
          - Consumer retries can make your application read the same data twice
          - Consumer retries happen when record processors restart:
            - A worker terminates unexpectedly
            - Worker instances are added or removed
            - Shards are merged or split
            - The Application is deployed
          - Fix: Make your consumer application idemptotent
          - If the final destination can handle duplicate, it's recommended to do it there
    - **Limits**
      - Producer
        - 1 MB/sec or 1000 messages/sec at write PER SHARD
        - `ProvisionedThroughputException` otherwise
      - Consumer Classic
        - 2 MB/sec at read PER SHARD across all consumers
        - 5 API calls per second PER SHARD across all consumers
      - Consumer Enhanced Fan-Out
        - 2 MB/sec at read PER SHARD, PER ENHANCED CONSUMER
        - No API calls needed (push model)
      - Scaling
        - Resharding cannot be done in parallel; so that means you can't reshard a thousand streams at a time or a thousand shards at a time
        - Takes a few seconds: For 1000 shards, it takes 30,000 seconds to double the shards to 2000

      {:refdef: style="text-align: center;"}
      ![Image]({{site.baseurl}}/images/aws_das_1.4.jpg)
      {:refdef}
    
  - ### _Kinesis Data Firehose_
    - Firehose automatically delivers to specified destination, near real time service (60 second latency for non-full batches)
    - Can deliver to any HTTP Endpoint
    - Fully managed service, has automatic scaling
    - Support data conversion from JSON to Parquet/ORC
    - Data transformation through AWS Lambda
    - Supports compression when target is S3 (GZIP, ZIP, and SNAPPY)
    - Only GZIP is supported by Redshift
    - Pay for the amount of data going through Firehose
    - Spark/KCL do not read from Data Firehose
    - You can deposit the source data directly into S3
    - Buffers incoming streaming data to specified size or for a specified period of time before delivering to destinations
      - Firehose accumulates reocrds in a buffer
      - The buffer is flushed based on time and size rules
      - Buffer Size (Ex. 32 MB): If that buffer size is reached, it's flushed
      - Buffer Time (Ex. 2 Minutes): If that time is reached, it's flushed
      - Firehose can automatically increase the buffer size to increase throughput
      - High Throughput > Buffer size will be hit
      - Low Throughput > Buffer time will be hit
    - Automatically scales to match data throughput
      - No manual intervention or developer overhead required
      {:refdef: style="text-align: center;"}
      ![Image]({{site.baseurl}}/images/aws_das_1.5.jpg)
      {:refdef}
  - ### _Data Migration Service_
    - DMS is used to take data from a source database to a destination target (database, data lake, data warehouse)
    - Source database remains fully operational during the migration
    - Supports several data engines as a source and target for data replication
    - Uses tasks to capture ongoing changes after initial migration to a supported target data store
      - Ongoing replication or Change Data Capture
    - Uses database engine's APIs to read changes from transaction log then replicate to target database
    - EC2 replication instance, scale to meet utilization requirements
    - Schema Conversion Tool allows you to convert your database's schema from one engine to another
      {:refdef: style="text-align: center;"}
      ![Image]({{site.baseurl}}/images/aws_das_1.6.jpg)
      {:refdef}

  - ### _AWS Glue_
    - Key Point: Batch oriented (although now it supports Streaming)
      - Micro-batches but no streaming data
    - Does not support NoSQL databases as data source
    - Crawl data source to populate data catalog
    - Generate a script to transform data or write your own in console or API
    - Run jobs on demand or via a trigger event
    - Glue catalog tables contain metadata not data from the data source
    - Uses a scale-out Apache Spark environment when loading data to destination
      - Allocate data processing units (DPUs) to ETL jobs
    - Glue crawler scans data in S3, creates schema and populates the Glue Data Catalogue
      - Glue crawler will extract partitions based on how your S3 data in organized
    - Glue can integrate with most SQL databases, regardless of whether they are AWS services.
    - Running Glue Jobs
      - Job bookmarks
        - Persists state from the job run
        - Prevents reprocessing of old data
        - Allows you to process new data only when re-running on a schedule
      - CloudWatch Events
        - Fire off a Lambda function or SNS notification when ETL succeeds or fails
        - Invoke EC2 run, send event to Kinesis, activate a Step Function
    - Streaming
      - Glue ETL supports serverless streaming ETL
        - Consumes from Kinesis of Kafka
        - Clean and transform in-flight
        - Store results into S3 or other data stores
        - Runs on Spark Structured Streaming
    - Glue DataBrew
      - Visual data preparation tool
      - UI for pre-processing large data sets
      - Input from S3, data warehouse, or database
      - Output to S3
      - You can create 'recipes' of transformations that can be saved as jobs
    {:refdef: style="text-align: center;"}
    ![Image]({{site.baseurl}}/images/aws_das_1.7.jpg)
    {:refdef}

  - ### _AWS SQS_
    - Summary
      - SQS will send a message to the SQS Queue and a consumer, or consumers, will pull messages from that Queue. 
      - fully managed and it will scale automatically. 
      - Even if you have 1 message per second to 10,000 messages per second there are no limit to how many messages per seconds you can send to SQS 
      - The default retention period of a message is 4 days but, you can have a maximum of 14 days 
      - There's no limit to how many messages can be in the queue
      - There has extremely low latency
      - There is horizontal scaling in terms of the number of consumers who can scale as many consumers as you want
      - Limit of 256 KB per message
    - Producing Messages
      - Define Body
      - Add message attributes
      - Provide Delay Delivery
    - Consuming Messages
      - Poll SQS for messages (up to 10 at a time)
      - Delete the message using the message ID and receipt handle
      - Cannot be handled by different consumers like Kinesis
    - FIFO Queue
      - Lower throughput (up to 3,000 messages per second with batching, 300 messages per second without)
      - Messages are processed in order by the consumer
      - Messages are sent exactly once
      - Trade off: less throughput but exact ordering
    - Use Cases
      - Decouple Applications
      - Buffer writes to a database
      - Handle large loads of messages coming in
      - SQS can be integrated with Auto Scaling through CloudWatch
    - Limits
      - Maximum 120,000 in flight messages processed by consumers
      - Batch Request has a maximum of 10 messages - Max 256KB
      
  - ### _AWS MSK_
    - Alternative to Kinesis
    - Fully managed Apache Kafka on AWS
      - Allows you to create, update, delete clusters
      - MSK creates and manages Kafka broker nodes, Zookeeper nodes for your
      - Deploy the MSK cluster in your VPC, multi-AZ (up to 3 for HA)
      - Automatic recovery from common Apache Kafka failures
      - Data is stored on EBS volumes
    - You can build producers and consumers of data
    - Can create custom configurations for your clusters
      - Default message size of 1 MB
      - Possibilities of sending large messages into Kafka after customer configuration
    - Security
      - Encryption
        - Optional in-flight using TLS between the brokers
        - Optional in-flight with TLS between the clients and brokers
        - At rest encryption for your EBS volumes using KMS
    - Monitoring
      - CloudWatch Metrics
        - Basic monitoring

  - ### _The Three Types of Data to Ingest_
    - **Batch Data**
      - Applications logs, video files, audio files, etc.
      - Larger event payloads ingested in an hourly, daily, or weekly daily
      - Ingested in intervals from aggregated data
    - **Streaming Data**
      - Click-stream data, IoT sensor data, stock ticker data, etc.
      - Ingesting large amounts of small records continuously and in real-time
    - **Transactional Data**
      - Initially load and receive continuous updates from data stores used as operational business databases
      - Similar to batch data but with a continuous update flow
      - Ingested from databases storing transactional data

  - ### _Batch Data_
    - Data is usually 'colder' and can be processed on less frequent intervals
      - Use AWS batch-oriented services like Glue
      - EMR support batch processing on a large scale
    - Latency is minutes to hours
    - Complex analytics across big data sets

  - ### _Streaming Data_
    - Often bound by time or event sets in order to produce real-time outcomes
    - Data is usually 'hot' arriving at a high frequency that you need to analyze in real-time
      - Use Kinesis Data Streams, Kinesis Data Firehose, Kinesis Data Analytics
      - Can load data into data lake or data warehouse
    - Individual records or aggregated micro-batches 
    - low-latency, i.e. milliseconds
    - Simpler analytics, rolling metrics, aggregations

  - ### _Transactional Data_
    - Data stored at low latency and quickly accessible
    - Load data from a database on-prem or in AWS
    - Use Database Migration Service
    - Can load data from relational databases, data warehouses and NoSQL databases
    - Can convert to different database systems using the AWS Schema Conversion Tool (AWS SCT) then use DMS to migrate your data

  - ### _Comparing the Data Collection Systems_
    - Understand how each ingestion approach is best used
      - Throughput, bandwidth, scalability
      - Availability and fault tolerance
      - Cost of running the services
    - Understand Differences between Services
    <div class="table-container">
      <table>
        <tr><th>Kinesis Data Streams</th><th>Kinesis Data Firehose</th><th>Data Migration Service</th><th>Glue</th></tr>
        <tr><td>Use when you need custom producers and consumers</td><td>Use cases where you want to deliver directly to S3, Redshift, Opensearch, or Splunk</td><td>Use cases when you need to migrate data from one database to another</td><td>Batch-oriented use cases where you want to perform an Extract Transform Load(ETL) process</td></tr>
        <tr><td>Use cases that require sub-second processing</td><td>Use cases where you can tolerate latency of 60 seconds or greater</td><td>Use cases where you want to migrate a database to a different database engine</td><td>Not for use with streaming use cases</td></tr>
        <tr><td>Use cases that require unlimited bandwidth</td><td>Use cases where you wish to transform your data or convert the data format</td><td></td><td></td></tr>
      </table>
    </div>
    - Throughput, Bandwidth, Scalability
    <div class="table-container">
      <table>
        <tr><th>Kinesis Data Streams</th><th>Kinesis Data Firehose</th><th>Data Migration Service</th><th>Glue</th></tr>
        <tr><td>Shards can handle up to 1,000 PUT records per second</td><td>Automatically scales to accommodate the throughput of your stream</td><td>EC2 instances used for the replication instance</td><td>Runs in a scale-out Apache Spark environment to move data to target system</td></tr>
        <tr><td>Can increase the number of shards in a stream without limit</td><td></td><td>You need to scale your replication instance to accommodate your throughput</td><td>Scales via Data Processing Units (DPU) for your ETL jobs</td></tr>
        <tr><td>Each shard has a capacity of 1 MB/s for input and 2 MB/s output</td><td></td><td></td><td></td></tr>
      </table>
    </div>
    - Availability and Fault Tolerance
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
    - Understand how each service incurs cost
    <div class="table-container">
      <table>
        <tr><th>Kinesis Data Streams</th><th>Kinesis Data Firehose</th><th>Data Migration Service</th><th>Glue</th></tr>
        <tr><td>Pay per shard hour and PUT payload unit</td><td>Pay for the volume of data ingested</td><td>Pay for the EC2 compute resources you use when migrating</td><td>Pay an hourly rate at a billing per second for both crawlers and ETL jobs</td></tr>
        <tr><td>Extended data retention and enhanced fanout incur additional cost</td><td>Pay for data conversions</td><td>Pay for log storage</td><td>Monthly fee for storing and accessing data in your Glue data catalogue</td></tr>
        <tr><td></td><td></td><td>Data transfer fees</td><td></td></tr>
      </table>
    </div>
  
  - ### Select a collection system that addresses the key properties of data, such as order, format, and compression
      - **Managing Data Order, Format and Compression**
        - Problems with your streaming data
          - Data that is out of order
          - Data that is duplicated
          - Data where we need to change the format
          - Data that needs to be compressed
        - Methods to address these problems
          - Choose an ingestion service that has guaranteed ordering
            - Kinesis Data Streams
            - DynamoDB Streams
          - Choose an ingestion service that has deduping capacity
            - DynamoDB Streams - exactly-once
            - Kinesis Data Streams - at-least-once
              - Embed a primary key in data records and remove duplicates later when processing
            - Kinesis Data Firehose - at-least-once
              - Crawl target data with Glue ML FindMatches Transform
          - Use conversion or compression feature of ingestion service
            - Kinesis Data Streams
              - Use Lambda consumer to format or compress
              - Use KCL application to format or compress
            - Kinesis Data Firehose
              - Use format conversion feature if data in JSON
              - Use Lambda transform to preprocess format conversion feature if data not JSON
              - Use S3 compression (GZIP, Snappy, or Zip)
              - Use GZIP COPY command option for Redshift compression
            {:refdef: style="text-align: center;"}
            ![Image]({{site.baseurl}}/images/aws_das_1.8.jpg)
            {:refdef}
      - **Transforming Data when Ingesting**
        - Kinesis Data Firehose
          - Using Lambda, Firehose sends buffered batches to Lambda for transformation
          - Batch, encrypt, and/or compress data
        - Lambda
          - Convert the format of your data, e.i. GZIP -> JSON
          - Transform your data, e.g. expand strings into individual columns
          - Filter your data to remove extraneous info
          - Enrich your data, e.g. error correction
        - Database Migration Service
          - Table and schema transformations, e.g. change table, schema and/or column names

## Storage and Data Management
- ### Determine the operational characteristics of the storage solution for analytics
  - ### _Selecting your storage components_
    - Take into account the cost, performance, latency, durability, consistency, and shelf-life or your data
      - Choose the correct storage system
        - Operational
          {:refdef: style="text-align: center;"}
          ![Image]({{site.baseurl}}/images/aws_das_1.9.jpg)
          {:refdef}
          - Data stored as rows
          - Low latency
          - High throughput
          - Highly concurrent
          - Frequent changes
          - Benefits from caching
          - Often used in enterprise critical applications
        - Analytic
          {:refdef: style="text-align: center;"}
          ![Image]({{site.baseurl}}/images/aws_das_1.10.jpg)
          {:refdef}
          - Two types:
            - OLAP: ad-hoc queries
            - DSS: long running aggregations
          - Data stored as columns
          - Large datasets that take advantage of partitioning
          - Frequent complex aggregations
          - Loaded in bulk or via streaming
          - Less frequent changes
  - ### _RDS - distributed relational database service_
    - Use cases
      - E-Commerce, web, mobile
      - Fast OLTP database options
        - SSD-backed storage options
      - Scale
        - Vertical scaling
        - Instance and storage size determine scale
      - Reliability and durability
        - Multi-AZ
        - Automated backups and snapshots
        - Automated failover
  - ### _DynamoDB- fully managed NoSQL database_
      - Use cases
        - Ad Tech, Gaming, Retail, Banking, and Finance
      - Fast NoSQL database options
        - Single-digit millisecond latency at scale
      - Scale
        - Horizontal scaling
        - Can store data without bounds
        - High performance and low cost even at extreme scale
      - Reliability and durability
        - Data replicated across three AZs
        - Global-tables for multi region replication
      - Basics
        - Maximum size of an item is 400 KB
      - Primary Keys
        - Option 1
          - Partition Key only (HASH)
          - Key must be unique for each item
          - Example: user_id
        - Option 2
          - The combination must be unique
          - Data is grouped by partition key
          - Sort key == range key
          - Example: user_id for the partition key, game_id for the sort key
      - Anti-patterns
        - Joins or complex transactions
        - BLOB data
        - Large data with low I/O rate
      - Provisioned Throughput
        - Table must have provisioned read and write capacity units
        - Option to set up auto-scaling of throughput to meet demand
        - Throughput can be exceeded temporarily using 'burst credit'
        - If burst credit is empty, you'll get a `ProvisionedThroughputException`
        - If we exceed our RCU or WCU, we get `ProvisionedThroughputExceededException`
          - Reason: hot key, large items
        - By default, DynamoDB uses Eventually Consistent Reads
        - Write Capacity Units
          - One WCU represents one write per second for an item up to 1 KB in size
          - Example
            - 10 objects per seconds of 2 KB each: 2 * 10 = 20 WCU
            - 6 objects per second of 4.5 KB each: 6 * 5 = 30 WCU
            - 120 objects per minute of 2 KB each: 120/60 * 2 = 4 WCU
        - Read Capacity Units
          - One read capacity unit represents one strongly consistent reads per second, or two eventually consistent reads per second, for an item up to 4 KB in size'
          - If the items are larger than 4 KB, more RCU are consumed
          - Example
            - 10 strongly consistent reads per seconds of 4 KB each: 10 * 4 / 4 = 10 RCU
            - 16 eventually consistent reads per second of 12 KB each: (16/2) * (12/4) = 24 RCU
            - 10 strongly consistent reads per second of 6 KB each: 10 * 8 / 4 = 20 RCU (we have to round up 6 KB to 8 KB)
      - Partitions
        - You start with one partition
        - Each partition has a max of 3000 RCU/1000 WCU, and 10GB
        - To compute the number of partitions
          - By Capacity: (total RCU / 3000) + (total WCU / 1000)
          - By Size: total size / 10 GB
          - Total Partitions: CEILING(MAX(Capacity, Size))
      - Writing Data
        - `PutItem` - write data to DynamoDB
        - `UpdateItem` - Update data in DynamoDB
        - `BatchWriteItem` - up to 25 `PutItem` and/or `Delete Item` in one call
        - Conditional Writes
          - Accept a write/update only if conditions are
      - Reading Data
        - `GetItem` - read based on primary key
        - `BatchGetItem` - Up to 100 items, up to 16 MB of data
      - Scan
        - Scan the entire table and then filter out data (inefficient)
      - Indexes
        - LSI (Local Secondary Index)
          - The LSI is an alternate range key or sort key for your table that is local to the hash key, so the partition key stays the same, but you have an alternate range key
          - Up to five local secondary indexes per table
          - The sort key consists of exactly one scalar attribute
          - The attribute that you choose must be a scalar String, Number or Binary
          - LSI must be defined at table creation
          {:refdef: style="text-align: center;"}
          ![Image]({{site.baseurl}}/images/aws_das_1.28.jpg)
          {:refdef}
        - GSI (Global Secondary Index)
          - To speed up queries on non-key attributes, use a Global Secondary Index
          - GSI = partition key + optional sort key
          - The index is a new 'table' and we can project attributes on it
      - DAX
        - Seamless cache for DynamoDB, no application re-write
        - Writes go through DAX to DynamoDB
        - Micro second latency for cached reads and queries
        - Solves the Hot Key problem (too many reads)
        - 5 TTL for cache by default
        - Up to 10 nodes for the cluster
      - DynamoDB Streams
        - Changes in DynamoDB(Create, Update, Delete) can end up in a DynamoDB Stream
        - This stream can be read by AWS Lambda, and we can then do:
          - React to changes in real time
          - Create tables/views
          - Insert into OpenSearch
        - Stream has 24 hours of data retention
        - Configure batch size up to 1000 rows or 6 MB of data
      - TTL
        - Automaticlaly delete an item after an expiry date/time
        - Helps reduce storage and manage the table size, as well as helps adhere to regulatory norms
      - Storing Large Objects
        - Max size of an item is 400 KB
        - For large objects, store them in S3 and reference them in DynamoDB

  - ### _Elasticache - fully managed Redis and Memecache_
      - Use cases
        - Caching, session stores, gaming, real-time analytics
      - Sub-millisecond response time from in-memory data store
        - Single digit millisecond latency at scale
      - Reliability and durability
        - Redis Elasticache offers Multi-AZ with automatic failover
      - Timestream - fully managed time series database
      - Use cases
        - IoT applications, industrial telemetry, application monitoring
      - Fast: analyze trillions of events per day
        - One tenth the cost of relational database
      - Scale
        - Vertical scaling
        - Timestream scales up or down depending on your load
      - Reliability and durability
        - Managed service takes care of provisioning, patching, etc.
        - Retention policies to manage reliability and durability
  - ### _Redshift - Cloud Data Warehouse_
      - Use cases
        - Data science queries, marketing analysis
      - Fast: columnar storage technology that parallelize queries
        - Millisecond latency queries
      - Reliability and durability
        - Data replicated within the Redshift cluster
        - Continuous backup to S3
  - ### _S3_
      - Use cases:
        - Data lake, analytics, data archiving, static websites
      - Fast: query structured and structured data
        - Use Athena and Redshift Spectrum to query at low latency
      - Reliability and Durability
        - Data replicated across three AZs in a region
        - Same-region or cross-region replication
      - Storage
        - Max size is 5TB
      - Strongly consistent
        - After a successful write of a new object, or an overwrite or delete of an existing object, any subsequent read request immediately receives the latest version of the object
      - Event Notifications
        - Events can happen in your S3 bucket, for example, a new object is created, removed, restored, or replicated
        - You want to be able to react to all these events. You can create rules which you can use to filter by object names
  - ### _Data Freshness_
    - Considering your data freshness when selecting your storage system components
      - Place hot data in cache (Elasticache or DAX) or NoSQL (DynamoDB)
      - Place warm data in SQL data stores (RDS)
      - Can use S3 for all types (hot, warm, cold)
      - Use S3 Glacier for cold data
  - ### _Operational Characteristics of DynamoDB_
    - DynamoDB Tables
      - Attributes - Columns
      - Items - Rows
      - Must have a primary key, two types
        - Partition Key: primary key with one attribute called the hash attribute
          - DynamoDB hash function determines the partition where an item is located
        - Composite Key: partition key plus sort key where all items with the same sort key are located together ordered by sort key value
      - No limit to number of items in a table
      - Maximum item size is 400KB
    - DynamoDB Consistency
      - DynamoDB has eventual and strong consistency
        - Eventually consistent reads (default)
          - Achieves maximum read throughput
          - May return stale data
        - Strongly consistent reads
          - Returns result representing all prior write operations
          - Always accurate data returned (no stale data)
          - Increased latency with this option
        {:refdef: style="text-align: center;"}
        ![Image]({{site.baseurl}}/images/aws_das_1.11.jpg)
        {:refdef}
    - DynamoDB Capacity
      - Cost versus performance - two capacity modes
        - On-demand capacity
          - DynamoDB automatically provisions capacity based on your workload, up or down
        - Provisioned capacity
          - Specify read capacity units (RCUs) and write capacity units (WCUs) per second for your application
          - One RCU equals one strongly consistent read or two eventually consistent reads per second for items up to 4KB
          - One WCU equals one write per second for items up to 1KB
          - Can use auto scaling to automatically calibrate your table's capacity
    - DynamoDB Global Tables
      - Specify multiple regions in which your table(replica) is available
      - DynamoDB propagates all changes across all regions
      - Any change to an item in any replica is propagated to all other replicas
      - New items propagated within seconds
      - User last-writer-wins reconciliation
      - When a table in a region has issues, application directs to a different region
  - ### _Redshift_
    - Redshift Overview
      - Enterprise-class data warehouse and relational database query and management system
      - Connect using many types of client applications
        - Business Intelligence
        - Reporting
        - Analytics
      - Build multi-stage query operations that retrieve, compare and evaluate large amounts of data
      - Efficient storage and optimum query performance
        - Massively parallel processing
        - Columnar data storage
        - Very efficient, targeted data compression encoding schemes
    - Redshift Architecture
      - Based on PostgreSQL
      - Clients connect via JDBC and ODBC
      - Built upon clusters
        - One or more compute nodes
        - If greater than 1 compute nodes, a leader node coordinates the compute nodes and communicates with external client apps
      - Leader node
        - Builds execution plans to execute database operations - complex queries
        - Complies code and distributes it to the compute nodes, also assigns a portion of data to each compute node
      - Compute nodes
        - Executes the compiled code and sends intermediate results back to the leader node for final aggregation
        - Has dedicated CPU, memory and attached disk storage, which are determined by the node type
          - Node types
            - RA3
            - DC2
            - DS2
        - User data stored on compute nodes
      - Node Slices
        - Compute nodes are partitioned into slices
        - Slices are allocated a portion of the node's memory and disk space
        - Processes a part of the workload assigned to the node
        - Leader node distributes data to the slices, divides query workload to the slices
        - Slices work in parallel to complete your queries
        - Assign a column as a distribution key when you create your table on Redshift
        - When you load data into your table, rows are distributed to the node slices by the table distribution key - facilitates parallel processing
      - Columnar Storage
        - Drastically reduces the overall disk I/O requirements and reduces the amount of data you need to load from disk
          - In relational databases, data blocks store values sequentially for each consecutive column making up the entire row
          - In a columnar databases, each data block stores values of a single column for multiple rows
      - Moving data in Redshift
        - Redshift integrates well with AWS services to move, transform, and load your data quickly and reliably
          - S3
            - Leverages parallel processing to export data from Redshift to S3
          - DynamoDB
            - Use COPY command to move tables
          - SSH / Remote Host
            - Execute COPY command to load data from remote hosts such as EC2 and EMR
          - Data Pipeline
            - You can automate data movement and transformation into and out of Redshift using the scheduling capabilities
          - DMS
            - Move data back and forth between Redshift and other relational databases
  - ### _Snow family_
    - Snowcone
      - Small portable computing, anywhere, rugged and secure
      - Device used for edge computing, storage, and data transfer
      - 8 TB of usable storage
      - Can be used offline
    - Snowball Edge
      - Move TB or PB of data in or out of AWS
      - Snowball Edge Storage Optimized
        - 80 TB of HDD Capacity
      - Snowball Edge Compute Optimized
        - 42 TB of HDD Capacity
      - Use Cases: Data cloud migrations, DC Decommission, Disaster Recovery
    - Snowmobile
      - Transfer of exabytes of data
      - High Security
      - Better than snowball if you transfer more than 10 PB of data
    
- ### Determine data access and retrieval patterns
  - ### _Data Access and Retrieval Patterns_
    - Characteristics of your data
      - What type of data are you storing?
    - Data storage life cycle
      - How long do you need to retain your data?
    - Data access retrieval and latency requirements
      - How fast does your retrieval need to be?
  - ### _Characteristics of your data_
    - Structured data is consists of clearly defined data types with patterns that make them easily searchable and is stored in a predefined format, where unstructured data is a conglomeration of many varied types of data that are stored in their native formats. This means that structured data takes advantage of schema-on-write and unstructured data employs schema-on-read.
    - Structured data
      - Examples: accounting data, demographic info, logs, mobile device geolocation data
      - Storage options: RDS, Redshift
    - Unstructured data
      - Examples: email text, photos, video audio, pdfs
      - Storage options: S3 Data Lake, DynamoDB
    - Semi Structured data
      - Examples: email metadata, digital photo metadata, video metadata, JSON data
      - Storage options: S3 Data Lake, DynamoDB
  - ### _Data Lake or Data Warehouse_
    - Data Warehouse
      - Optimized for relational data produced by transactional systems
      - Data structure, schema predefined which optimizes fast SQL queries
      - Used for operational reporting and analysis
      - Data is transformed before loading
    - Data Lake
      - Relational data and non-relational data: mobile apps, IoT devices, and social media
      - Data structure/schema not defined when stored in the data lake
      - Big data analytics, text analysis, ML
      - Schema on read
  - ### _Object vs. Block Store_
    - Object storage
      - S3 is used for object storage: highly scalable and available
      - Store structured, unstructured and semi structured data
      - Websites, mobile apps, archive, analytics applications
      - Storage via a web service
    - File storage
      - EFS is used for file storage: shared file systems
      - Content repositories, development environments, media stores, user home directories
    - Block storage
      - EBS attached to EC2 instances, EFS: volume type choices
      - Redshift, Operating Systems, DBMS installs, file systems
      - HDD: throughput intensive, large I/O, sequential I/O, big data
      - SSD: high I/O per second, transaction, random access, boot volumes
  - ### _Data Storage Lifecycle_
    - Persistent data
      - OLTP and OLAP
      - DynamoDB, RDS, Redshift
    - Transient data
      - Elasticache, DAX
      - Website session info, streaming gaming data
    - Archive data
      - Retained for years, typically regulatory
      - S3 Glacier (can combine with Vault Lock which allows you to easily deploy and enforce compliance controls for individual S3 Glacier vaults with a vault lock policy)
  - ### _Data Access Retrieval and Latency_
    - Retrieval speed
      - Near-real time
        - Streaming data with near-real time dashboard display
      - Cached data
        - Elasticache
        - DAX
          - Uses write-through cache
  - ### _Different Approaches to Data Management_
    - Data Lake
      - Store any data
      - Store raw data
      - Use for analytics - dashboards, visualizations, big data, real-time analytics, machine learning
    - Data Warehouse
      - Centralized data repository for BI and analysis
      - Access the centralized data using BI tools, SQL clients, and other analytics apps
    - Data Optimization
    <div class="table-container">
      <table>
        <tr><th>Data Lake</th><th></th><th>Data Warehouse</th></tr>
        <tr><td>Any kind of data from IoT devices, social media, sensors, mobile app, relational, text</td><td>Data</td><td>Relational data with corresponding tables defined in the warehouse</td></tr>
        <tr><td>Schema-on-read: Schema is constructed by analyst or system when retrieved</td><td>Schema</td><td>Schema-on-write: defined based on knowledge of the data to load</td></tr>
        <tr><td>Raw data from many disparate sources</td><td>Data Format</td><td>Data is carefully managed and controlled, predefined schema</td></tr>
        <tr><td>Uses low-cost object storage</td><td>Storage</td><td>Costly with large data volumes</td>=</tr>
        <tr><td>Change configuration as needed at any time</td><td>Agility</td><td>Configuration (schema/table structure) is fixed</td></tr>
        <tr><td>Machine learning specialists, data scientists, business analysts</td><td>Users</td><td>Management, business analysts</td></tr>
        <tr><td>Machine learning, data discovery, analytics applications, real-time streaming visualizations</td><td>Applications</td><td>Visualizations, business intelligence reporting</td></tr>
      </table>
    </div>
    - Data Warehouse
      - Data warehouse is optimized to store relational data from transactional systems with schema-on-write
      - Data lake stores all types of data: relational data and non-relational data from IoT devices, mobile apps, social media, etc. with schema-on-read
    - Redshift Node types
      - Node type defines CPU, memory, storage capacity and drive type
    - Other compute node considerations
      - Redshift distributes and executes your queries in parallel across all of your compute nodes
      - Increase performance by adding compute nodes
      - Clusters with more than one compute node, Redshift mirrors data on each node to another node, making data durable
      - When nodes are added, Redshift deploys and load balances for you
      - Can purchase reserved nodes
    - Compute node data distribution optimization
      - Efficient parallel processing accross your compute nodes
      - Three distribution modes
        - Key, All, Even
        {:refdef: style="text-align: center;"}
        ![Image]({{site.baseurl}}/images/aws_das_1.12.jpg)
        {:refdef}
        - Use cases:
          - **Key**: Rows are distributed according to the values in one column. The leader node will place matching values on the same node slice and matching values from the common columns or physically stored together. Used for large joins across a particular column
          - **Even**: Distributes the rows across the slices in a round robin fashion. Steps through each individual slice and keep assigning new data to each slice in a circular manner. Used for small tables that don't participate in joins and don't change often
          - **All**: A copy of the entire table is distributed to every node that ensures that every row is co-located for every join that the table participates in the all distribution multiplies the storage required by the number of nodes in the cluster. Use for small tables that are updated infrequently and are not updated extensively
          - **Auto**: Automatically assigned
    - Compute node data distribution optimization
  
- ### Select appropriate data layout, schema, structure, and format
  - ### _DynamoDB Partition Keys and Burst/Adaptive Capacity_
    - Optimal data distribution using DynamoDB partition keys
      - Ensure uniform activity across all logical partition keys in the table and its secondary indexes
      - Burst/Adaptive capacity: automatically enabled
        {:refdef: style="text-align: center;"}
        ![Image]({{site.baseurl}}/images/aws_das_1.13.jpg)
        {:refdef}
        - Allows DynamoDB to run your imbalanced workloads
          - "hot" partitions receive more reads/writes than other partitions and can lead to throttling
        - Adaptive capacity automatically and instantly increases the throughput capacity for hot partitions
  - ### _Redshift Sort Keys_
    - Sort key definition
      - When you load your data the rows are sorted in sorted order
      - Sort key column info is passed to the query planner, which uses the info to build plans that benefit from that sort information
      - Compound or Interleaved Sort Key
        - When query predicates use a prefix, which is a subset of the sort key column in order, a **compound sort key** is more efficient
        - **Interleaved sort keys** weight each column in the sort key equally; query predicates can use any subset of the columns that make up the sort key, in any order
  - ### _Redshift `COPY` Command_
    - `COPY` command is the most efficient way to load a Redshift table
      - Read from multiple data files or multiple data streams simultaneously
      - Redshift assigns the workload to the cluster nodes and loads the data in parallel, inclduing sorting the rows and distributing data across node slices
      - **Note**: Can't `COPY` into Redshift Spectrum tables
    - After ingestion or deletion, use `VACUUM` command to reorganize your data in your tables
  - ### _Redshift Compression Types_
    - Compression encoding defines the type of compression that is applied to a column, as rows are added to a table
    - If you don't specify a compression type at table creation or alter time Redshift applies this logic
      - Columns that are sort keys get `RAW` compression
      - Columns that are `BOOLEAN`, `REAL` or `DOUBLE PRECISION` get `RAW` compression
      - Columns that are `SMALLINT`, `INTEGER`, `BIGINT`, `DECIMAL`, `DATE`, `TIMESTAMP`, or `TIMESTAMPTZ` get `AZ64` compression
      - Columns that are `CHAR` or `VARCHAR` get `LZO` compression
  - ### _Primary Key and Foreign Key Constraints_
    - Informational only, not enforced by Redshift but used to give more efficient query plans
    - Query planner uses primary and foreign keys in some statistical computations to order large numbers of joins, and to eliminate redundant joins
- ### Define data lifecycle based on usage patterns and business requirements
  - ### _Data Lifecycle_
    - S3 Data Lifecycle
      - Lifecycle policies
      - S3 replication - business that require data to be distributed accross accounts or regions
    - Database backups
      - Redshift, RDS, DynamoDB
  - ### _S3 Data Lifecycle_
  
    <div class="table-container">
      <table>
        <tr><th>Storage Class</th><th>Intended Use</th></tr>
        <tr><td>S3 Standard</td><td>Frequently accessed data</td></tr>
        <tr><td>S3 Standard-IA</td><td>Long-lived, infrequently accessed data</td></tr>
        <tr><td>S3 Intelligent Tiering</td><td>Long-lived data with changing or unknown access patterns</td></tr>
        <tr><td>S3 One-Zone-IA</td><td>Long-lived, infrequently accessed non-critical data</td></tr>
        <tr><td>S3 Glacier</td><td>Long-term data archiving with retrieval times of minutes to hours tolerated</td></tr>
        <tr><td>S3 Glacier Deep Archive</td><td>Archive of rarely accessed data with retrieval time of 12 hours as default</td></tr>
      </table>
    </div>
    - Storage classes
      - Help reduce cost of data storage
      - Allow you to choose the right storage tier based on the characteristics of your data
  - ### _S3 Lifecycle Policies_
    - Lifecycle rules configuration configures S3 when to transition objects to another Amazon S3 storage class
    - Define rules to move objects from one storage class to another
    - Transition between storage classes uses a waterfall model
      {:refdef: style="text-align: center;"}
      ![Image]({{site.baseurl}}/images/aws_das_1.15.jpg)
      {:refdef}
    - Encrypted objects stay encryped throughout their lifecycle
    - Transition to S3 Glacier Deep Archive is a one way trip (use the restore operation to move object from Deep Archive)
  - ### _S3 Replication_
    - Replication copies your S3 objects automatically and asychronously across S3 buckets
      - Use Cross-Region Replication (CRR) to copy objects across S3 buckets in different regions
      - Use Same-Region Replication (SRR) to copy objects across S3 buckets in the same region
    - Use cases:
      - Compliance requirements - physically seperated backups
      - Latency - house replicated object copies local to your users
      - Operational efficiency - applications in different regions analyzing the same object data
  - ### _Database Backups_
    - Database management requires backups on a given frequency according to your requirements
    - Restores from backups
    - Redshift stores snapshots internally on S3
      - Snapshots are point-in-time backups of your cluster
    - DynamoDB allows for backup on demand
      - Backup and restore have no impact on the performance of your tables
    - RDS performs automated backups of your database instance
      - Can recover to any point-in-time
      - Can perform manual backups using database snapshots

- ### Determine the appropriate system for cataloging data and managing metadata
  - Hive records your data metastore information in a MySQL database housed on the master node file system
    - Hive metastore describes the table and the underlying data on which it is built
      - Partition names
      - Data types
    - At cluster termination, the master node shuts down
      - Local data is deleted since master node file system is on ephemeral storage
    - To maintain a persistent metastore, create an external metastore
      - two options
        - Glue data catalogue as Hive metastore
        - External MySQL or Aurora Hive metastore
  - ### _Glue Data Catalogue as Hive Metastore_
    - When you need a persistent metastore or a shared metastore used by different clusters, services, applications or AWS accounts
    - Metadata repository across many data sources and date formats
      - EMR, RDS, Redshift, Redshift Spectrum, Athena, application code compatible with Hive metastore
      - Glue crawlers infer the schema from your data objects in S3 and store the associated metadata in the Data Catalogue
    - Limitations
      - Hive transactions are not supported
      - Column level statistics are not supported
      - Hive authorizations are not suported, use Glue resource-based policies
      - Cost-based optimization in Hive is not supported
      - Temporary tables are not support
  - ### _External RDS Hive Metastore_
    {:refdef: style="text-align: center;"}
    ![Image]({{site.baseurl}}/images/aws_das_1.16.jpg)
    {:refdef}
    - Override the default for the metastore in Hive, use external database location
    - RDS MySQL or Aurora instance
    - Hive cluster runs using the metastore located in Amazon RDS
    - Start all additional Hive clusters that share this metastore by specifying the RDS metastore location
    - RDS replication is not enabled by default, configure replication to avoid any data loss in the vent of failure
    - Hive does not support and also does not prevent concurrent writes to metastore tables
      - When sharing metastore information between two clusters, do not write to the same metastore table concurrently, unless writing to different metastore table partitions
  - ### _Populating the Glue Catalogue_
    - Holds references to data used as sources and targets in your Glue (ETL) jobs
    - Catalog your data in the Glue Data Catalogue to use when creating your data lake or data warehouse
    - Holds information on the location, schema, and runtime metrics of your data
      - Use this information to create ETL jobs
      - Information stored as metadata tables, with each table describing a single data store
    - Ways to add metadata tables to your Data Catalogue
      - Glue crawler
      - AWS console
      - CreateTable Glue API call
      - CloudFormation templates
      - Migrate an Apache Hive metastore
    - Steps to Populate the Glue Data Catalogue
      - Four steps
        1. Classify your data by running a crawler
           - Custom classifiers
           - Built-in classifiers
        2. Crawler connects to the data store
        3. Crawler infers the schema
        4. Crawler writes metadata to the Data Catalogue
  - ### _Glue Ecosystem_
      - Categorizes, cleans, enriches, and moves your data reliably between various data stores
      - Several AWS services natively support querying data sources via the unified metadata repository of the Glue Data Catalogue
        - Athena
        - Redshift
        - Redshift Spectrum
        - EMR
        - RDS
        - Any application compatible with the Apache Hive metastore

## Processing
- ### Determine appropriate data processing solution requirements
  - ### _Glue ETL on Apache Spark_
    - Use Glue when you don't need or want to pay for an EMR cluster
      - Glue generates an Apache Spark (PySpark or Scala) script which you can edit
    - Glue runs in a fully managed Apache Spark environment
      - Spark has 4 primary libraries
        - Spark SQL
        - Spark Streaming
        - MLlib
        - GraphX
      - Cluster Managers
        - Yarn
        - Mesos
        - Standalone Scheduler
  - ### _EMR Cluster ETL Processing_
    - More flexible and powerful than Spark
      - Can use Spark on EMR, but there are other options
  - ### _EMR Integration_
    - Integrates with the following data stores
      - Use S3 as an object store for Hadoop
      - HDFS on the Core nodes instance storage
      - Directly access and process data in DynamoDB
      - Process data in RDS
      - Use `COPY` command to load data in parallel into Redshift from EMR
      - Integrates with S3 Glacier
  - ### _Kinesis ETL Processing_
    - Use Kinesis Analytics to gain real-time insights into your streaming data
      - Query data in your stream or build streaming applications using SQL or Flink
      - Use for filtering, aggregation, and anomaly detection
      - Preprocess your data with Lambda
  - ### _Glue ETL Jobs - Structure_
    - A Glue job defines the business logic that performs the ETL work in AWS Glue
      - Glue runs your script to extract data from your sources, transform the data, and load it into your targets
      - Glue triggers can start jobs based on a schedule or event, or on demand
      - Monitor your job runs to get runtime metrics, completion status, duration, etc.
      - Based on your source schema and target location or schema, the Glue code generator automatically creates an Apache Spark API (PySpark) script
        - Edit the script to customize to your requirements
  - ### _Glue ETL Jobs - Types_
    - Glue outputs file formats such as JSON, CSV, ORC, Parquet, and Avro
    - Three types of Glue jobs
      - Spark ETL job
        - Executed in managed Spark environment, processes data in batches
        - Streaming ETL job: (like a Spark ETL job, but works with data streams) uses the Apache Spark Structured Streaming framework
        - Python shell job: Schedule and run tasks that don't require an Apache Spark environment
  - ### _Glue ETL Jobs - Transforms_
    - Glue has built-in transforms for processing data
    - Call from within your ETL script
    - In a DynamicFrame (an extension of an Apache Spark SQL DataFrame), your data passes from transform to transform
    - Built-in transform types (subset)
      - ApplyMapping: maps source DynamicFrame columns and data types to target DynamicFrame columns and data types
      - Filter: selects records from a DynamicFrame and returns a filtered DynamicFrame
      - Map: applies a function to the records of a DynamicFrame and returns a transformed DynamicFrame
      - Relationalize: converts a DynamicFrame to a relational (rows and columns) form
  - ### _Glue ETL Jobs - Triggers_
    - A trigger can start specified jobs and crawlers
      - On demand, based on a schedule, or based on a combination of events
      - Add a trigger via the Glue console, the AWS CLI or the Glue API
      - Activate or deactivate a trigger via the Glue console, the CLI or the Glue API
  - ### _Glue ETL Jobs - Monitoring_
    - Glue produces metrics for crawlers and jobs for monitoring
      - Statistics about the health of your environment
      - Statistics are written to the Glue Data Catalogue
    - Use automated monitoring tools to watch Glue and report problems
      - CloudWatch events
      - CloudWatch logs
      - CloudTrail logs
    - Profile your Glue jobs using metrics and visualize on the Glue and CloudWatch consoles to identify and fix issues
  - ### _EMR Components_
    - EMR is built on clusters of EC2 instances
      - The EC2 instances are called nodes, all of which have roles (or node type) in the cluster
      - EMR installs different software components on each node type, defining the node's role in the distributed architecture of EMR
      - Three types of nodes
        - Master node: manages the cluster, running software components to coordinate the distribution of data and tasks across other nodes for processing
        - Core node: has software components that run tasks and store data in the HDFS on your cluster
        - Task node: node with software components that only runs tasks and does not store data in HDFS
  - ### _EMR Cluster - Work_
    - Options for submitting work to your EMR cluster
      - Script the work to be done as functions that you specify in the steps you define when you create a cluster
        - This approach is used for clusters that process data then terminate
      - Build a long-running cluster and submit steps (containing one or more jobs) to it via the console, the EMR API, or the AWS CLI
        - This approach is used for clusters that process data continuously or need to remain available
      - Create a cluster and connect to the master node and/or other nodes as required using SSH
        - This approach is used to perform tasks and submit queries, either scripted or interactively, via the interfaces of the installed applications
  - ### _EMR Cluster - Processing Data_
    - At launch time, you choose the frameworks and applications to install to achieve your data processing needs
    - You submit jobs or queries to installed applications or run steps to process data in your EMR cluster
      - Submitting jobs/steps to installed applications
        - Each step is a unit fo work that has instructions to process data by software installed on the cluster
  - ### _EMR Cluster - Lifecycle_
    - Provisions EC2 instances of the cluster
    - Runs bootstrap actions
    - Installs applications such as Hive, Hadoop, Sqoop, Spark
    - Connect to the cluster instances; cluster sequentially runs any steps that specified at creation; submit additional steps
    - After steps complete the cluster waits or shuts down, depending on config
    - When all instances are terminated, the cluster moves to `COMPLETED` state
  - ### _EMR Architecture - Storage_
    - Architected in layers
      - Storage: file systems used by the cluster
        - HDFS: distributes the data it stores across instances in the cluster(ephemeral)
        - EMRFS: directly access data stored in S3 as if it were a file system like HDFS
        - Local file system: EC2 locally connected disk
  - ### _EMR Architecture - Cluster Management_
    - YARN
      - Centrally manages cluster resources
      - Agent on each node that keeps the cluster healthy and communicates with EMR
      - EMR defaults to scheduling YARN jobs so that jobs won't fail when task nodes running on spot instances are terminated
  - ### _EMR Architecture - Cluster Management_
    - Framework layer that is used to process and analyze data
      - Different frameworks available
        - Hadoop MapReduce
          - Parallel distributed applications that use Map and Reduce functions
            - Map function maps data to sets of key-value pairs
            - Reduce function combines the key-value pairs and processes the data
        - Spark
          - Cluster framework and programming model for processing big data workloads
  - ### _Lake Formation_
    - Loading data and monitoring data flows
    - Setting up partitions
    - Encryption and managing keys
    - Defining transformation jobs and monitoring them
    - Built on top of Glue
  - ### _Data Pipeline_
    - Data pipeline lets you schedule tasks for processing your big data
    - Destinations include S3, RDS, DynamoDB, Redshift and EMR
    - Manage task dependencies
    - Precondition checks
    - Data sources may be on-premises

- ### Design a solution for transforming and preparing data for analysis
  - ### _Optimizing EMR_
    - **Instance type**
      - Ways to add EC2 instances to your cluster
        - Instance Groups
          - Manually add instances of the same type to existing core and task instance groups
          - Manually add a task instance group, can use a different instance type
          - Automatic scaling for an instance group based on the value of a CloudWatch metric specified by you
        - Instance Fleets
          - Add a single task instance fleet
          - Change the target capacity for On-Demand and Spot Instances for existing core and task instance fleets
        - Instance Types
          - Plan your capacity
            - Run a test cluster using a representative sample data set and monitor the node utilization
            - Calculate instance capacity and compare the value against the size of your data
          - Master node doesn't require high computation capacity
          - Most EMR cluster can run on m5.xlarge or m4.xlarge
    - **Instance configuration**
      - Persistent EMR cluster (e.g. Data Warehouse)
        - Master and core instance groups as on-demand instances
        - Task instance group as spot instances
        - Cost-driven workloads: low cost, partial work loss OK
          - Transient clusters
          - Run all groups, master, core and task instance groups as spot instances
        - Data-critical workloads: low cost, partial work loss not OK
          - Master and core instance groups as on-demand, task instance groups as spot
        - Test environment: all instance groups on spot
    - **HDFS capacity**
      - To calculate the storage allocation for your cluster consider the following
        - Number of EC2 instances used for core nodes
        - Capacity of the EC2 instance store for the instance type used
        - Number and size of EBS volumes attached to core nodes
        - Replication factor: how each data block is stored in HDFS for RAID-like redundancy
          - 3: for cluster of 10 or more core nodes
          - 2: for cluster of 4-9 core nodes
          - 1: for cluster of 3 or fewer nodes
        - HDFS capacity for your cluster
          - For each core node, add instance store volume capacity to EBS storage capacity
          - Multiply by the number of core nodes, then divide the total by the replication factor
    - **Dynamic sizing**
      - Dynamically scales nodes in your cluster based on demand
        - On scale down of task nodes on a running cluster expect a short delay for any running Hadoop to decommission
        - On scale down fo core nodes EMR waits for HDFS to decommission to protect your data
        - Changing configuration improperly on a cluster with high load can seriously degrade cluster performance
    - **Instance fleets vs. uniform instance groups**
      - Choice applies to all nodes for the lifetime of the cluster
      - Instance fleets and instance groups cannot coexist in a cluster
      - Instance fleets
        - Each node type has a single instance fleet; task instance fleet is optional
        - Up to 5 instance types (if using allocation strategy, up to 15 instance types on task instance fleets), which can be provisioned as On-Demand and Spot Instances
        - Mix of specified instance types to reach target capacities: On-Demand and Spot Instances
        - Core and Task instance fleets: assign a target capacity for On-Demand Instances, and another for Spot Instances
      - Instance Group
        - Simplified, up to 50 instance groups:
          - 1 master instance group containing 1 EC2 instance
          - Core instance group containing one or more EC2 instance, up to 48 optional task instance groups
          - Scale each instance group by adding and removing EC2 instances manually, or set up automatic scaling
    - **Run multiple steps in parallel**
      - Allows for parallel processing and greater speed
      - Considerations
        - Using EMR automatic scaling to scale up/down based on the YARN resources to prevent resource contention
        - Running multiple steps in parallel requires more memory and CPU utilization from the master node than running one step at a time
        - Use YARN scheduling features such as FairScheduler or CapacityScheduler to run multiple steps in parallel
        - If you run out of resources because the cluster is running too many concurrent steps, manually cancel any running steps to free up resources
    - **EMR - AWS integration**
      - AWS VPC to configure the virutal network in which you launch your instances
      - S3 to store input and output data
      - CloudWatch to monitor cluster performance and configure alarms
      - IAM to configure permissions
      - CloudTrail to audit requests made to the service
      - Data Pipeline to schedule and start your clusters
  - ### _Batch versus Streaming ETL Services_
    - Based on your use case, you need to select the best tool or service
    - Batch or Streaming ETL
      - **Batch processing model**
        - Data is collected over a period of time, then run through analytics tools
        - Time consuming, design for large quantities of information that aren't time-sensitive
        - AWS services used for batch processing
          - **Glue batch ETL**
            - Schedule ETL jobs to run at a maximum of 5-minute intervals
            - Process micro-batches
            - Serverless
          - **EMR batch ETL**
            - Use Impala or Hive to process batches of data
            - Cluster of servers
      - **Streaming processing model**
        - Data is processed in a stream, a record at a time or in micro-batches
        - Fast, designed for information that's needed immediately
        - AWS services used for stream processing
          - Lambda
            - Read records from your data stream, runs functions synchronously
            - Frequently used with Kinesis
            - Serverless
          - Kinesis
            - Use the KCL, Kinesis Analytics, Kinesis Firehose to process your data
            - Serverless
          - EMR Streaming ETL
            - Use Spark Streaming to build your stream processing ETL application
            - Cluster of servers
- ### Automate and operationalize data processing solutions
  - ### _Orchestration of Workflows_
    - Coordinating your ETL jobs across Glue, EMR, and Redshift
      - With orchestration, you can automate each step in your workflow
        - Retry on errors
        - Find and recover jobs that fail
        - Track the steps in your workflow
        - Build repeatable workflows
      - Respond to state changes on EMR cluster
      - Using CloudWatch metrics to manage your EMR cluster
      - View and monitor your EMR cluster
      - Leverage EMR API calls in CloudTrail
  - ### _Respond to State Changes on EMR Cluster_
    - Trigger create, terminate, scale cluster, run Spark, Hive or Pig workloads based on Cluster state changes
    - EMR CloudWatch events support notify you of state changes in your cluster
    - Respond to state changes programmatically
      - EMR CloudWatch events
        - Cluster State Change
        - Instance Group and Instance Fleet State Change
        - Step State Change
        - Auto Scaling State Change
        - Create filters and rules to match events and route them to SNS topics, Lambda functions, SQS queues, Kinesis Streams
  - ### _Use CloudWatch Metrics to Manage EMR Cluster_
    - EMR metrics updated every 5 minutes, collected and pushed to CloudWatch
      - Non-configurable metric timing
      - Metrics archived for two weeks then discarded
    - EMR metrics uses
      <div class="table-container">
          <table>
            <tr><th>Use Case</th><th>Metrics</th></tr>
            <tr><td>Track progress of cluster</td><td>`RunningMapTasks`, `RemainingMapTasks`, `RunningReduceTasks`, and `RemainingReduceTasks` metrics</td></tr>
            <tr><td>Detect idle clusters</td><td>`IsIdle` metric tracks if a cluster is live, but not currently running tasks. Set an alarm to fire when the cluster has been idle for a given period of time</td></tr>
            <tr><td>Detect when a node runs out of storage</td><td>`HDFSUtilization` metric gives the percentage of disk space currently used. If it rises above an acceptable level for your application, such as 80% of capacity used, you take action to resize your cluster and add more core nodes</td></tr>
          </table>
      </div>
  - ### _View and Monitor EMR Cluster_
    - EMR has several tools for retrieving information about your cluster
      - Console, CLI, API
      - Hadoop web interfaces and logs on Master node
      - Use monitoring services like CloudWatch and Ganglia to track the performance of your cluster
      - Application history available through persistent application UIs for Spark history Server, persistent YARN timeline server, and Tez user interfaces
 
  - ### _Leverage EMR API Calls in CloudTrail_
    - CloudTrail holds a record of actions taken by users, roles or an AWS service in EMR
      - Captures all API calls for EMR as events
      - Enable continuous delivery of CloudTrail events to an S3 bucket
      - Determine the EMR request, the IP address from which the request, when it was made, and additional details
  - ### _Orchestrate Spark and EMR workloads using Step Functions_
    - Directly connect Step Functions to EMR
      - Create data processing and analysis workflows with minimal code and optimize cluster utilization
      - Easy visualizations
  - ### _Workflows in Glue_
    - Use workflows to create and visualize complex ETL tasks involving multiple crawlers, jobs and triggers
    - Manages the execution and monitoring of all components
    - Glue console provides a visual representation of a workflow as a graph
    - Chain interdependent jobs and crawlers
      - Event triggers fired by both jobs or crawlers, and can start both jobs and crawlers
    - Views
      - Static: shows the design of the workflow
      - Dynamic: run time view, shows the latest run information for each of the jobs and crawlers
  - ### _Orchestration of Glue and EMR Workflows_
    - Several ways to operationalize Glue and EMR workflows
      - Glue workflows
        - A workflow is a grouping of set of jobs, crawlers, and triggers in GLue
          - Can design a complex multi-job (ETL) sequence that Glue can execute and track as single entity
          - Create workflows using the AWS Console or the Glue API
          - Console lets you to see the components and flow of a workflow with a graph
      - Automate workflow using Lambda
        - Use Lambda functions and CloudWatch Events to orchestrate your workflow
          - Start your workflow with Lambda trigger
          - Use CloudWatch Events to trigger other steps in your workflow
      - Step Functions with Glue
        - Serverless orchestration of your Glue steps
        - Easily integrates with EMR steps
      - Step Functions directly with EMR
        - Use Step Functions to control all steps in your EMR cluster operation
        - Also integrate other AWS services into your workflow

## Analysis and Visualization
- ### Determine the operational characteristics of the analysis and visualization solution
  - ### _Purpose-Built Analytics Services_
    - Choose the correct approach and tool for your analytics problem
    - Know the AWS purpose-built analytics services
      - Athena
      - Elastic Search
      - EMR
      - Kinesis - Data Streams, Firehose, Analytics, Video Streams
      - Redshift
      - MSK
      - Also know where to use
        - S3
        - EC2
        - Glue
        - Lambda
  - ### _Appropriate Analytics Service_
    <div class="table-container">
      <table>
        <tr><th>Category</th><th>Use Case</th><th>Analytics Service</th></tr>
        <tr><td rowspan="6" style="font-weight: bold">Analytics</td><td>Interactive analytics</td><td>Athena</td></tr>
        <tr><td>Big data processing</td><td>EMR</td></tr>
        <tr><td>Data Warehousing</td><td>Redshift</td></tr>
        <tr><td>Real-time analytics</td><td>Kinesis</td></tr>
        <tr><td>Operational analytics</td><td>Opensearch</td></tr>
        <tr><td>Dashboards and visualizations</td><td>QuickSight</td></tr>
        <tr><td style="font-weight: bold">Data Movement</td><td>Real-Time data movement</td><td> 
        Managed Streaming for Kafka (MSK),
        Kinesis Data Streams,
        Kinesis Data Firehose,
        Kinesis Data Analytics,
        Kinesis Video Streams,
        Glue</td></tr>
        <tr><td rowspan="4" style="font-weight: bold">Data Lake</td><td>Object storage</td><td>S3, Lake Formation</td></tr>
        <tr><td>Backup and archive</td><td>S3 Glacier, AWS Backup</td></tr>
        <tr><td>Data catalog</td><td>Glue, Lake Formation</td></tr>
        <tr><td>Third-part data</td><td>AWS Data Exchange</td></tr>
        <tr><td rowspan="2" style="font-weight: bold">Predictive Analytics/ML</td><td>Frameworks and interfaces</td><td>AWS Deep Learning AMis</td></tr>
        <tr><td>Platform services</td><td>SageMaker</td></tr>
      </table>
    </div>
    
  - ### _Use Cases - Data Warehousing_
    - Without unnecessary data movement use SQL to query structured and unstructured data in your data warehouse and data lake
    - Redshift
      - Query petabytes of structured, semi-structured, and unstructured data
        - Data Warehouse
        - Operational Database
        - S3 Data Lake using Redshift Spectrum
      - Save query results to S3 data lake using common formats such as Parquet
        - Analyze using other analytics services such as EMR, Athena, and SageMaker
  - ### _Use Cases - Big Data Processing_
    - Process large amounts of data in your data lake
    - EMR
      - Data Engineering
      - Data Science
      - Data Analytics
    - Spin up/spin down clusters for short running jobs, such as ingestion clusters, and pay by the second
    - Automatically scale long-running clusters, such as query clusters, that are highly available
  - ### _Use Cases - Real Time Analytics with MSK_
    - Data collection, processing, analysis on streaming data loaded directly into data lake, data stores, analytics services
    - Uses Apache Kafka to process streaming data
      - To and from Data Lake and databases
      - Power machine learning and analytics applications
    - MSK provisions and runs Apache Kafka cluster
    - Monitors and replaces unhealthy nodes
    - Encrypts data at rest
  - ### _Use Cases - Real Time Analytics with Kinesis_
    - Ingest real-time data such as video, audio, application logs, website clickstreams, and IoT data
    - Kinesis
      - Ingest data in real-time for machine learning and analytics
        - Process and analyze data as it streams to your data lake
        - Process and respond in real-time on your data stream
  - ### _Use Cases - Operational Analytics_
    - Search, filter, aggregate, and visualize your data in near real-time
    - Opensearch
      - Application monitoring, log analytics, and clickstream analytics
        - Managed Kibana
        - Alerting and SQL querying
- ### Usage patterns, performance and cost
  - ### _Glue_
    - **Usage:**
      - Crawl your data and generate code to execute, includes data transformations and loading
      - Integrate with services like Athena, EMR, and Redshift
      - Generates customizable, reusable, and portable ETL code using Python
    - **Cost:**
      - Hourly rate, billed by the minute, for crawler and ETL jobs
      - Glue Data Catalogue: pay a monthly fee for storing and accessing your metadata
    - **Performance:**
      - Scale-out Apache Spark environment to load data to target data store
      - Specify the number of Data Processing Units (DPUs)
    - **Durability and Availability:**
      - Glue leverages the durability of the data stores to which you connect
      - Provides job status and pushes notifications to CloudWatch events
      - Use SNS notifications from CloudWatch events to notify of job failures or success
    - **Scalability and Elasticity:**
      - Runs on top of Apache Spark for transformation job scale-out execution
    - **Interfaces:**
      - Crawlers scan many data store types
      - Bulk import Hive metastore into Glue Data Catalog
    - **Anti-Patterns:**
      - Streaming data, unless Spark Streaming
      - Heterogeneous ETL job types, use EMR
  - ### _Lambda_
    - Run code without provisioning or managing servers
    - **Usage:**
      - Execute code in response to triggers such as changes in data, changes in system state, or actions by users
      - Real-time file processing and stream
      - Process AWS Events
      - Replace CRON
      - Perform ETL jobs
      - Note: S3 has the ability to trigger a Lambda function whenever a new object appears in a bucket
    - **Cost:**
      - Charged by the number of requests to functions and code execution time
      - $0.20 per 1,000,000 requests
    - **Performance:**
      - Process events within milliseconds
      - Latency higher for cold start
      - Retains a function instance and reuses it to serve subsequent requests, versus creating new copy
    - **Durability and Availability:**
      - No maintenance windows or scheduled downtime
      - On failure, Lambda synchronously responds with an exception
      - Asynchronous functions are retried at least 3 times
    - **Scalability and Elasticity:**
      - Scales automatically with no limits with dynamic capacity allocation
    - **Interfaces:**
      - Trigger Lambda with AWS service events
      - Respond to CloudTrail audit log entries as events
    - **Anti-Patterns:**
      - Long-running applications: 900 sec runtime
      - Dynamic websites
      - Stateful applications
    - **Lambda + Kinesis**
      - Your Lambda code receives an event with a batch of stream records
        - You specify a batch size when setting up the trigger (up to 10,000 records)
        - Too large a batch size can cause timeouts
        - Batches may also be split beyond Lambda's payload limit (6 MB)
        - Lambda polls your Kinesis streams for new activity
      - Lambda will retry the batch until it succeeds or the data expires
        - This can stall the shard if you don't handle errors properly
        - Use more shards to ensure processing isn't totally held up by errors
      - Lambda processes shard data synchronously

  - ### _EMR_
    - **Usage:**
      - Reduces large processing problems and data sets into smaller jobs and distributes them across many compute nodes in a Hadoop cluster
      - Log processing and analytics
      - Large ETL data movement
      - Ad targeting, click stream analytics
      - Predictive analytics
    - **Cost:**
      - Pay for the hours the cluster is up
      - EC2 pricing options (On-Demand, Reserved, and Spot)
      - EMR price is in addition to EC2 price
    - **Performance**
      - Driven by type/number of EC2 instances
      - Ephemeral or long-running
    - **Durability and Availability:**
      - Starts up with a new node if core node fails
      - Won't replace nodes if all nodes in the cluster fail
      - Monitor for node failure through CloudWatch
    - **Scalability and Elasticity:**
      - Resize your running cluster: add core nodes, add/remove task nodes
    - **Interfaces:**
      - Tools on top of Hadoop: Hive, Pig, Spark, HBase, Presto
      - Kinesis Connector: directly read and query data from Kinesis Data Streams
    - **Anti-Patterns:**
      - Small data sets
      - ACID transactions; RDS is a better choice
  - ### _Kinesis_
    - **Usage:**
      - Move data from producers and continuously process it to transform before moving to another data store; drive real-time metrics and analytics
      - Real-time data analytics
      - Log intake and processing
      - Real-time metrics and reporting
      - Kinesis Analytics can only monitor streams from Kinesis, but both data streams and Firehose are supported
      - Kinesis Analytics must have a stream as its input, and a stream or Lambda function as its output
      - If a record arrives late to your application during stream processing, it is written to the error stream
      - Kinesis Data Analytics provisions capacity in the form of Kinesis Processing Units (KPU). A single KPU provides you with the memory (4 GB) and corresponding computing and networking. The default limit for KPUs for your application is eight.
      - Video/Audio processing
         {:refdef: style="text-align: center;"}
         ![Image]({{site.baseurl}}/images/aws_das_1.22.jpg)
         {: refdef}
    - **Cost**
      - Pay for the resources consumed
      - Data Streams hourly price per/shard
      - Charge for each 1 million `PUT` transactions
    - **Performance**
      - Data Streams: throughput capacity by number of shards
      - Provision as many shards as needed
    - **Durability and Availability:**
      - Near real-time
      - Synchronously replicates data across three AZs
      - Highly available and durable due to config of multiple AZs in one Region
      - Use cursor in DynamoDB to restart failed apps
      - Resume at exact position in the stream where failure occurred
    - **Scalability and Elasticity:**
      - Use API calls to automate scaling, increase or decrease stream capacity at any time
    - **Interfaces:**
      - Two interfaces: input (KPL, agent, PUT API), output (KCL)
      - Kinesis Storm Spout: read form a Kinesis stream into Apache Storm
    - **RANDOM_CUT_FOREST:**
      - SQL function used by anomaly detection on numeric columns in a stream
      - They're especially proud of this because they published a paper on it
    - **Anti-Patterns:**
      - Small scale consistent throughput
      - Long-term data storage and analytics, Redshift, S3, or DynamoDB are better choices
  - ### _DynamoDB_
    - **Usage:**
      - Apps needing low latency NoSQL database able to scale storage and throughput or down without code changes or downtime
      - Mobile apps and gaming
      - Sensor networks
      - Digital ad serving
      - E-Commerce shopping carts
    - **Cost:**
      - Three components:
        - Provisioned throughput capacity(per hour)
        - Indexed data storage (per GB per month)
        - Data Transfer in or out (per GB per month)
    - **Performance**
      - SSDs and limiting indexing on attributes provides high throughput and low latency
      - Define provisioned throughput capacity required for your tables
    - **Durability and Availability:**
      - Protection against individual machine or facility failures
      - DynamoDB Streams allows replication across regions
      - Streams enables table data activity replicated across geographic regions
    - **Scalability and Elasticity:**
      - No limit to data storage, automatic storage allocation, automatic data partition
    - **Interfaces:**
      - REST API allows management and data interface
      - DynamoDB select operation creates SQL-like queries
    - **Anti-Patterns:**
      - Port applications from relational databases
      - Joins or complex transactions
      - Large data with low I/O rate
      - Blob data > 400kb, S3 is a better choice
  - ### _Redshift_
    - **Usage:**
      - Designed for OLAP using business intelligence tools
      - Analyze sales data for multiple products
      - Analyze ad impressions and clicks
      - Aggregate gaming data
      - Analyze social trends
    - **Cost:**
      - Charges based on the size and number of cluster nodes
      - Backup storage > provisioned storage size and backups stored after cluster termination billed at standard S3 rate
    - **Performance:**
      - Columnar storage, data compression, and zone maps to reduce query I/O
      - Parallelizes and distributes SQL operations to take advantage of all available resources
    - **Durability and Availability:**
      - Automatically detects and replaces a failed node in your data warehouse cluster
      - Failed node cluster is read-only until replacement node is provisioned and added to the DB
      - Cluster remains available on drive failure; Redshift mirrors your data across the cluster
      - Backup to S3
      - Automatic snapshots
      - Limited to 1 AZ
    - **Scalability and Elasticity:**
      - With API change the number, or type, of nodes while cluster remains live
    - **Interfaces:**
      - JDBC and ODBC drivers for use with SQL clients
      - S3, DynamoDB, Kinesis, BI tools such as QuickSight
    - **Anti-Patterns:**
      - Small data sets
      - Online transaction processing
      - Unstructured data
      - Blob data, S3 is a better choice
    - **Analytics and Visualization:**
      - Two options:
        - Query via AWS management console using the query editor, or via a SQL client tool
          - Supports SQL client tools via JDBC and ODBC
          - Using the Redshift API you can access Redshift data with web services-based applications, including AWS Lambda, AWS AppSync, SageMaker notebooks, and AWS Cloud9
      - Redshift Spectrum
        - Query data directly from files on S3
        - Need a Redshift cluster and a SQL client connected to your cluster to execute SQL commands
        - Visualize data via QuickSight
        - Cluster and data in S3 must be in the same region
    - **Integration with other services:**
      - You can load data from DynamoDB, EMR, EC2, Data Pipeline, DMS to Redshift using `COPY`
    - **Redshift Workload Management(WLM):**
      - It's a way to help users prioritize workload by ensuring that short, fast running queries are not stuck behind long running slow queries. 
      - The way it works is by creating query queues at runtime according to service classes and configuration parameters for various types of cues are defined by those service classes.
    - **Concurrency Scaling:**
      - Automatically adds cluster capacity to handle increase in concurrent `READ` queries
      - WLM queues manage which queries are sent to the concurrency scaling cluster
      - Short Query Acceleration
        - Prioritize short-running queries over longer-running ones
        - Short queries run in a dedicated space, won't wait in queue behind long queries
        - Can be used in place of WLM queues for short queries
    - **`VACUUM` command:**
      - `VACUUM FULL`: It will resort all  rows and reclaimed space from deleted rows.
      - `VACUUM DELETE ONLY`: which is the same as a full vacuum, except that it skips the sorting part. Just reclaims deleted row space and not actually resort it.
      - `VACUUM SORT ONLY`: Resorts the table, but does not reclaim the space.
      - `VACUUM REINDEX`: Used for re-initializing interleaved indexes
    - **Resizing Redshift Clusters:**
      - Elastic resize
        - Quickly add or remove nodes of same type
        - Cluster is down for a few minutes
        - Tries to keep connection open across the downtime
        - Limited to doubling or halving for some node types
      - Classic resize
        - Change node type and/or number of nodes
        - Cluster is read-only for hours to days
      - Snapshot, restore, resize
        - Used to keep cluster available during a classic resize
        - Copy cluster, resize new cluster
    - **Redshift Security concerns:**
      - Using a Hardware Security Module
        - Must be a client and server certificate to configure a trusted connection between Redshift and the HSM
        - If migrating an unencrypted cluster to an HSM-encrypted cluster, you must create the new encrypted cluster and then move data to it
      - Defining access privileges for user or group
        - Use the `GRANT` or `REVOKE` commands in SQL
        - 
  - ### _Athena_
    - **Usage:**
      - Interactive ad hoc querying for web logs
      - Presto under the hood
      - Serverless
      - Query staging data before loading into Redshift (stays in S3)
      - Sending AWS service logs to S3 for Analysis with Athena
      - Integrate with Jupyter, Zepplin
      - Integrate with QuickSight
      - Can organize users/teams/apps/workloads into Workgroups
        - Integrates with IAM, CloudWatch, SNS
        - Each workgroup can have its own data history, data limits, etc.
    - **Cost:**
      - $5 per TB of query data scanned
      - Save on per-query costs and get better performance by compressing, partitioning, and converting data into columnar formats
      - Save LOTS of money by using columnar formats like ORC, Parquet 
    - **Performance**
      - Compressing, partitioning, and converting your data into columnar formats
      - Convert data to columnar formats like Parquet and ORC, allowing Athena to read only the columns it needs to process queries
    - **Durability and Availability:**
      - Executes queries using compute resources across multiple facilities
      - Automatically routes queries if a particular facility is unreachable
      - S3 is the underlying data store, gaining S3's 11 9s durability
    - **Scalability and Elasticity:**
      - Serverless, scales automatically as needed
    - **Interfaces:**
      - Athena CLI, API via SDK, and JDBC
      - QuickSight visualizations
    - **Anti-Patterns:**
      - Enterprise Reporting and Business Intelligence Workloads; Redshift better choice
      - ETL Workloads; EMR and Glue is a better choice
      - Not a replacement for RDBMS
      - Querying across regions (unless you query a Glue Data Catalogue)
    - **Analytics and Visualization:**
      - Athena queries data sources that are registered with the AWS Glue Data Catalogue
      - Running queries in Athena via the Data Catalogue uses the Data Catalogue schema to derive insight from the underlying dataset

  - ### _Aurora_
    - MySQL and PostgreSQL - compatible
    - faster than both
    - up to 15 read replicas
    - Continuous back up to S3

  - ### _Opensearch_
    - **Usage:**
      - Fully managed
      - Analyze activity logs, social media sentiments, data stream updates from other AWS services
      - Usage monitoring for mobile applications
    - **Integration:**
      - S3 buckets
      - Kinesis Data streams
      - DynamoDB streams
      - CloudWatch/CloudTrail
      - Zone Awareness
      - Kinesis, DynamoDB, Logstash / Beats, and OpenSearch's native API's offer means to import data into Amazon OS
    - **Options:**
      - Dedicated master node
        - Now, the master nodes are only used for the management of Opensearch domain that you're creating, and it does not hold or process any data. You don't need too many of them unless your cluster is really massive.
      - Domains: An Amazon Opensearch service domain is a collection of all the resources needed to run the Opensearch cluster. it contains all the configuration for the cluster as a whole. A cluster in Amazon Opensearch parlance is a domain.
    - **Cost:**
      - Opensearch instance hours
      - EBS storage (if you choose this option), and standard data transfer fees
    - **Performance:**
      - Instance type, workload, index, number of shards used, read replicas, storage configurations
      - Fast SSD instance storage for storing indexes or multiple EBS volumes
    - **Durability and Availability:**
      - Distributes domain instances across two different AZs
      - Automated and manual snapshots for durability
    - **Scalability and Elasticity:**
      - Use API and CloudWatch metrics to automatically scale up/down
    - **Interfaces:**
      - S3, Kinesis, DynamoDB Streams, Kibana
      - Lambda function as an event handler
    - **Anti-Patterns:**
      - OLTP; RDS better choice
      - Ad hoc data querying, Athena is a better choice
      - Opensearch is primarily for search and analytics.

  - ### _QuickSight_
    - **Usage:**
      - ad-hoc data exploration/visualization
      - Dashboards and KPIs
      - Analyze and visualize data coming from logs and stored in S3
      - Analyze and visualize data in SaaS applications like Salesforce
    - **Cost:**
      - Standard: $9/user/month
      - Enterprise: $18/user/month
      - SPICE capacity for Standard
    - **Performance:**
      - SPICE uses a combination of columnar storage, in-memory technologies
      - Machine code generation to run interactive queries on large datasets at low latency
    - **Durability and Availability:**
      - Automatically replicates data for high availability
      - Scales to hundreds of thousands of users
      - Simultaneous analytics across AWS data sources
    - **Scalability and Elasticity:**
      - Fully managed service, scale to terabytes of data
    - **Interfaces/Data Sources:**
      - RDS, Aurora, Redshift, Athena, S3
      - SaaS, applications such as Salesforce
      - Files (S3 or on-premises) such as Excel, CSV
    - **Anti-Patterns:**
      - Highly formatted canned Reports, better for ad hoc query, analysis, and visualization of data
      - ETL; Glue is a better choice
    - **Capabilities:**
      - Need to know the capabilities and operational characteristics
        - **Data Source**
          {:refdef: style="text-align: center;"}
          ![Image]({{site.baseurl}}/images/aws_das_1.17.jpg)
          {:refdef}
        - **SPICE**
          - Superfast, parallel, in memory calculation engine
          - Engineered to rapidly perform advanced calculations and serve data
          - In the Enterprise edition, data in SPICE is encrypted at rest
          - Can release unused SPICE capacity(in-memory data)
          - Each user gets 10GB of SPICE
        - **Analysis**
          - Use analysis to create and interact with visuals and stories, a container for a set of related visuals and stories
          - Use multiple data sets in an analysis, but any given visual can only use one data set
        - **Visuals**
          - Graphical representation of a data set using a diagram, chart, graph or table
           {:refdef: style="text-align: center;"}
           ![Image]({{site.baseurl}}/images/aws_das_1.18.jpg)
           {:refdef}
        - **ML Insights**
          - ML Insights use machine learning to uncover hidden insights and trends in your data, identify key drivers, and forecast business metrics
          - Quicksight Q allows NLP powered business questions
            {:refdef: style="text-align: center;"}
            ![Image]({{site.baseurl}}/images/aws_das_1.19.jpg)
            {:refdef}
        - **Sheets**
          - Set of visuals that are viewed together in a single page(like excel)
        - **Dashboards**
          - Read-only snapshot of an analysis that you can share with other users for reporting
          - Preserves the configuration of the analysis at the time of publishing
          - When a user views the dashboard, it reflects the current data in the data sets used by the analysis
  - ### _Analysis for Visualization_
    - **Data Lake**
      - Lake Formation provides multi-service, fine-grained access control to data
      - Macie helps detect sensitive data that may have been stored in the wrong place
      - Inspector identifies configuration errors that could lead to data breaches
- ### Select the appropriate data analysis solution for a given scenario
  - ### _Types of Analysis_
    - **Descriptive Analysis (Data Mining)**
      - Determine _what_ generated the data
      - Lowest value
    - **Diagnostic Analysis**
      - Determine _why_ data was generated
    - **Predictive Analysis**
      - Determine _future outcomes_
    - **Prescriptive Analysis**
      - Determine _action to take_
  - ### _Analysis Solutions_
    - **Batch Analytics**
      - Large volumes of raw data
      - Analytics process on a schedule, reports
      - EMR
    - **Interactive Analytics**
      - Complex queries on complex data at high speed
      - See query results immediately
      - Athena, Opensearch, Redshift
    - **Streaming Analytics**
      - Analysis of data that has short shelf-life
      - Incrementally ingest data and update metrics
      - Kinesis
  - ### _Analytics Solutions Patterns_
    - **EMR**
      - Uses the map-reduce technique to reduce large processing problems into small jobs distributed across many nodes in a Hadoop cluster
        - On demand big data analytics
        - Event-driven ETL
        - Machine Learning predictive analytics
        - Clickstream analysis
        - Load data warehouse
      - Don't use for transactional processing or with small data sets
    - **Kinesis**
      - Streams data to analytics processing solutions
        - Video analytics applications
        - Real-time analytics applications
        - Analyze IoT device data
        - Blog posts and article analytics
    - **Redshift**
      - OLAP using BI tools
        - Near real-time analysis of millions of rows of manufacturing data generated by continuous manufacturing equipment
        - Analyze events from mobile app to gain insight into how users use it
        - Make live data generated by range of next-gen security solutions available to large numbers of organizations for analysis
      - Don't use OLTP or with small data sets
- ### Select the appropriate data visualization solution for a given scenario
  - ### _Refresh Schedule - Real time Scenarios_
    - Typically using Opensearch and Kibana
      - `refresh_interval` in Opensearch domain updates the domain indices; determines query freshness
      - Default is every second
      - Formula: K * (number of data nodes), where k is the number of shards per node
      - Balance refresh rate cost with decision-making needs
  - ### _Refresh Schedule - Interactive Scenarios_
    - Typically ad-hoc exploration using QuickSight
    - Refresh Spice data
      - Refresh a data set on a schedule, or you can refresh your data by refreshing the page in an analysis or dashboard
      - Use the `CreateIngestion` API operation to refresh data
    - Data set based on a direct query and not stored in SPICE, refresh data by opening the data set
  - ### _Refresh Schedule - EMR notebooks_
    - Use EMR notebooks to query and visualize data
    - Data refreshed every time the notebook is run
  - ### _QuickSight Visual Data - Filters_
    - QuickSight filter options for data interaction
      - Filtering: used to focus on or exclude a visual element representing a particular value
        - Associated with a single dataset in an analysis
        - Scope to one, several, or all visuals in a dataset's analysis
        - Applies to only a single field, calculated or regular
        - Make sure multiple filters applied to the same field aren't mutually exclusive
  - ### _QuickSight Visual Data - Sorting_
    - QuickSight sorting options for data interaction
      - Sorting: Most visual types offer the ability to change data sort order
      - SPICE limitations for sort
        - Up to two million unique values
        - Up to 16 columns
  - ### _QuickSight Visual Data - Drill Down_
    - QuickSight drill down options for data interaction
      - Drill Down: All visual types except pivot tables allow creation of a hierarchy of fields for a visual element
  - ### _Kibana - Visualization Tool_
    - Open source data visualization dn exploration tool used for log and time-series analytics, application monitoring, and operational intelligence use cases
    - Histograms, line graphs, pie charts, heat maps, and built in geospatial support
    - Tight integration with Opensearch
    - Charts and reports to interactively navigate through large amounts of log data
    - Pre-built aggregations and filters
    - Dashboards and reports to sharer
  - ### _Kibana - Configuration_
    - To visualize and explore data in Kibana, you must first create index patterns
    - Index patterns point Kibana to the Opensearch indexes containing the data to explore
    - Explore data with Kibana's data discovery functions
    - Kibana visualizations are based on Amazon Opensearch queries
  - ### _Kibana - Security_
    - Cognito allows end-uers to log in to Kibana through enterprise identity providers such as Microsoft Active Directory using SAML
  - ### _Visualization Use Case_
      <div class="table-container">
          <table>
            <tr><th>Use Case</th><th>Visualization Solution</th></tr>
            <tr><td>Equity trading managers need to drill into
            values in a visualization and perform ad-
            hoc queries on data that spans five days
            but is updated by the minute</td>
            <td>Browser access to an interactive report
            based on an Opensearch Kibana view</td></tr>
            <tr><td>A company's website shares previous
            years' sales volume by region, reports
            monthly revenue to the company, and
            highlights top performing regions</td>
            <td>Embed a QuickSight dashboard into the
            company's public web page that can be
            accessed via mobile platforms as well as
            browser</td></tr>
            <tr><td>Operations manager needs an end of week
            summary of operations research reports by region
            </td>
            <td>Send static report via email link to a
            location on an S3 bucket</td></tr>
            <tr><td>Data scientists need to visualize the results
            of machine learning models that use
            product data to predict potential supply
            chain issues based on the previous 6
            months of data</td>
            <td>Use EMR Notebooks on EMR</td></tr>
          </table>
      </div> 
## Security
- ### Select appropriate authentication and authorization mechanisms
  - ### _IAM Authentication_
    - To set permissions for an identity in IAM, choose an AWS managed policy, a customer managed policy, or an inline policy
      - **AWS managed policy**
        - Standalone policy that is created and administered by AWS
        - Provide permissions for many common use cases
          - Full
          - Power-user
          - Partial Access
        - Use Case
          - Best used when you are dealing with a service that has typical use cases that can be covered by AWS manged policy because it takes care of maintenance of the policy
      - **Customer Managed Policy**
        - Standalone policies that you administer in your AWS account
        - Use Case
          - When you have something that you would like to manage using AWS managed policy but you have your own customization that you would like to make
      - **Inline Policy**
        - Strict one-to-one relationship of policy to identity
          - Policy embedded in an IAM identity (a user, group, or role)
        - Use Case
          - Very targeted policy on a specific user or group or a role that you can assign to a service such as Athena or S3
    <div class="table-container">
        <table>
          <tr><th style="font-weight: bold">Services</th><th>Action</th><th>Resource level Permissions</th><th>Resource Based Policies</th><th>Auth Based on Tags</th><th>Temp Creds</th><th>Service-Linked Roles</th></tr>
          <tr><td style="font-weight: bold">Athena</td><td>✅</td><td>✅</td><td>❌</td><td>✅</td><td>✅</td><td>❌</td></tr>
          <tr><td style="font-weight: bold">Opensearch</td><td>✅</td><td>✅</td><td>✅</td><td>❌</td><td>✅</td><td>✅</td></tr>
          <tr><td style="font-weight: bold">EMR</td><td>✅</td><td>✅</td><td>❌</td><td>✅</td><td>✅</td><td>✅</td></tr>
          <tr><td style="font-weight: bold">Glue</td><td>✅</td><td>✅</td><td>✅</td><td>🔶</td><td>✅</td><td>❌</td></tr>
          <tr><td style="font-weight: bold">Kinesis Analytics</td><td>✅</td><td>✅</td><td>❌</td><td>✅</td><td>✅</td><td>❌</td></tr>
          <tr><td style="font-weight: bold">Kinesis Firehose</td><td>✅</td><td>✅</td><td>❌</td><td>✅</td><td>✅</td><td>❌</td></tr>
          <tr><td style="font-weight: bold">Kinesis Streams</td><td>✅</td><td>✅</td><td>❌</td><td>❌</td><td>✅</td><td>❌</td></tr>
          <tr><td style="font-weight: bold">Quicksight</td><td>✅</td><td>✅</td><td>❌</td><td>✅</td><td>✅</td><td>❌</td></tr>
        </table>
    </div>
      - ✅ = Yes
      - ❌ = No
      - 🔶 = Partial
    - ### _IAM Authorization_
      - Use IAM identity and resource-based permission to authorize access to analytics resources
      - Policy is an object in AWS you associate with an identity or resource to define the identity or resource permissions
      - To use a permissions policy to restrict access to a resource you choose a policy type
        - Identity based policy
          - Attached to an IAM user, group, or role
          - Specify what an identity can do (its permissions)
          - Example: attach a policy to a user that allows that user to perform the EC2 `RunInstances` action
        - Resource based policy
          - Attached to a resource
          - Specify which users have access to the resource and what actions they can perform on it
          - Example: attach resource-based policies to S3 buckets, SQS queues, and Key Management Service encryption keys

      - Control access/authorization using IAM policies
      - Encryption in flight using HTTPS endpoints
      - Encryption at rest using KMS
      - Client side encryption must be manually implemented
      - VPC Endpoints available for Kinesis to access within VPC        
        {:refdef: style="text-align: center;"}
        ![Image]({{site.baseurl}}/images/aws_das_1.20.jpg)
        {:refdef}
    
  - ### _Network Security_
  - Need to secure the physical boundary of your analytics services using network isolation
  - Use VPC to achieve network isolation
  - Example: EMR cluster where the master, core, and task nodes are in a private subnet using NAT to perform outbound only internet access
  - Also, use security groups to control inbound and outbound access from your individual EC2 instances
  - With EMR use both EMR-managed security groups and additional security groups to control network access to your instance
    {:refdef: style="text-align: center;"}
    ![Image]({{site.baseurl}}/images/aws_das_1.21.jpg)
    {:refdef}
  
  - ### _EMR - Managed Security Groups_
    - Every EMR cluster has managed security groups associated with it, either default or custom
    - EMR automatically adds rules to managed security groups used by the cluster to communicate between cluster instances and AWS services
    - Rules that EMR creates in managed security groups allow the cluster to communicate among internal components
    - To allow users and applications to access a cluster from outside the cluster, edit rules in managed security groups
      - Editing rules in managed security groups may have unintended consequences; may inadvertently block the traffic required for clusters to function properly
    - Specify security groups only on cluster create, can't add to a cluster or cluster instances while a cluster is running
    - Can edit, add and remove rules from existing security groups, the rules take effect as soon as you save them
    - There are two options
      - Master node
      - Cluster node (core node or task node)
    
  - ### _EMR - Additional Security Groups_
    - Additional security groups are optional
    - Specify in addition to manage security groups to tailor access to cluster instances
    - Contain only rules that you define, EMR does not modify them
    - To allow users and applications to access a cluster from outside the cluster, create additional security groups with additional rules
    
  - ### _EMR - VPC Options - S3 Endpoints & NAT Gateway_
    - S3 Endpoints
      - All instances in a cluster connect to S3 through either a VPC endpoint or internet gateway
      - Create a private endpoint for S3 in your subnet to give your EMR cluster direct access to data in Amazon S3
    - NAT Gateway
      - Other AWS services which do not currently support VPC endpoints use only an internet gateway

  - ### _Direct Connect (DX)_
    - Provides a dedicated private connection from a remote network to your VPC
    - Dedicated connection must be setup between your DC and AWS Direct Connection locations
    - You need to set up a Virtual Private Gateway on your VPC
    - Access public resources (S3) and private (EC2) on same connection
    - Use Cases:
      - Increase bandwidth throughput - working with large data sets - lower costs
      - More consistent network experience - application using real-time data feeds
      - Hybrid environments (on prem + cloud)
    - Supports both IPv4 and IPv6
    - Connection Types:
      - Dedicated Connections: 1 GB/sec, 10GB/sec
        - Physical ethernet port dedicated to a customer
      - Hosted Connections: 50 MB/sec, 500 MB/sec, 10GB/sec
        - Capacity can be added or removed on demand
      - Encryption
        - Data in transit is not encrypted but is private
        - Direct Connect + VPN provides an IPsec-encrypted private connection
      - Resiliency
        - High resiliency for critical workloads - consists of a second separate backup connection
        - Maximum resiliency for critical workloads - each direct location has to two independent locations (total 4)
      {:refdef: style="text-align: center;"}
      ![Image]({{site.baseurl}}/images/aws_das_1.27.jpg)
      {:refdef}

  - ### _QuickSight Security_
    - VPC connectivity
      - Add QuickSight's IP address range to your database security groups
    - Row level/Column level security
      - Enterprise edition only
    - Private VPC access
    - Quicksight + Redshift
      - By default, Quicksight can only access data stored in the same region as the one quicksight is running within
      - If you have QuickSight in one region, and Redshift in another, that won't work
      - Solution: Create a new security group with an inbound rule authorizing access from the IP range of QuickSight servers in that region

  - ### _Kinesis Security_
    - Kinesis Data Streams
      - SSL endpoints using the HTTPS protocol to do encryption in flight
      - AWS KMS provides server-side encryption (encryption at rest)
      - For client side encryption, you must use your own encryptoin libraries
      - Supported interface VPC Endpoints / Private Link
      - KCL - must get read/write access to DynamoDB table (it uses it for checkpointing)
    - Kinesis Data Firehose
      - Attach IAM roles so it can deliver to S3 / OS / Redshift / Splunk
      - Can encrypt the delivery stream with KMS (server-side encryption)
      - Supported interface VPC Endpoints / Private Link
    - Kinesis Data Analytics
      - Attach IAM role so it can read from Kinesis Data Streams and reference sources and write to an output destination (ex. Kinesis Data Firehose)

  - ### _SQS Security_
    - IAM policy must allow usage of SQS
    - SQS queue access policy

  - ### _S3 Security_
    - IAM policies
    - S3 bucket policies
    - Access Control Lists (ACLs)
    - Encryption at flight using HTTPS
    - Encryption at rest
    - Versioning + MFA delete
    - CORS for protecting websites
    - VPC Endpoint is provided through a Gateway
    - Glacier - vault lock policies to prevent deletes (WORM)

  - ### _RDS/Aurora Security_
    - VPC provides network isolation
    - Security Groups control network access to DB instances
    - KMS provides encryption at rest
    - SSL provides encryption in-flight
    - Must manage user permissions within the database itself

  - ### _Lambda Security_
    - IAM roles attach to each Lambda function
    - Lambda will pull data from sources and will send data to targets. The IAM roles attached to your Lambda functions will help you decide what your lead the function can do in terms of sources and targets

  - ### _Glue Security_
    - Configure Glue to only access JDBC through SSL
    - Data Catalogue
      - Encrypted by KMS
      - Resource policies to protect Data Catalog resources
      - Connection password: encrypted by KMS
      - Data written by Glue can be encrypted

  - ### _QuickSight Security_
    - Standard edition
      - IAM users
      - Email based accounts
    - Enterprise edition
      - Active Directory
      - Federated Login
      - Supports MFA
      - Encryption at rest and in SPICE
    - row level security

  - ### _Security Token Service (STS)_
    - Allows the granting of limited and temporary access to AWS resources
    - Token valid for up to one hour (must be refreshed)
    - Usage
      - Cross account access
      - Federation (active directory)
        - Provides a non-AWS user with temporary AWS access by linking users Active Directory
        - Uses SAML
        - Allows SSO
      - Federation with third party providers / Cognito
        - Used mainly in web and mobile applications
        - Makes use of Facebook/Google/Amazon etc. to federate them

  - ### _Identity Federation_
    - Federation lets users outside of AWS to assume a temporary role for accessing AWS resources
    - These users assume identity provided access role
    - Federation assumes a form of 3rd party authentication
      - LDAP
      - Microsoft Active Directory (SAML)
      - Single Sign On
      - Open ID
      - Cognito
    - SAML Federation for Enterprises
      - To integrate Active Directory / ADFS with AWS
      - Provides access to AWS Console or CLI (through temporary credentials)
      - No need to create an IAM user for each of your employees
    - Custom Identity Broker Application for Enterprises
      - Use only if identity provider is not compatible with SAML 2.0
      - The identity broker must determine the appropriate IAM policy
    - Cognito
      - Provides direct access to AWS resources from the client side

- ### Apply data protection and encryption techniques
  - ### _Encryption and Tokenization_
    - Protect data against data exfiltration (unauthorized copying and/or transferring data) and unauthorized access
    - Different ways to protect data
      - Use a hardware security module (HSM) to manage the top-level encryption keys
        - Use HSM if you require your keys to be stored in a dedicated, third-party validated hardware security modules under your exclusive control
        - If you need to comply with FIPS 140-2
        - If you need high-performance in-VPC cryptographic acceleration
        - If you need a multi-tenant, managed service that allows you to use and manage encryption keys
      - Encryption at rest & transit
        - Prevent unauthorized users from reading data on a cluster and associated data storage systems
      - Alternative to encryption to secure highly sensitive subsets of data for compliance purposes
      - Secrets management
        - Use secrets in application code to connect to databases, API, and other resources
        - Provides rotation, audit, and access control
      - Systems Manager Parameter Store
        - Centralized store to manage configuration data
        - Plain-text data such as database strings or secrets such as passwords
        - Does not rotate parameter stores automatically
  - ### _Types of Encryption_
    - SSE-S3
      - Keys are handled and managed by Amazon
      - Object is encrypted server side
      - AES-256 encryption type
    - SSE-KMS
      - Keys are handled and managed by KMS
      - KMS advantage: user control + audit trail
      - Object is encrypted server side
    - SSE-C
      - Server-side encryption using data keys fully managed by the customer outside of AWS
      - Amazon S3 does not store the encryption key you provide
      - HTTPS must be used
    - Client Side Encryption
      - You encrypt/decrypt the object yourself before uploading

  - ### _Key Management Service (KMS)_
    - Easy way to control access to your data, AWS manages keys and encryption
    - Fully integrated with IAM
    - The value in KMS is that the CMK used to encrypt data can never be retrived by the user
    - types of CMK
      - AWS managed service default CMK
      - User keys created in KMS
      - User keys imported
    - Automatic Key Rotation
      - For Customer managed CMK
      - if enabled, key rotation happens every 1 year
      - Previous key is kept active so you can decrypt old data
      - New key has the same CMK ID (only the backing key is changed)
    - Manual Key Rotation
      - New key has a different CMK ID

  - ### _CloudHSM_
    - AWS provisions dedicated physical encryption hardware
    - You manage your own encryption keys entirely
    - HSM device is tamper resistant

  - ### _EMR Encryption_
    - At rest data encryption for EMRFS
      - Encryption in S3
        - SSE-S3, SSE-KMS, Client Side encryption
      - Encryption in Local Disks
    - At rest data encryption for local disks
      - Open source HDFS encryption
      - EC2 instance store encryption
        - NVMe encryption or LUKS encryption (physically)
      - EBS Volumes
        - EBS encryption (KMS) - works with root volume
        - LUKS encryption - does not work with root
    - In transit encryption
      - Node to node communication
      - for EMRFS traffic between S3 and cluster nodes
      - TLS encryption
  
      {:refdef: style="text-align: center;"}
      ![Image]({{site.baseurl}}/images/aws_das_1.29.jpg)
      {:refdef}

  - ### _VPC Endpoints_
    - Endpoints allow you to connect to AWS Services using a private network instead of the public 'www' network
    - SQS is a public service you can access it from your local computer. It is accessible on the worldwide web but say you wanted to access it from within an EC2 instance on a private subnet. One way would be to give internet access to that EC2 instance but that would be a bit tricky because you need to create a public subnet, you need a new gateway all that stuff or you can just create a VPC endpoint also called private link which basically has a private connection directly into the SQS service and then to connect to the SQS service. Simple as that your EC2 instance will connect directly into the VPC end point
    - Gateway Endpoint
      - Provisions a target must be used in a route table - only S3 and DynamoDB
    - Interface (VPC PrivateLink)
      - Provisions an ENI (private IP address) as an entry point (must attach security group) - most AWS services

- ### Apply data governance and compliance controls
  - ### _Compliance and Governance_
    - Identify required compliance frameworks (HIPAA, PIC, etc.)
    - Understand contractual and agreement obligations
    - Monitor policies, standards, and security controls to respond to events and changes in risk
    - Services to create compliant analytics solution
      - **AWS Artifact**
        - Provides on-demand access to AWS compliance and security related information
        - self-service document retrieval portal
        - Manage AWS agreements
        - Provide security controls documentation
        - Download AWS security and compliance documents
      - **CloudTrail and CloudWatch** 
        - Enable governance, compliance, operational auditing, and risk auditing of AWS account
        - CloudTrail tracks actions in your AWS account by recording API activity
        - CloudWatch monitors how resources perform, collecting application metrics, and log information
        - Simplify security analysis, resource change tracking, and troubleshooting by combining event history via CloudWatch
        - Use CloudTrail to audit and review API calls and detect security anomalies
        - Use CloudWatch to create alert rules that trigger SNS notifications to a security or risk event
        - If a resource is deleted in AWS, look into CloudTrail first
        - Only shows 90 days past of activity
        - CloudTrail Trails allow you to get a detailed list of all the events you choose
      - **AWS Config** 
        - Ensure AWS resources conform to your organization's security guidelines and best practices
        - AWS resource inventory, configuration history, and configuration change notifications that enable security and governance
        - Logs in CloudWatch
        - Discover existing AWS resources
        - Export complete inventory of account AWS resources with all configuration details
        - Determine how a resource was configured at any point in time
        - Config Rules: represent desired configuration for a resource and is evaluated against configuration changes on the relevant resources, as recorded by AWS Config
        - Assess overall compliance and risk status from a configuration perspective, view compliance trends over time and pinpoint which configuration change caused by a resource to drift out of compliance with a rule

- ### AWS Service Integrations
  - ### _Kinesis Streams - Producers_
  {:refdef: style="text-align: center;"}
  ![Image]({{site.baseurl}}/images/aws_das_1.23.jpg)
  {:refdef}
  - ### _Kinesis Streams - Consumers_
  {:refdef: style="text-align: center;"}
  ![Image]({{site.baseurl}}/images/aws_das_1.24.jpg)
  {:refdef}
  - ### _Kinesis Firehose - Sources_
  {:refdef: style="text-align: center;"}
  ![Image]({{site.baseurl}}/images/aws_das_1.25.jpg)
  {:refdef}
  - ### _Kinesis Firehose - Destinations_
  {:refdef: style="text-align: center;"}
  ![Image]({{site.baseurl}}/images/aws_das_1.26.jpg)
  {:refdef}
  - ### _Kinesis Data Analytics - Sources_
  {:refdef: style="text-align: center;"}
  ![Image]({{site.baseurl}}/images/aws_das_1.30.jpg)
  {:refdef}
  - ### _Kinesis Data Analytics - Destinations_
  {:refdef: style="text-align: center;"}
  ![Image]({{site.baseurl}}/images/aws_das_1.31.jpg)
  {:refdef}
  - ### _SQS - Destinations and Sources_
  {:refdef: style="text-align: center;"}
  ![Image]({{site.baseurl}}/images/aws_das_1.32.jpg)
  {:refdef}
  - ### _DynamoDB - Destinations and Sources_
  {:refdef: style="text-align: center;"}
  ![Image]({{site.baseurl}}/images/aws_das_1.33.jpg)
  {:refdef}
  - ### _OpenSearch - Sources_
  {:refdef: style="text-align: center;"}
  ![Image]({{site.baseurl}}/images/aws_das_1.34.jpg)
  {:refdef}
  - ### _Athena - Destinations and Sources_
  {:refdef: style="text-align: center;"}
  ![Image]({{site.baseurl}}/images/aws_das_1.35.jpg)
  {:refdef}

- ### Notes
  - The entire security in DynamoDB is managed through IAM, we don't need to create users within DynamoDB
  - Amazon EMR supports multiple master nodes to enable high availability for EMR applications. Launch an EMR cluster with three master nodes and support high availability applications like YARN Resource Manager, HDFS Name Node, Spark, Hive, and Ganglia. EMR clusters with multiple master nodes are not tolerant of Availability Zone failures. In the case of an Availability Zone outage, you lose access to the EMR cluster.
  - Archive objects that are queried by S3 Glacier Select must be formatted as uncompressed comma-separated values (CSV).
  - Duplicate records in Kinesis can be the result of a network-related timeout or a change in the number of shards
  - `ExpiredIteratorExceptions`: If the shard iterator expires immediately, before you can use it, this might indicate that the DynamoDB table used by Kinesis does not have enough capacity to store the lease data. This situation is more likely to happen if you have a large number of shards. To solve this problem, increase the write capacity units assigned to the shard table
  - A large number of small files in S3 will slow down reads from Athena, but splitting a  large file will help loading to Redshift in performance. Having one large file will load in serialized manner which lowers performance
  - Kinesis Data Streams records are available to be read immediately after they are written. There are some use cases that need to take advantage of this and require consuming data from the stream as soon as it is available. You can significantly reduce the propagation delay by overriding the KCL default settings to poll more frequently, as shown in the following examples
  - `MSCK REPAIR TABLE`: compares the partitions in the table metadata and the partitions in S3. If new partitions are present in the S3 location that you specified when you created the table, it adds those partitions to the metadata and to the Athena table
  - Amazon S3 buckets can support 3,500 `PUT`/`COPY`/`POST`/`DELETE` or 5,500 `GET`/`HEAD` requests per second per partitioned prefix. Every partition prefix gets additional support and that is why it is wise to add a prefix especially when there is a large set of data
  - Amazon Machine Image (AMI): Provides the information required to launch an instance
  - Access Control Lists (ACLs) provide important authorization controls Apache Kafka clusters data.


Once you pass, you'll earn the beautiful badge below! 

{:refdef: style="text-align: center;"}
![Badge]({{site.baseurl}}/images/data-analytics-specialty-badge.jpg)
*AWS DAS Badge*
{: refdef}