# Q1. What does this .sql model compile to?
select * \
from {{ source('raw_nyc_tripdata', 'ext_green_taxi' ) }}

Answer: select * from myproject.my_nyc_tripdata.ext_green_taxi



