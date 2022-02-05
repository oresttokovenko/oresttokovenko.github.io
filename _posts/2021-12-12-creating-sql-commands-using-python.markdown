---
layout: post
title:  Creating SQL commands using Python
description:
date:   2022-02-05
image:  '/images/08.jpg'
tags:   [SQL, Database, Python]
---

# ️This Blog post is a WIP ⛔️

Deliverable:  If a client in the old database was labelled as ‘inactive’ please ensure that they are listed as an inactive client in the new database.

{% highlight python %}

# 1 = did not import correctly
# 0 = imported correctly
# -1 = did not import correctly


# active = new database
# ACTIVE = old database


def create_to_update_active(row):
if row["active"] == 1 and row["ACTIVE"] == "N":
val = 1
elif row["active"] == 1 and row["ACTIVE"] == "Y":
val = 0
else:
val = -1
return val

{% endhighlight %}