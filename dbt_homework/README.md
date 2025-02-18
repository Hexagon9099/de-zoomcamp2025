# Q1. What does this .sql model compile to?
select * \
from {{ source('raw_nyc_tripdata', 'ext_green_taxi' ) }}

Answer: select * from myproject.my_nyc_tripdata.ext_green_taxi


# Q2. What would you change to accomplish that in a such way that command line arguments takes precedence over ENV_VARs, which takes precedence over DEFAULT value?

Answer: Update the WHERE clause to pickup_datetime >= CURRENT_DATE - INTERVAL '{{ var("days_back", env_var("DAYS_BACK", "30")) }}' DAY'


# Q3. Select the option that does NOT apply for materializing fct_taxi_monthly_zone_revenue

Answer: dbt run --select models/staging/+


