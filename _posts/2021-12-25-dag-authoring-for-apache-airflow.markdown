---
layout: post
title:  Study Guide for Apache Airflow Fundamentals Certification
description:
date:   22 December 2021
image:  '/images/04.jpg'
tags:   [placeholder]
---

### Astronomer Certification for Apache Airflow Fundamentals

Apache Airflow is the leading orchestrator for authoring, scheduling, and monitoring data pipelines.

The exam consists of 

### User Interface

* #### Graph View

  * The Graph View shows a visualization of the tasks and dependencies in your DAG and their current status for a specific DAG Run. This view is particularly useful when reviewing and developing a DAG. When running the DAG, you can toggle the Auto-refresh button to on to see the status of the tasks update in real time.

* #### Tree View

  * The Tree View shows a tree representation of the DAG and its tasks across time. Each column represents a DAG Run and each square is a task instance in that DAG Run. Task instances are color-coded according to their status. DAG Runs with a black border represent scheduled runs, whereas DAG Runs with no border are manually triggered.

* #### Code View

  * The Code View shows the code that is used to generate the DAG. While your code should live in source control, the Code View can be a useful way of gaining quick insight into what is going on in the DAG. Note that code for the DAG cannot be edited directly in the UI.

* #### Gantt View

  * Allows you to analyze task duration as well as overlaps. Check if the tasks are running in parallel. 

### Core Concepts
* #### Dags
  * DAG (Directed Acyclic Graph) is the core concept of Airflow, collecting Tasks together, organized with dependencies and relationships to say how they should run.
    * Here’s a basic example DAG:
      * It defines four Tasks - A, B, C, and D - and dictates the order in which they have to run, and which tasks depend on what others. It will also say how often to run the DAG - maybe “every 5 minutes starting tomorrow”, or “every day since January 1st, 2020”. 
      * The DAG itself doesn’t care about what is happening inside the tasks; it is merely concerned with how to execute them - the order to run them in, how many times to retry them, if they have timeouts, and so on
* #### DAG Runs
  * DAGs will run in one of two ways:
    * When they are triggered either manually or via the API 
    * On a defined schedule, which is defined as part of the DA
* #### Tasks
  * A Task is the basic unit of execution in Airflow. Tasks are arranged into DAGs, and then have upstream and downstream dependencies set between them into order to express the order they should run in. 
    * There are three basic kinds of Task:
      * Operators, predefined task templates that you can string together quickly to build most parts of your DAGs. 
      * Sensors, a special subclass of Operators which are entirely about waiting for an external event to happen. 
      * A TaskFlow-decorated @task, which is a custom Python function packaged up as a Task.
* #### Operators
  * An operator represents a single, ideally idempotent, task. Operators determine what actually executes when your DAG runs. 
    * Types of Operators 
      * Action Operator 
      * Transfer Operator 
      * Sensor Operator

***Scheduling
    1. The scheduler "taps" the dags folder and instantiates all DAG objects in the metadata databases. Depending on the configuration, each DAG gets a configurable number of processes. Note: This means all top level code (i.e. anything that isn't defining the DAG) in a DAG file will get run each scheduler heartbeat. Try to avoid top level code to your DAG file unless absolutely necessary.
    2. Each process parses the DAG file and creates the necessary DagRuns based on the scheduling parameters of each DAG's tasks. A TaskInstance is instantiated for each task that needs to be executed. These TaskInstances are set to Scheduled in the metadata database.
    3. The primary scheduler process queries the database for all tasks in the SCHEDULED state and sends them to the executors (with state changed to QUEUED).
    4. Depending on the execution setup, workers will pull tasks from the queue and start executing it. Tasks that are pulled off of the queue are changed from "queued" to "running."
    5. If a task finishes, the worker then changes the status of that task to its final state (finished, failed, etc.). The scheduler then reflects this change in the metadata database.
***Backfill & Catchup
    - Note: Both are for rerunning tasks
    - Catchup
        - An Airflow DAG with a start_date, possibly an end_date, and a schedule_interval defines a series of intervals which the scheduler turns into individual DAG Runs and executes. The scheduler, by default, will kick off a DAG Run for any data interval that has not been run since the last data interval (or has been cleared). This concept is called Catchup.
        - If your DAG is not written to handle its catchup (i.e., not limited to the interval, but instead to Now for instance.), then you will want to turn catchup off. This can be done by setting catchup = False in DAG or catchup_by_default = False in the configuration file. When turned off, the scheduler creates a DAG run only for the latest interval.
    - Backfill
        - Rerun past DAG run. There can be the case when you may want to run the DAG for a specified historical period e.g., A data filling DAG is created with start_date 2019-11-21, but another user requires the output data from a month ago i.e., 2019-10-21. This process is known as Backfill.
        - You may want to backfill the data even in the cases when catchup is disabled. This can be done through CLI. Run the below command
***Architectural Components
    - Apache Airflow consists of 4 core components:
        - Webserver: Airflow's UI.
            - At its core, this is just a Flask app that displays the status of your jobs and provides an interface to interact with the database and reads logs from a remote file store (S3, Google Cloud Storage, AzureBlobs, ElasticSearch etc.).
        - Scheduler: Responsible for scheduling jobs.
            - This is a multi-threaded Python process that uses the DAG object with the state of tasks in the metadata database to decide what tasks need to be run, when they need to be run, and where they are run.
        - Executor: how each task will be executed by Airflow
            - There are several executors, each with strengths and weaknesses.
        - Metadata Database A database (usually PostgresDB or MySql, but can be anything with SQLAlchemy support) that determines how the other components interact.
            - The scheduler stores and updates task statuses, which the Webserver then uses to display job information

***Use Cases
    - Airflow can be used for virtually any batch data pipelines, and there are a ton of documented use cases in the community. Because of its extensibility, Airflow is particularly powerful for orchestrating jobs with complex dependencies in multiple external systems.
***To Know
    - Parallelism = 32 (max)
    - dag_concurrency = 16 (max)
    - max_active_runs_per_dag = 16 (max)
    - Types of executors
        - Local executor
        - Sequential executor
        - Celery executor
    - Airflow is an orchestrator
    - Dynamic, Scalable, Interactive
    - NOT a streaming or data processing framework (like Spark)
