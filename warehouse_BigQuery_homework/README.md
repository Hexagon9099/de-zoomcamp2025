# Preration 1. Create an external table using the Yellow Taxi Trip Records.

CREATE OR REPLACE EXTERNAL TABLE `kestra-sandbox-449315.de_zoomcamp.yellow_tripdata_2024_external` \
OPTIONS ( \
  format='PARQUET', \
  uris=['gs://kestra-de-zoomcamp-bucket-kestra-sandbox-449315/yellow_tripdata_2024-*.parquet'] \
); 


# Preparation 2. Create a (regular/materialized) table in BQ using the Yellow Taxi Trip Records (do not partition or cluster this table).

CREATE OR REPLACE TABLE `kestra-sandbox-449315.de_zoomcamp.yellow_tripdata_2024_hw3` \
AS \
SELECT *  \
FROM `kestra-sandbox-449315.de_zoomcamp.yellow_tripdata_2024_external` 


# Q1. What is count of records for the 2024 Yellow Taxi Data?

SELECT COUNT (*) \
FROM `kestra-sandbox-449315.de_zoomcamp.yellow_tripdata_2024_hw3` \

Answer: 20,332,093


# Q2. Write a query to count the distinct number of PULocationIDs for the entire dataset on both the tables. What is the estimated amount of data that will be read when this query is executed on the External Table and the Table?

SELECT COUNT(DISTINCT PULocationID) \
FROM `kestra-sandbox-449315.de_zoomcamp.yellow_tripdata_2024_external`;

SELECT COUNT(DISTINCT PULocationID) \
FROM `kestra-sandbox-449315.de_zoomcamp.yellow_tripdata_2024_hw3`;

Answer: 0 MB for the External Table and 155.12 MB for the Materialized Table


# Q3. Write a query to retrieve the PULocationID from the table (not the external table) in BigQuery. Now write a query to retrieve the PULocationID and DOLocationID on the same table. Why are the estimated number of Bytes different?

SELECT PULocationID \
FROM `kestra-sandbox-449315.de_zoomcamp.yellow_tripdata_2024_hw3`;

SELECT PULocationID, DOLocationID \
FROM `kestra-sandbox-449315.de_zoomcamp.yellow_tripdata_2024_hw3`;

Answer: BigQuery is a columnar database, and it only scans the specific columns requested in the query. Querying two columns (PULocationID, DOLocationID) requires reading more data than querying one column (PULocationID), leading to a higher estimated number of bytes processed.


# Q4. How many records have a fare_amount of 0?

SELECT COUNT (fare_amount) \
FROM `kestra-sandbox-449315.de_zoomcamp.yellow_tripdata_2024_hw3` \
WHERE fare_amount = 0; \

Answer: 8333


# Q5. What is the best strategy to make an optimized table in Big Query if your query will always filter based on tpep_dropoff_datetime and order the results by VendorID (Create a new table with this strategy)

CREATE OR REPLACE TABLE `kestra-sandbox-449315.de_zoomcamp.yellow_tripdata_2024_hw3_partitioned_clustered` \
PARTITION BY DATE (tpep_dropoff_datetime) \
CLUSTER BY VendorID \
AS \
SELECT * \
FROM `kestra-sandbox-449315.de_zoomcamp.yellow_tripdata_2024_hw3`;

Answer: Partition by tpep_dropoff_datetime and Cluster on VendorID


# Q6. Write a query to retrieve the distinct VendorIDs between tpep_dropoff_datetime 2024-03-01 and 2024-03-15 (inclusive) 

Use the materialized table you created earlier in your from clause and note the estimated bytes. Now change the table in the from clause to the partitioned table you created for question 5 and note the estimated bytes processed. What are these values?
Choose the answer which most closely matches.

SELECT DISTINCT VendorID \
FROM `kestra-sandbox-449315.de_zoomcamp.yellow_tripdata_2024_hw3` \
WHERE DATE (tpep_dropoff_datetime) BETWEEN '2024-03-01' AND '2024-03-15';

Answer: 310.24 MB for non-partitioned table and 26.84 MB for the partitioned table



