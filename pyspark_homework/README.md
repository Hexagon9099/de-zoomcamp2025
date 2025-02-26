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
