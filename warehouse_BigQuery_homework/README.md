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

