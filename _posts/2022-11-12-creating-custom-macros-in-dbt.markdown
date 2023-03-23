---
layout: post
title: Creating Custom Macros in dbt
description:
date:   2023-03-19
image:  '/images/dbt_macro.jpg'
tags:   [SQL, dbt, Jinja]
---

As a data engineer, I spend a lot of time working with YAML files. These files contain information about the data that we're working with, including the column names, data types, and other metadata. But I've always found it frustrating to manually reorganize these files, especially when I'm dealing with large datasets.

That's when I took inspiration from an existing dbt model called `generate_model_yaml` and decided to create a custom macro that would automatically generate my YAML for new dbt models, as well as organizing the columns into four categories: ID columns, date columns, Fivetran metadata columns, and everything else. The reason for organizing it in such a way is that it aligns with our company YAML naming standards. This new macro adds the much-needed functionality to the existing macro, which includes unnecessary boilerplate code, always adds description rows and does not organize the columns in any way.

Id columns are columns that contain unique identifiers for each row in the dataset. These are typically used as primary keys when we're joining tables together. Date columns are columns that contain dates, Fivetran columns are columns for Fivetran ingestion metadata, and everything else includes all the other columns.

{% highlight jinja %}

{% raw %}
{% macro generate_ordered_yaml(model_name) %}

{% set model_yaml=[] %}

{% do model_yaml.append('\n') %}
{% do model_yaml.append('  - name: ' ~ model_name | lower) %}
{% do model_yaml.append('    description: ""') %}
{% do model_yaml.append('    columns:') %}

{% set relation=ref(model_name) %}
{%- set columns = adapter.get_columns_in_relation(relation) -%}

{% set id_columns = [] %}
{% set date_columns = [] %}
{% set other_columns = [] %}
{% set fivetran_columns = [] %}
{% for column in columns %}
    {% if column.name.lower().endswith('_id') %}
        {% do id_columns.append(column) %}
    {% elif column.name.lower().endswith('_date') or column.name.lower().endswith('_at') %}
        {% do date_columns.append(column) %}
    {% elif column.name.lower().startswith('_') %}
        {% do fivetran_columns.append(column) %}
    {% else %}
        {% do other_columns.append(column) %}
    {% endif %}
{% endfor %}

{% set id_columns = id_columns | sort(attribute='name') %}
{% set date_columns = date_columns | sort(attribute='name') %}
{% set other_columns = other_columns | sort(attribute='name') %}
{% set fivetran_columns = fivetran_columns | sort(attribute='name') %}

{% set updated_columns = id_columns + other_columns + date_columns + fivetran_columns %}

{% for column in updated_columns %}
    {% do model_yaml.append('      - name: ' ~ column.name | lower ) %}
{% endfor %}

{% if execute %}

    {% set joined = model_yaml | join ('\n') %}
    {{ log(joined, info=True) }}
    {% do return(joined) %}

{% endif %}

{% endmacro %}
{% endraw %}
{% endhighlight %}

By automatically organizing the YAML file based on these categories, I can easily see which columns are IDs, which are dates, and which are everything else. This makes it much easier for me to work with the data and to build pipelines that use this data. You can run the macro by using the query below, sure make sure you include it in your repo under the `~/macros` directory.

{% highlight bash %}

dbt run-operation generate_ordered_yaml --args '{"model_name":"<your_dbt_model>"}'

{% endhighlight %}

Overall, creating this macro has been a huge time-saver for me. It's allowed me to spend more time focusing on the data itself, rather than on the tedious task of manually reorganizing YAML files. If you're interested in creating your own custom dbt macros, check out this tutorial by Madison Mae at `https://madisonmae.substack.com/p/tutorial-write-a-custom-dbt-macro`. By leveraging the power of Jinja and dbt, you too can streamline your data engineering tasks and work more efficiently with large datasets