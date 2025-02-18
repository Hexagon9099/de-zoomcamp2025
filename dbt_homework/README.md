# Q1. What does this .sql model compile to?
select * \
from {{ source('raw_nyc_tripdata', 'ext_green_taxi' ) }}

Answer: select * from myproject.my_nyc_tripdata.ext_green_taxi


# Q2. What would you change to accomplish that in a such way that command line arguments takes precedence over ENV_VARs, which takes precedence over DEFAULT value?

Answer: Update the WHERE clause to pickup_datetime >= CURRENT_DATE - INTERVAL '{{ var("days_back", env_var("DAYS_BACK", "30")) }}' DAY'


# Q3. Select the option that does NOT apply for materializing fct_taxi_monthly_zone_revenue

Answer: dbt run --select models/staging/+


# Q4. Regarding macro, select all statements that are true to the models using it:
{% macro resolve_schema_for(model_type) -%} 

    {%- set target_env_var = 'DBT_BIGQUERY_TARGET_DATASET'  -%} \
    {%- set stging_env_var = 'DBT_BIGQUERY_STAGING_DATASET' -%}

    {%- if model_type == 'core' -%} {{- env_var(target_env_var) -}} \
    {%- else -%}                    {{- env_var(stging_env_var, env_var(target_env_var)) -}}
    {%- endif -%}

{%- endmacro %}

Use as \
{{ config( \
    schema=resolve_schema_for('core'), \
) }}

Answers:
1) Setting a value for DBT_BIGQUERY_TARGET_DATASET env var is mandatory, or it'll fail to compile \
2) When using core, it materializes in the dataset defined in DBT_BIGQUERY_TARGET_DATASET
3) When using stg, it materializes in the dataset defined in DBT_BIGQUERY_STAGING_DATASET, or defaults to DBT_BIGQUERY_TARGET_DATASET
4) When using staging, it materializes in the dataset defined in DBT_BIGQUERY_STAGING_DATASET, or defaults to DBT_BIGQUERY_TARGET_DATASET



