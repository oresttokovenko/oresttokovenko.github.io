---
layout: post
title:  Best Python libraries for Data Science and Data Engineering
description:
date:   2021-12-18
image:  '/images/best_python_libraries.jpg'
tags:   [python, pandas]
---

# ⛔️ ️This Blog post is a WIP ⛔️

### pandas/numpy
  The bread and butter of Data Science and Data Engineering. Pandas allows for easy manipulation, cleaning and organization of tabular data, while NumPy allows you to vectorize commands against Pandas DataFrames, such as using `np.where` instead of simple for loops. 
### difflib/fuzzywuzzy
  This package is used for string matching
### pandas_profiling
  pandas_profiling is a neat little package that returns an HTML report containing an analysis of your Pandas dataframe. This is useful when you want to scope out a file that you're not familiar with, to see if there are any oddities. It covers things like Missing values, most frequent values, some basic descriptive statistics, text analysis, etc.
### sqlalchemy
  SQLAlchemy is a supremey useful package, allowing you to pull data directly from a local database(not a live one unfortunately) into python. It's useful for situations when you need to reference data, for example modeling data to be loaded into a database, one can pull a table that refers to country codes.
  {% highlight python %}
  print('hello world')
  {% endhighlight %}
### glob
  The glob module finds all the pathnames matching a specified pattern according to the rules used by the Unix shell. Put simply, it is used to retrieve files/pathnames matching a certain pattern. This is useful when you want to import multiple csv files into a jupyter notebook without referencing them one by one. 
### datetime
  Often times you will be working with a column that contains a date but when you run df.info() on it, it will come back as a string dtype.
  {% highlight python %}
  df['trade_date'] = pd.to_datetime(df['trade_date'])
  {% endhighlight %}
### re
  This one is used in tandem with the Pandas `.contains()` function, to track down certain rows you might be interested in
  {% highlight python %}  
  df_accounts.loc[df_accounts['temp_account_id'].str.contains(r'^_')
  {% endhighlight %}
### black
  This one is pretty self explanatory, who doesn't want cleanly and PEP8 formatted code? You can run this from the command line to format individual files using `black [your_file_here]`, or you can add a config file to your git repository, it looks something like this:
  {% highlight yaml %}
    repos:
      - repo: https://github.com/oresttokovenko/sample_repo
        rev: 22.1.0
        hooks:
          - id: black
            language_version: python3.9
  {% endhighlight %}
