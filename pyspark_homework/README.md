# Q1. Execute spark.version. What's the output?

import pyspark \
from pyspark.sql import SparkSession

spark = SparkSession.builder \
    .master("local[*]") \
    .appName('test') \
    .getOrCreate()

spark.version

Answer: 3.3.2



# Q2. What is the average size of the Parquet (ending with .parquet extension) Files that were created (in MB)?

The solution is in the attached .ipynb file. I read, casted column types and wrote results to parquet. Then, in a terminal:

$ ls -lh data/pq/yellow/2024/10/

Answer: 25MB



# Q3. How many taxi trips were there on the 15th of October?

The full solution is in an attached file.

df_pq = spark.sql(""" \
SELECT \
    COUNT (*) AS num_trips, \
    CAST(pickup_datetime AS DATE) AS date \
FROM yellow_tripdata_1024 \
WHERE pickup_datetime >= '2024-10-15' \
    AND pickup_datetime < '2024-10-16' \
GROUP BY date \
""")

df_pq.show()

Answer: 125,567
