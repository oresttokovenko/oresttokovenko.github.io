---
layout: post
title:  Setting up Cloud Infrastructure on GCP and AWS with Terraform
description:
date:   2022-02-15
image:  '/images/09.jpg'
tags:   [Terraform, IaC, GPC, AWS, Cloud Computing]
---

# ️This Blog post is a WIP ⛔️

**What is Terraform?**

Terraform is an open-source tool by HashiCorp, used for provisioning infrastructure resources. It supports DevOps best practices for change management and managing configuration files in source control to maintain an ideal provisioning state for testing and production environments

What is IaC?
IaC is 'Infrastructure-as-Code', which allows you to build, change, and manage your infrastructure in a safe, consistent, and repeatable way by defining resource configurations that you can version, reuse, and share. 

**Some advantages of IaC**
  - Infrastructure lifecycle management
  - Version control commits
  - Very useful for stack-based deployments, and with cloud providers such as AWS, GCP, Azure, Kubernetes
  - State-based approach to track resource changes throughout deployments

(this blog post omits the `variables.tf` file for brevity's sake)

**GCP Terraform Code**

The following code provisions a BigQuery table and a Data Lake in a Google Storage bucket, with bucket versioning enabled and a lifecycle rule where the bucket resource is deleted after 30 days. 

{% highlight Terraform %}
terraform {
  required_version = ">= 1.0"
  backend "gcs" {}
  required_providers {
    google = {
      source  = "hashicorp/google"
    }
  }
}

provider "google" {
  project = var.project
  region = var.region
  # credentials = file(var.credentials)
}

# Data Lake Bucket
resource "google_storage_bucket" "data-lake-bucket" {
  name          = "${local.data_lake_bucket}_${var.project}"
  location      = var.region

  storage_class = var.storage_class
  uniform_bucket_level_access = true

  versioning {
    enabled     = true
  }

  lifecycle_rule {
    action {
      type = "Delete"
    }
    condition {
      age = 30  # days
    }
  }

  force_destroy = true
}

# Data Warehouse
resource "google_bigquery_dataset" "dataset" {
  dataset_id = var.BQ_DATASET
  project    = var.project
  location   = var.region
}

{% endhighlight %}

**AWS Terraform Code**

The following code is for AWS and it provisions a Redshift table and a Data Lake in an S3 bucket

{% highlight Terraform %}
terraform {
  required_version = ">= 1.0"
  backend "aws" {}
  required_providers {
    aws = {
      source  = "hashicorp/aws"
    }
  }
}
{% endhighlight %}

**Execution Steps**

So what do you once you have your Terraform files ready? The following are the execution steps

{% highlight bash %}
terraform init
{% endhighlight %}
Initializes & configures the backend, installs plugins/providers, & checks out an existing configuration from a version control

{% highlight bash %}
terraform plan
{% endhighlight %}
Matches/previews local changes against a remote state, and proposes an Execution Plan.

{% highlight bash %}
terraform apply
{% endhighlight %}
Asks for approval to the proposed plan, and applies changes to cloud

{% highlight bash %}
terraform destroy
{% endhighlight %}
Removes your stack from the Cloud