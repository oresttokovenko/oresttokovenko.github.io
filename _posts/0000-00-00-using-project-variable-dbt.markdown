---
layout: post
title: Leveraging dbt Project Variables and Jinja for a DRY Workflow
description:
date:   2023-04-12
image:  '/images/project_variable.jpg'
tags:   [dbt, SQL, Jinja]
---

Oh, the joy of hard-coded values! Said no one ever. If you're anything like me, you've probably had your fair share of dealing with hard-coded values in your dbt code base, especially when you're working on a large-scale data project. As a Data Engineer, I've faced this issue countless times, and let me tell you, it's not fun. But worry not, my fellow data enthusiasts, because today, I'll be sharing my recent experience dealing with such a predicament and how I successfully used dbt project variables and Jinja to keep my code DRY (Don't Repeat Yourself).

#### Process

Let me set the scene: our company had recently acquired a competitor, and we were in the process of migrating their distribution centres to our warehouse management software. The catch? Our existing dbt code base had hard-coded values for distribution centre locations across multiple models. It was a mess. Here's how I tackled the issue:

First, I defined the variables in the dbt_project.yml file. This allowed me to store the distribution centre location information in a single, centralized place. That way, if there's a new value, I could simply update the list or dictionary in the dbt_project.yml file, and the changes would be reflected in all the models.

{% highlight yaml %}

vars:
    distribution_centres: ['CA', 'FL', 'NJ']

    distribution_centres_tz: {"CA":"America/Sacramento", "FL":"America/Fort_Lauderdale", "NJ":"America_Trenton"}

{% endhighlight %}


Next, I called these variables into each model using the `var()` function and instantiated Jinja objects. This helped me replace the hard-coded values with dynamic references that would update automatically whenever the variables were changed.

{% highlight jinja %}

{% raw %}

{% distribution_centres = var("distribution_centres") %}

{% distribution_centres_tz = var("distribution_centres_tz") %}

{% endraw %}

{% endhighlight %}

Finally, used them to iterate through the distribution centre location information. This allowed me to perform any necessary calculations or transformations on the data in a clean and efficient manner.

#### Example

##### Before implementing DRY code

{% highlight sql %}

, monthly_sums as (
    select
        quantity_on_hand.prod_id
        , quantity_on_hand.prod_sku
        , quantity_on_hand.recorded_at
        -- quantity ordered
        , coalesce(sum(
            case when int_ordered_items.assigned_wh = 'CA'
                then int_ordered_items.qty_ordered
            end )
            , 0) as last_30_day_qty_ordered_ca
        , coalesce(sum(
            case when int_ordered_items.assigned_wh = 'FL'
                then int_ordered_items.qty_ordered
            end )
            , 0) as last_30_day_qty_ordered_fl
        , coalesce(sum(
            case when int_ordered_items.assigned_wh = 'NJ'
                then int_ordered_items.qty_ordered
            end )
            , 0) as last_30_day_qty_ordered_nj
        , coalesce(sum(
            case when int_ordered_items.assigned_wh in ('FL', 'CA', 'NJ')
                then int_ordered_items.qty_ordered
            end )
            , 0) as last_30_day_qty_ordered

        -- cogs
        , round(coalesce(sum(
            case when int_ordered_items.assigned_wh = 'CA'
                then int_ordered_items.prod_cogs
            end ), 0)
            , 2) as last_30_day_cogs_ca
        , round(coalesce(sum(
            case when int_ordered_items.assigned_wh = 'FL'
                then int_ordered_items.prod_cogs
            end ), 0)
            , 2) as last_30_day_cogs_fl
        , round(coalesce(sum(
            case when int_ordered_items.assigned_wh = 'NJ'
                then int_ordered_items.prod_cogs
            end ), 0)
            , 2) as last_30_day_cogs_nj
        , round(coalesce(sum(
            case when int_ordered_items.assigned_wh in ('FL', 'CA', 'NJ')
                then int_ordered_items.prod_cogs
            end ), 0)
            , 2) as last_30_day_cogs

    from quantity_on_hand
    group by 1, 2, 3
)

{% endhighlight %}

##### After implementing DRY code

{% highlight jinja %}

{% raw %}

{% distribution_centres = var("distribution_centres") %}

, monthly_sums as (
    select
        quantity_on_hand.prod_id
        , quantity_on_hand.prod_sku
        , quantity_on_hand.recorded_at
        {% for distribution_centre in distribution_centres %}
        , coalesce(sum(
            case when int_ordered_items.assigned_wh = '{{ distribution_centre }}'
                then int_ordered_items.qty_ordered
            end )
            , 0) as last_30_day_qty_ordered_{{ distribution_centre }}
        {% endfor %}
        , coalesce(sum(
            case
                when
                    int_ordered_items.assigned_wh in (
                        '{{ distribution_centres|join("','") }}'
                    )
                    then int_ordered_items.qty_ordered
            end )
            , 0) as last_30_day_qty_ordered
        {% for distribution_centre in distribution_centres %}
        , round(coalesce(sum(
            case when int_ordered_items.assigned_wh = '{{ distribution_centre }}'
                then int_ordered_items.prod_cogs
            end ), 0)
            , 2) as last_30_day_cogs_{{ distribution_centre }}
        {% endfor %}
        , round(coalesce(sum(
            case
                when
                    int_ordered_items.assigned_wh in (
                        '{{ distribution_centres|join("','") }}'
                    )
                    then int_ordered_items.prod_cogs
            end ), 0)
            , 2) as last_30_day_cogs

    from quantity_on_hand
    group by 1, 2, 3
)

{% endraw %}

{% endhighlight %}


#### Caveats

You can't simply reference the variables in a `IN` operator. 

For example, this is what you might want to do:

{% highlight jinja %}

{% raw %}

select *
from warehouses in ({{ distribution_centres }})

{% endraw %}

{% endhighlight %}

It will throw an error (an extremely esoteric one at that).

Do this instead:

{% highlight jinja %}

{% raw %}

select *
from warehouses in ('{{ distribution_centres|join("','") }}')

{% endraw %}

{% endhighlight %}

With the help of dbt project variables and Jinja, I was able to say goodbye to hard-coded values and embrace a DRY workflow. Not only did it save me a ton of time, but it also made my code base more maintainable and easier to work with in the long run. So the next time you find yourself drowning in a sea of hard-coded values, just remember: dbt project variables and Jinja can be your lifesavers. It's not every day that you get to be a data superhero, but when you do, it feels pretty darn good!

