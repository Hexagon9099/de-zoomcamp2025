# Q1. Within the execution for Yellow Taxi data for the year 2020 and month 12: what is the uncompressed file size (i.e. the output file yellow_tripdata_2020-12.csv of the extract task)?
execute ETL pipeline workflows first, attached in the current folder. \
then go to the gcp bucket, find required file and look in its details. \
128.3 MB

# Q2. What is the rendered value of the variable file when the inputs taxi is set to green, year is set to 2020, and month is set to 04 during execution?
inputs: \
  - id: taxi \
    type: SELECT \
    displayName: Select taxi type \
    values: ['yellow', 'green'] \
    defaults: 'yellow' 

  - id: year \
    type: SELECT \
    displayName: Select year \
    values: ["2019", "2020"] \
    defaults: "2019" 

  - id: month \
    type: SELECT \
    displayName: Select month \
    values: ["01", "02", "03", "04", "05", "06", "07", "08", "09", "10", "11", "12"] \
    defaults: "01" 

variables: \
  file: "{{inputs.taxi}}_tripdata_{{inputs.year}}-{{inputs.month}}.csv" \
  staging_table: "public.{{inputs.taxi}}_tripdata_staging" \
  table: "public.{{inputs.taxi}}_tripdata" \
  data: "{{outputs.extract.outputFiles[inputs.taxi ~ '_tripdata_' ~ inputs.year ~ '-' ~ inputs.month ~ '.csv']}}" 

  the answer is green_tripdata_2020-04.csv

# Q3. How many rows are there for the Yellow Taxi data for all CSV files in the year 2020?
execute 'gcp ETL pipeline scheduled' using triggers first, to backfill data for 2020 year. \
then, in BigQuery 

SELECT COUNT (*) AS row_count \
 FROM `kestra-sandbox-449315.de_zoomcamp.yellow_tripdata` \
 WHERE EXTRACT(YEAR FROM tpep_pickup_datetime) = 2020

to check if there are NULL values in both pickup and dropoff columns run

SELECT COUNT(*) AS row_count \
FROM `kestra-sandbox-449315.de_zoomcamp.yellow_tripdata` \
WHERE \
  EXTRACT(YEAR FROM tpep_pickup_datetime) = 2020 \
  OR EXTRACT(YEAR FROM tpep_dropoff_datetime) = 2020\
  OR (tpep_pickup_datetime IS NULL AND tpep_dropoff_datetime IS NOT NULL AND EXTRACT(YEAR FROM tpep_dropoff_datetime) = 2020) \
  OR (tpep_dropoff_datetime IS NULL AND tpep_pickup_datetime IS NOT NULL AND EXTRACT(YEAR FROM tpep_pickup_datetime) = 2020) 

 the answer is 24,648,499

 # Q4 How many rows are there for the Green Taxi data for all CSV files in the year 2020?
 similarly to Q3, \
 execute 'gcp ETL pipeline scheduled' using triggers first, to backfill data for 2020 year. \
then, in BigQuery

SELECT COUNT (*) AS row_count \ 
 FROM `kestra-sandbox-449315.de_zoomcamp.green_tripdata` \
 WHERE EXTRACT(YEAR FROM lpep_pickup_datetime) = 2020

 the answer is 1,734,051



