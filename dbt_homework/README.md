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

    {%- set target_env_var = 'DBT_BIGQUERY_TARGET_DATASET'  -%} 
    {%- set stging_env_var = 'DBT_BIGQUERY_STAGING_DATASET' -%}

    {%- if model_type == 'core' -%} {{- env_var(target_env_var) -}} 
    {%- else -%}                    {{- env_var(stging_env_var, env_var(target_env_var)) -}}
    {%- endif -%}

{%- endmacro %}

Use as \
{{ config( \
    schema=resolve_schema_for('core'), \
) }}

Answers:
1) Setting a value for DBT_BIGQUERY_TARGET_DATASET env var is mandatory, or it'll fail to compile 
2) When using core, it materializes in the dataset defined in DBT_BIGQUERY_TARGET_DATASET
3) When using stg, it materializes in the dataset defined in DBT_BIGQUERY_STAGING_DATASET, or defaults to DBT_BIGQUERY_TARGET_DATASET
4) When using staging, it materializes in the dataset defined in DBT_BIGQUERY_STAGING_DATASET, or defaults to DBT_BIGQUERY_TARGET_DATASET

# Q5. Considering the YoY Growth in 2020, which were the yearly quarters with the best (or less worse) and worst results for green, and yellow
The solution is in a dbt model named _fct_taxi_trips_quarterly_revenue.sql_ and attached in this directory in a dbt project.

Answer: green: {best: 2020/Q1, worst: 2020/Q2}, yellow: {best: 2020/Q1, worst: 2020/Q2}

# Q6. What are the values of p97, p95, p90 for Green Taxi and Yellow Taxi, in April 2020?
The solution is in a dbt model named _fct_taxi_trips_monthly_fare_p95.sql_ and attached in this directory in a dbt project.

This query is just a check from fact_trips table, the code is run by the mentioned model.

{ SELECT 
  service_type, 
  APPROX_QUANTILES(fare_amount, 100)[OFFSET(97)] AS p97, 
  APPROX_QUANTILES(fare_amount, 100)[OFFSET(95)] AS p95, 
  APPROX_QUANTILES(fare_amount, 100)[OFFSET(90)] AS p90 
FROM `de_zoomcamp.fact_trips` 
WHERE year = 2020
  AND month = 4
  AND fare_amount > 0
  GROUP BY service_type
ORDER BY service_type; }

Answer: 

