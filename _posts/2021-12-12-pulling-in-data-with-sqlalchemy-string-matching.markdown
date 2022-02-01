---
layout: post
title:  Using SQLAlchemy and the `difflib` Library to String Match Data
description:
date:   2021-12-12
image:  '/images/sqlalchemy-string-matching.jpg'
tags:   [python, SQL, pandas]
---

#### _Disclaimer_: All proprietary company data has been completely anonymized and replaced with randomly generated counterparts, to respect the stipulations outlined in my non-disclosure agreement

# This Blog post is a WIP ⛔️

I had a situation where we needed to add transactions to a client's account on our software, and they provided us with transactions in an excel sheet. However, we already have their existing data in our database, with investor names matched to a certain id. Therefore, we need to avoid duplicates when uploading this new data. The solution is to pull a list of investors, their ids, 

I begin by importing the necessary libraries, establishing a connection to my local copy of the production database, and read in the excel sheet containing the new transactions to be uploaded

{% highlight python %}
import numpy as np
import pandas as pd
import difflib
from datetime import datetime
from sqlalchemy import (MetaData, Table, Column, Integer, Float, Numeric, String, Text, DateTime, ForeignKey, create_engine)
from sqlalchemy import select, asc, desc, update, delete
from sqlalchemy import and_, or_, not_
from sqlalchemy.sql import func

metadata = MetaData()

db_name='prod_db'
engine=create_engine('mysql+pymysql://root:rootroot@localhost/' + db_name + '?host=localhost?port=3306')
metadata.create_all(engine)
connection = engine.connect()

df_v1 = pd.read_excel('/Users/oresttokovenko/Desktop/Client Data/Client_1/Data/Aug_08_2021/Data_4Aug2021.xlsx',
sheet_name= 'Uploading Data')

{% endhighlight %}

Next, I select the data I need to pull in regarding to this specific client using a SQL query, then convert the results into an array of dictionaries, which then becomes a Pandas DataFrame

{% highlight python %}
results = connection.execute("SELECT id AS investor_id, account_type, LOWER(IF(account_type='investor', CONCAT(first_name, ' ', last_name), company_name)) AS account_holder_name FROM investors WHERE company_id = 1 AND active=1 AND deleted_at IS NULL").fetchall()

live_investors_list = [dict(zip(['investor_id', 'account_type', 'account_holder_name'], d)) for d in results]

len(live_investors_list)
> 10208

live_investors_list_df = pd.DataFrame(live_investors_list)

{% endhighlight %}


{% highlight python %}
def internal_find_closest_match(text, potential_candidates):
number_of_closest_matches_to_return = 3
# Cutoff is 0.6 by default. It needs to be between 0 and 1.
cutoff = 0.8
closest_matches = difflib.get_close_matches(text, potential_candidates, number_of_closest_matches_to_return, cutoff)

    if len(closest_matches) > 0:
        return closest_matches[0]
    else:
        return None

def df_find_closest_match(row, colname, potential_candidates):
text = row[colname]
text = text.strip()

    closest_match = internal_find_closest_match(text, potential_candidates)
    #print(closest_match)
    return closest_match

{% endhighlight %}

get unique list

{% highlight python %}
list_of_people_to_look_up_in_db_df = df_v1

list_of_people_to_look_up_in_db_df['name_lowercase'] = list_of_people_to_look_up_in_db_df['investor_name'].str.lower()

all_account_holder_names_in_live_db = live_investors_list_df['account_holder_name'].unique()

all_account_holder_names_in_live_db
> array(['lela drake', 'mary taber', 'orlando ohara', ..., 'harvey prentice', 'albert hite', 'earl mcgraw'], dtype=object)

{% endhighlight %}

placeholder

{% highlight python %}

list_of_people_to_look_up_in_db_df['closest_account_holder_name_match'] = list_of_people_to_look_up_in_db_df.apply(
df_find_closest_match,
axis=1,
colname='name_lowercase',
potential_candidates=all_account_holder_names_in_live_db
)

live_investors_list_df.drop_duplicates(subset=['account_holder_name'], inplace=True)

df_v2 = pd.merge(
list_of_people_to_look_up_in_db_df,
live_investors_list_df,
left_on=['closest_account_holder_name_match'],
right_on=['account_holder_name'],
)

{% endhighlight %}

removing duplicates and checking to see if any matches failed

{% highlight python %}

df_v2[df_v2.duplicated(subset='investor_name', keep=False)]

df_v2.drop_duplicates(subset=['investor_name'], inplace=True)

df_v2[
df_v2['closest_account_holder_name_match'] == None
]
> (empty table)
{% endhighlight %}

now that the names are matched, we can ensure that the `investor_id` matches too
