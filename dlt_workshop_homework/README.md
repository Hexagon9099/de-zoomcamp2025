All the exircises are performed either in Google Collab or Jupyter notebook

# Q1. what is dlt version?
!pip install dlt[duckdb] \
!dlt --version

Answer: 1.6.1

# Q2. Ingest data through dlt into DuckDB. How many tables were created?

_Cell 1:_

import dlt
from dlt.sources.helpers.rest_client import RESTClient
from dlt.sources.helpers.rest_client.paginators import PageNumberPaginator

@dlt.resource \
def ny_taxi_data(): \
    api_url = "https://us-central1-dlthub-analytics.cloudfunctions.net/data_engineering_zoomcamp_api" \
    client = RESTClient(base_url=api_url) \
    paginator = PageNumberPaginator() \
    page = 1 \
    while True: \
        response = client.get(f"/data", params={"page":page, "per_page":1000}) \
        if not response.json(): \
            break \
        yield response.json() \
        page += 1

pipeline = dlt.pipeline( \
    pipeline_name="ny_taxi_pipeline", \
    destination="duckdb", \
    dataset_name="ny_taxi_data" \
)


_Cell 2:_

load_info = pipeline.run(ny_taxi_data) \
print(load_info)


_Cell 3:_

import duckdb \
import pandas as pd

conn = duckdb.connect(f"{pipeline.pipeline_name}.duckdb") \
conn.sql(f"SET search_path = '{pipeline.dataset_name}'")

df = conn.sql("DESCRIBE").df()

df


Answer: 4


# Q3. What is the total number of records extracted? 

df = pipeline.dataset(dataset_type="default").ny_taxi_data.df() \
df

Answer: 10000


# Q4. What is the average trip duration?

with pipeline.sql_client() as client: \
    res = client.execute_sql( \
        """ \
        SELECT \
        AVG(date_diff('minute', trip_pickup_date_time, trip_dropoff_date_time)) \
        FROM ny_taxi_data; \
        """ \
    ) \
print(res)

Answer: 12.3049

