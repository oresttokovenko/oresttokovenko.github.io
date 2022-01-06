---
layout: post
title:  Study Guide for GitLab Certified Associate Certification
description:
date:   30 December 2021
image:  '/images/gitlab_associate.jpg'
tags:   [Git, Gitlab, Study Guide]
---

This study guide covers the GitLab Certified Associate Certification. It is a technical certification offered by GitLab Professional Services to help validate individuals' ability to apply GitLab and Git in Version Control. To earn this certification, candidates must first pass a 1-hour written assessment, followed by a 2-hour hands-on lab assessment graded by GitLab Professional Services engineers.

There are two parts, a multiple-choice selection on a set of 14 questions, with 80% accuracy required to move on to the next stage that can be retaken indefinitely, and a hands-on set of 14 tasks which must be completed on a project you create.

The exam is generally things about GitLab which you’ve covered in the course, but included two questions about using Git that were not covered in any of the modules. For this reason, I’d suggest when you get to those questions, open a git environment, and try each of the commands offered given the specific scenario.

---

### _Study Guide_

### Git Basics
* #### Git Term Definitions
  * **Branch**
    * A branch is an independent line of development
  * **Tag**
    * Mark a specific point in time on a branch.
  * **Checkout**
    * Get a specific branch to start making your changes.
  * **Commit**
    * Add changes you've made to the repository.
  * **Push**
    * Send changes to a remote directory.
  * **Workspace**
    * Directory where you store the repository on your computer.
  * **Untracked Files**
    * New files that Git has not been told to track previously
  * **Working Area**
    * Files that have been modified but not committed
  * **Staging Area**
    * Modified/Added files that are marked to go into the next commit
  * **Local Repo**
    * Local copy of the upstream repo
  * **Remote/Upstream Repo**
    * Hosted repository on a shared server (GitLab)
* #### Centralized vs. Distributed
  * **It is safer for data loss**:
    * Each developer has a clone from the repo 
    * Even a CI server has a trustable source of backup
  * **It is faster**:
    * Most of the operations doesn’t require network 
    * It's less bureaucratic- you can create branches, commits, and merges without accessing the remote repository
  * **More collaborative**:
    * You can have several remote repositories
    * Allows you collaborate with different developers without changing the official repository (using forks for example)
* #### GitLab Cheat Sheets
{:refdef: style="text-align: center;"}
![Badge]({{site.baseurl}}/images/git-cheat-sheet-1.jpg)
![Badge]({{site.baseurl}}/images/git-cheat-sheet-2.jpg)
![Badge]({{site.baseurl}}/images/git-cheat-sheet-3.jpg)
![Badge]({{site.baseurl}}/images/git-cheat-sheet-4.jpg)
{: refdef}

* #### Typical Workflow
{:refdef: style="text-align: center;"}
![Badge]({{site.baseurl}}/images/typical_workflow.jpg)
{: refdef}

* #### Code Review Workflow- GitLab Tools to Use
  * **New Merge Request**
    * Edit files inline on branch 
    * Commit Changes 
      * Squash locally 
      * Or one commit per file in GUI
    * Merge Request 
    * Resolve merge conflicts
  * **Assign & Review**
    * Assignments
    * Changes tab to view differences 
    * Edit Inline 
    * Make comments inline 
    * Discussion tab to view comments
  * **Final Assignment**
    * Assignments feature in GitLab
  * **Final Review**
    * Assignments 
    * Changes tab to view differences 
    * Edit Inline 
    * Make comments inline 
    * Discussion tab to view comments
  * **Merged Approved**
    * Approval feature in GitLab.
  * **Additional Tools for Code Review**
    * Wiki
    * Web IDE 
    * Snippets

* #### Additional Tools Used
  * **Snippets**
    * With GitLab Snippets you can store and share bits of code and text with other users.
  * **Wiki**
    * A separate system for documentation called Wiki, is built right into each GitLab project. It is enabled by default on all new projects and you can find it under Wiki in your project. 
    * Wikis are very convenient if you don’t want to keep your documentation in your repository, but you do want to keep it in the same project where your code resides. 
    * You can create Wiki pages in the web interface or locally using Git since every Wiki is a separate Git repository.
  * **Web IDE**
    * The Web IDE editor makes it faster and easier to contribute changes to your projects by providing an advanced editor with commit staging.

* #### Defining CI/CD
  * Continuous Integration is the practice of merging all the code that is being produced by developers. The merging usually takes place several times a day in a shared repository. From within the repository, or production environment, building and automated testing are carried out that ensure no integration issues and the early identification of any problems.
  * Continuous Delivery adds that the software can be released to production at any time, often by automatically pushing changes to a staging system. 
  * Continuous Deployment goes further and pushes changes to production automatically.
  * Together, CI and CD act to accelerate how quickly your team can deliver results for your customers and stakeholders. CI helps you catch and reduce bugs early in the development cycle, and CD moves verified code to your applications faster.

* #### Benefits of GitLab CI/CD
  * **Error Detection**
    * Detects errors as quickly as possible: fix problems while they are still fresh in developers mind.
  * **Increased Efficiency**
    * Reduces integration problems: smaller problems are easier to digest and fix immediately. Bugs won't shut your whole system down.
  * **No Snowball Effect**
    * Avoid compounding problems: allows teams to develop faster, with more confidence and collaboration.
  * **Release Stages**
    * Ensures every change is releasable: test everything, including deployment, before calling it done with less risk on each release.
  * **Valuable Delivery**
    * Delivers value more frequently: reliable deployments mean more releases
  * **Better Feedback Processes**
    * Tight customer feedback loops: fast and frequent customer feedback on changes allow for continuous improvement for your product.

* #### CI/CD Features in GitLab- by Lifecycle Stage
{:refdef: style="text-align: center;"}
![Badge]({{site.baseurl}}/images/gitlab_lifecycle_stage.jpg)
{: refdef}
  * To use GitLab CI/CD you or your GitLab administrator must first define a pipeline within a YAML file called .gitlab-ci.yml and then install and configure a Gitlab Runner.
    * The YAML file is the pipeline definition file. It specified the stages, jobs, and actions that you want to perform. Think of the YAML file as the brains, and the runner as the body.
    * A GitLab Runner, a file written in Go, will run the jobs specified in the YAML file using an API to communicate with GitLab. Your GitLab administrator can configure shared runners to run on multiple projects, and you can set up your own by project.

* #### How GitLab CI/CD Works
{:refdef: style="text-align: center;"}
![Badge]({{site.baseurl}}/images/gitlab_ci_cd.jpg)
{: refdef}

* #### Anatomy of a CI/CD Pipeline
{:refdef: style="text-align: center;"}
![Badge]({{site.baseurl}}/images/anatomy_ci_cd.jpg)
{: refdef}

* #### Anatomy of a CI/CD Pipeline
  * **GitLab's Package Stage**
    * GitLab enables teams to package their applications and dependencies, manage containers, and build artifacts with ease. The private, secure container registry and artifact repositories are built-in and preconfigured out-of-the box to work seamlessly with GitLab source code management and CI/CD pipelines. Ensure DevOps acceleration with automated software pipelines that flow freely without interruption.
  * **Package Registry**
    * Every team needs a place to store their packages and dependencies. GitLab aims to provide a comprehensive solution, integrated into our single application, that supports package management for all commonly used languages and binary formats.
  * **Container Registry**
    * A container registry* is a secure and private registry for Docker images built-in to GitLab. Creating, pushing, and retrieving images works out of the box with GitLab CI/CD.
  * **Helm Chart Registry**
    * Kubernetes cluster integrations can take advantage of Helm charts to standardize their distribution and install processes. Supporting a built-in helm chart registry allows for better, self-managed container orchestration.
  * **Dependency Proxy**
    * The GitLab dependency proxy* can serve as an intermediary between your local developers and automation and the world of packages that need to be fetched from remote repositories. By adding a security and validation layer to a caching proxy, you can ensure reliability, accuracy, and audit-ability for the packages you depend on.
  * **Jupyter Notebooks**
    * Jupyter Notebooks are a common type of code used for data-science use cases. With GitLab you can store and version control those notebooks in the same way you store packages and application code.
  * **Git LFS**
    * Git LFS (Large File Storage) is a Git extension, which reduces the impact of large files in your repository by downloading the relevant versions of them lazily. Specifically, large files are downloaded during the checkout process rather than during cloning or fetching.

* #### GitLab's Release Stage
  * **Pages**
    * GitLab Pages is a feature that allows you to publish static websites directly from a repository in GitLab. You can use it either for personal or business websites, such as portfolios, documentation, manifestos, and business presentations. You can also attribute any license to your content.
  * **Review Apps**
    * Automatic Live Preview 
      * Code, commit, and preview your branch in a live environment. Review Apps automatically spin up dynamic environments for your merge requests. 
    * One-click to Collaborate 
      * Designers and product managers won't need to check out your branch and run it in a staging environment. Simply send the team a link and let them click around. 
    * Fully-Integrated
      * With GitLab's code review, built-in CI/CD, and Review Apps, you can speed up your development process with one tool for coding, testing, and previewing your changes. 
    * Deployment Flexibility 
      * Deploy to Kubernetes, Heroku, FTP, and more. You can deploy anywhere that you can script with .gitlab-ci.yml and you have full control to deploy as many different kinds of review apps as your team needs.
  * **Incremental Rollout**
    * When you have a new version of your app to deploy in production, you may want to use an incremental rollout to replace just a few pods with the latest code. This will allow you to first check how the app is behaving, and later manually increasing the rollout up to 100%.
  * **Feature Flags**
    * Feature flags enable teams to achieve CD by letting them deploy dark features to production as smaller batches for controlled testing, separating feature delivery from customer launch, and removing risk from delivery.
  * **Release Orchestration**
    * Management and orchestration of releases-as-code built on intelligent notifications, scheduling of delivery and shared resources, blackout periods, relationships, parallelization, and sequencing, as well as support for integrating manual processes and interventions.
  * **Release Evidence**
    * Release Evidence includes features such as deploy-time security controls to ensure only trusted container images are deployed on Kubernetes Engine, and more broadly includes all the assurances and evidence collection that are necessary for you to trust the changes you're delivering.
  * **Secrets Management**
    * Vault is a secrets management application offered by HashiCorp. It allows you to store and manage sensitive information such as secret environment variables, encryption keys, and authentication tokens. Vault offers Identity-based Access, which means Vault users can authenticate through several of their preferred cloud providers.

* #### GitLab Security Scanning
  * Integrating a security scanner into GitLab consists of providing end users with a CI job definition they can add to their CI configuration files to scan their GitLab projects. This CI job should then output its results in a GitLab-specified format. These results are then automatically presented in various places in GitLab, such as the Pipeline view, Merge Request widget, and Security Dashboard.
  * Static Application Security Testing (SAST)
    * If you’re using GitLab CI/CD, you can use Static Application Security Testing (SAST) to check your source code for known vulnerabilities. If the pipeline is associated with a merge request, the SAST analysis is compared with the results of the target branch’s analysis (if available). The results of that comparison are shown in the merge request. If the pipeline is running from the default branch, the results of the SAST analysis are available in the security dashboards.
    * The results are sorted by the priority of the vulnerability:
      * Critical
      * High
      * Medium
      * Low
      * Info 
      * Unknown
    * Use Cases:
      * Your code has a potentially dangerous attribute in a class, or unsafe code that can lead to unintended code execution. 
      * Your application is vulnerable to cross-site scripting (XSS) attacks that can be leveraged to unauthorized access to session data.
    * SAST features covered under GitLab Free
      * Configure SAST Scanners
      * Customize SAST Settings
      * View JSON Report
    * SAST supports the following analyzers
      * bandit (Bandit)
      * brakeman (Brakeman)
      * eslint (ESLint (JavaScript and React))
      * flawfinder (Flawfinder)
      * gosec (Gosec)
      * kubesec (Kubesec)
      * mobsf (MobSF (beta))
      * nodejs-scan (NodeJsScan)
      * phpcs-security-audit (PHP CS security-audit)
      * pmd-apex (PMD (Apex only))
      * security-code-scan (Security Code Scan (.NET))
      * semgrep (Semgrep)
      * sobelow (Sobelow (Elixir Phoenix))
      * spotbugs (SpotBugs with the Find Sec Bugs plugin (Ant, Gradle and wrapper, Grails, Maven and wrapper, SBT))




Once you pass, you'll earn the beautiful badge below! 

{:refdef: style="text-align: center;"}
![Badge]({{site.baseurl}}/images/gitlab_badge.jpg)
*GitLab Certified Associate Badge*
{: refdef}