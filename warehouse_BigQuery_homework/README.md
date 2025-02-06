# Preration 1. Create an external table using the Yellow Taxi Trip Records.
CREATE OR REPLACE EXTERNAL TABLE `kestra-sandbox-449315.de_zoomcamp.yellow_tripdata_2024_external` \
OPTIONS ( \
  format='PARQUET', \
  uris=['gs://kestra-de-zoomcamp-bucket-kestra-sandbox-449315/yellow_tripdata_2024-*.parquet'] \
); 

