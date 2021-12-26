---
layout: post
title:  How to Import Multiple Excel files into Pandas
date:   2021-12-12
image:  '/images/import_multiple_excel.jpg'
tags:   [Pandas, Excel, Python]
---

#### _Disclaimer_: All proprietary company data has been completely anonymized and replaced with randomly generated counterparts, to respect the stipulations outlined in my non-disclosure agreement

First, you need to ensure all the files are in the same folder and are `.xlsx`

{% highlight python %}
import glob
import pandas as pd
import numpy as np

# get file names
path =r'/Users/oresttokovenko/Desktop/Client Data/EP Capital/Data/Aug_24_2021/Offerings'
filenames = glob.glob(path + "/*.xlsx")

filenames
> ['/Users/oresttokovenko/Desktop/Client Data/Client/Data/Aug_24_2021/Offerings/placeholder_1.xlsx',
'/Users/oresttokovenko/Desktop/Client Data/Client/Data/Aug_24_2021/Offerings/placeholder_2.xlsx',
'/Users/oresttokovenko/Desktop/Client Data/Client/Data/Aug_24_2021/Offerings/placeholder_3.xlsx',
'/Users/oresttokovenko/Desktop/Client Data/Client/Data/Aug_24_2021/Offerings/placeholder_4.xlsx',

master_df = pd.DataFrame()
for file in filenames:
    master_df = master_df.append(pd.read_excel(file, sheet_name = 0, skiprows = 1), ignore_index=True)

master_df.sample(20)

{% endhighlight %}