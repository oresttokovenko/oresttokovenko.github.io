---
layout: post
title:  Study Guide for GCP Professional Data Engineer Certification
date:   2023-04-15
image:  '/images/gcp_data_engineer.jpg'
tags:   [Cloud Computing, GCP, Study Guide]
---

This study guide covers GCP Professional Data Engineer Certification. The exam consists of 50-60 questions, and you have 120 minutes to write it. The study guide below covers everything you need to know for it. To study for this exam I did the following:

- Signed up for the Google Cloud Innovators Plus, and completed the Data Engineer Learning Path as well as used the $500 in GCP credits to build out some data processing projects
- Read through the Official Google Cloud Certified Professional Data Engineer Study Guide by Sybex
- Watched the *GCP Professional Data Engineer* Whizlabs Course
- Read the following whitepapers linked below
  - [Build a modern, unified analytics data platform with Google Cloud](https://cloud.google.com/resources/googlecloud-unified-analytics-data-platform-paper)
  - [Building a data lakehouse on Google Cloud](https://services.google.com/fh/files/misc/building-a-data-lakehouse.pdf)
  - [Building a unified analytics data platform](https://services.google.com/fh/files/misc/googlecloud_unified_analytics_data_platform_paper_2021.pdf)
  - [Enhance your investment research with Google Cloud](https://cloud.google.com/resources/investment-research-with-cloud-guide)
  - [SAP Analytics on BigQuery with Qlik Replicate](https://cloud.google.com/files/qlik_sap_bigquery_whitepaper.pdf)
  - [Transforming options market data with the Dataflow SDK](https://cloud.google.com/dataflow/pdf/TransformingOptionsMarketData.pdf)
  - [Data Vault 2.0 on Google Cloud BigQuery](https://services.google.com/fh/files/misc/google-cloud-data-vault_b.pdf)
  - [Building a data mesh on Google Cloud](https://services.google.com/fh/files/misc/build-a-modern-distributed-datamesh-with-google-cloud-whitepaper.pdf)
  - [What type of data processing organization are you?](https://services.google.com/fh/files/misc/dataprocessingorganisationwhitepaper.pdf)
  - [An insider’s guide to BigQuery cost optimization](https://services.google.com/fh/files/misc/19092_bigquery_best_practices_and_cost_optimization_whitepaper_v3_ca.pdf)
- Did the Google official [Professional Data Engineer Sample Questions](https://docs.google.com/forms/d/e/1FAIpQLSfkWEzBCP0wQ09ZuFm7G2_4qtkYbfmk_0getojdnPdCYmq37Q/viewform)
- Reviewed the [Google Blog Guide](https://www.googlecloudcommunity.com/gc/Community-Blogs/Your-guide-to-preparing-for-the-Google-Cloud-Professional-Data/ba-p/543105) for the Professional Data Engineer Certification

---

# _Study Guide_

# Section 1: Designing data processing systems
## 1.1 Selecting the appropriate storage technologies. Considerations include:
- Mapping storage systems to business requirements
- Data modeling
- Trade-offs involving latency, throughput, transactions
- Distributed systems
- Schema design

## 1.2 Designing data pipelines. Considerations include:
- Data publishing and visualization (e.g., BigQuery)
- Batch and streaming data (e.g., Dataflow, Dataproc, Apache Beam, Apache Spark and Hadoop ecosystem, Pub/Sub, Apache Kafka)
- Online (interactive) vs. batch predictions
- Job automation and orchestration (e.g., Cloud Composer)

## 1.3 Designing a data processing solution. Considerations include:
- Choice of infrastructure
- System availability and fault tolerance
- Use of distributed systems
- Capacity planning
- Hybrid cloud and edge computing
- Architecture options (e.g., message brokers, message queues, middleware, service-oriented architecture, serverless functions)
- At least once, in-order, and exactly once, etc., event processing

## 1.4 Migrating data warehousing and data processing. Considerations include:
- Awareness of current state and how to migrate a design to a future state
- Migrating from on-premises to cloud (Data Transfer Service, Transfer Appliance, Cloud Networking)
- Validating a migration

# Section 2: Building and operationalizing data processing systems
## 2.1 Building and operationalizing storage systems. Considerations include:
- Effective use of managed services (Cloud Bigtable, Cloud Spanner, Cloud SQL, BigQuery, Cloud Storage, Datastore, Memorystore)
- Storage costs and performance
- Life cycle management of data

## 2.2 Building and operationalizing pipelines. Considerations include:
- Data cleansing
- Batch and streaming
- Transformation
- Data acquisition and import
- Integrating with new data sources

## 2.3 Building and operationalizing processing infrastructure. Considerations include:
- Provisioning resources
- Monitoring pipelines
- Adjusting pipelines
- Testing and quality control

# Section 3: Operationalizing machine learning models
## 3.1 Leveraging pre-built ML models as a service. Considerations include:
- ML APIs (e.g., Vision API, Speech API)
- Customizing ML APIs (e.g., AutoML Vision, Auto ML text)
- Conversational experiences (e.g., Dialogflow)

## 3.2 Deploying an ML pipeline. Considerations include:
- Ingesting appropriate data
- Retraining of machine learning models (AI Platform Prediction and Training, BigQuery ML, Kubeflow, Spark ML)
- Continuous evaluation

## 3.3 Choosing the appropriate training and serving infrastructure. Considerations include:
- Distributed vs. single machine
- Use of edge compute
- Hardware accelerators (e.g., GPU, TPU)

## 3.4 Measuring, monitoring, and troubleshooting machine learning models. Considerations include:
- Machine learning terminology (e.g., features, labels, models, regression, classification, recommendation, supervised and unsupervised learning, evaluation metrics)
- Impact of dependencies of machine learning models
- Common sources of error (e.g., assumptions about data)

# Section 4: Ensuring solution quality (continued)
## 4.1 Designing for security and compliance. Considerations include (continued):
- Legal compliance (e.g., Health Insurance Portability and Accountability Act (HIPAA), Children's Online Privacy Protection Act (COPPA), FedRAMP, General Data Protection Regulation (GDPR))

## 4.2 Ensuring scalability and efficiency. Considerations include:
- Building and running test suites
- Pipeline monitoring (e.g., Cloud Monitoring)
- Assessing, troubleshooting, and improving data representations and data processing infrastructure
- Resizing and autoscaling resources

## 4.3 Ensuring reliability and fidelity. Considerations include:
- Performing data preparation and quality control (e.g., Dataprep)
- Verification and monitoring
- Planning, executing, and stress testing data recovery (fault tolerance, rerunning failed jobs, performing retrospective re-analysis)
- Choosing between ACID, idempotent, eventually consistent requirements

## 4.4 Ensuring flexibility and portability. Considerations include:
- Mapping to current and future business requirements
- Designing for data and application portability (e.g., multicloud, data residency requirements)
- Data staging, cataloging, and discovery

Once you pass, you'll earn the beautiful badge below! 

{:refdef: style="text-align: center;"}
![Badge]({{site.baseurl}}/images/cloud-certification_badge-data-engineer.jpg)
*GCP Professional Data Engineer*
{: refdef}