---
layout: post
title:  Running out of Space on your Machine? Try Clearing MySQL `binlog` Files!
description:
date:   2021-12-16
image:  '/images/07.jpg'
tags:   [SQL, Database]
---

One day, I tried to import a local copy of the production database, but my machine had no space left. I checked DaisyDisk and discovered that the /.msql/ folder contained huge amount of `.binlog` files.

{% highlight mysql %}
> mysql -u developer -h localhost -p db_prod < /home/../Users/oresttokovenko/Desktop/db_live-full-db-export-no-views_2021-07-21_17_10_10.sql

> ERROR 1114 (HY000) at line 17248: The table 'statements' is full

{% endhighlight %}

I decided to check the `binlog.` files via Terminal, and found them all, totalling about 40GB

{% highlight mysql %}

> mysql -u root -p

> SHOW BINARY LOGS;

+---------------+------------+-----------+
| Log_name      | File_size  | Encrypted |
+---------------+------------+-----------+
| binlog.000030 | 1074597650 | No        |
| binlog.000031 | 1073742397 | No        |
| binlog.000032 | 1074200584 | No        |
| binlog.000033 | 1074637673 | No        |
| binlog.000034 | 1074399487 | No        |
| binlog.000035 | 1073988797 | No        |
| binlog.000036 | 1074002396 | No        |
| binlog.000037 | 1073807807 | No        |
| binlog.000038 | 1073804101 | No        |
| binlog.000039 | 1073796629 | No        |
| binlog.000040 | 1074034112 | No        |
| binlog.000041 | 1074002365 | No        |
| binlog.000042 | 1074116849 | No        |
| binlog.000043 | 1074488730 | No        |
| binlog.000044 | 1073883958 | No        |
| binlog.000045 |  645382732 | No        |
| binlog.000046 | 1073881229 | No        |
| binlog.000047 | 1073947948 | No        |
| binlog.000048 | 1074280343 | No        |
| binlog.000049 | 1073795249 | No        |
| binlog.000050 | 1074125078 | No        |
| binlog.000051 | 1074388023 | No        |
| binlog.000052 | 1074168519 | No        |
| binlog.000053 | 1074135798 | No        |
| binlog.000054 | 1073822552 | No        |
| binlog.000055 | 1074243792 | No        |
| binlog.000056 | 1074531494 | No        |
| binlog.000057 | 1074545309 | No        |
| binlog.000058 | 1074651871 | No        |
| binlog.000059 | 1073959972 | No        |
| binlog.000060 | 1074152602 | No        |
| binlog.000061 | 1074242549 | No        |
| binlog.000062 | 1074506036 | No        |
| binlog.000063 | 1074287807 | No        |
| binlog.000064 | 1073819533 | No        |
| binlog.000065 | 1073888394 | No        |
| binlog.000066 | 1073769144 | No        |
| binlog.000067 | 1074638410 | No        |
| binlog.000068 | 1074153406 | No        |
| binlog.000069 | 1073970098 | No        |
| binlog.000070 | 1074539497 | No        |
| binlog.000071 | 1074519241 | No        |
| binlog.000072 | 1074259032 | No        |
| binlog.000073 | 1074408928 | No        |
| binlog.000074 | 1073920578 | No        |
| binlog.000075 | 1073929136 | No        |
| binlog.000076 |  901460870 | No        |
| binlog.000077 | 1073877986 | No        |
| binlog.000078 | 1073947919 | No        |
| binlog.000079 | 1074278107 | No        |
| binlog.000080 | 1073794310 | No        |
| binlog.000081 | 1074124862 | No        |
| binlog.000082 | 1074388023 | No        |
| binlog.000083 | 1074168519 | No        |
| binlog.000084 | 1074135798 | No        |
| binlog.000085 |  741190991 | No        |
+---------------+------------+-----------+
{% endhighlight %}

I decided to remove them, to free up the space. To do so I did the following:

{% highlight mysql %}
> PURGE BINARY LOGS BEFORE NOW();

> SHOW BINARY LOGS;
+---------------+-----------+-----------+
| Log_name      | File_size | Encrypted |
+---------------+-----------+-----------+
| binlog.000085 | 741190991 | No        |
+---------------+-----------+-----------+
{% endhighlight %}

Afterwards I successfully imported the copy of the database. Problem solved!