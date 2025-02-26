# Q1. Execute spark.version. What's the output?

import pyspark \
from pyspark.sql import SparkSession

spark = SparkSession.builder \
    .master("local[*]") \
    .appName('test') \
    .getOrCreate()

spark.version

Answer: 3.3.2
