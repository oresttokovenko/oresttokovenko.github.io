---
layout: post
title: Creating Custom Macros in dbt for Efficient YAML Generation
description:
date:   2023-03-19
image:  '/images/dbt_macro.jpg'
tags:   [SQL, dbt, Jinja]
---

As a data engineer, I spend a lot of time working with YAML files. These files contain information about the data that we're working with, including the column names, data types, and other metadata. But I've always found it frustrating to manually reorganize these files, especially when I'm dealing with large datasets.

That's when I took inspiration from an existing dbt model called `generate_model_yaml` and decided to create a custom macro that would automatically generate my YAML for new dbt models, as well as organizing the columns. The reason for organizing it in such a way is that it aligns with our company YAML naming standards. This new macro adds the much-needed functionality to the existing macro, which includes unnecessary boilerplate code, always adds description rows and does not organize the columns in any way. In addition, I've added error handling and validation to the macro, which checks if the model_name provided as an argument exists in the dbt project. This will help prevent potential issues due to incorrect input.

Now let's dive into the custom macro implementation.

{% highlight jinja %}
{% raw %}
{% macro generate_ordered_yaml(model_name) %}

{%- set all_models = dbt_utils.get_relation_names(database='warehouse') -%}
{%- if model_name not in all_models %}
    {{ log("The provided model name '" ~ model_name ~ "' does not exist in the dbt project.", info=True) }}
    {% do return() %}
{%- endif %}

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

This macro organizes columns into four categories based on their names:

1. ID columns: Columns that end with `_id`
2. Date columns: Columns that end with `_date` or `_at` 
3. Fivetran metadata columns: Columns that start with an underscore `_*`
4. Everything else: All other columns

The macro then sorts the columns alphabetically within each category and concatenates them into a single ordered list. Finally, the macro generates a YAML file with the organized columns.

To use the `generate_ordered_yaml` macro, include it in your dbt project under the `/macros` directory. Then, run the macro using the following command, replacing `<your_dbt_model>` with the name of the dbt model for which you want to generate the YAML file

{% highlight bash %}
dbt run-operation generate_ordered_yaml --args '{"model_name":"<your_dbt_model>"}'
{% endhighlight %}

Here's an example of the generated YAML file after running the macro:

{% highlight yaml %}
- name: your_dbt_model
  description: ""
  columns:
    - name: order_id
    - name: user_id
    - name: item_name
    - name: price
    - name: order_date
    - name: created_at
    - name: _fivetran_deleted
    - name: _fivetran_synced

{% endhighlight %}

Creating custom dbt macros like `generate_ordered_yaml` can significantly streamline your data engineering tasks and help you work more efficiently with large datasets. By leveraging the power of Jinja and dbt, you can easily generate and organize YAML files for your dbt models. For more information on creating custom dbt macros, including the fundamentals of Jinja, check out this tutorial by [Madison Mae](https://madisonmae.substack.com/p/tutorial-write-a-custom-dbt-macro){:target="_blank"}{:rel="noopener noreferrer"}

