from pyspark.sql import SparkSession
from pyspark.sql.functions import col, round, lit
from datetime import datetime

spark = SparkSession.builder.appName("pyspark-basics").getOrCreate()
spark.conf.set('temporaryGcsBucket', 'datalake-raw-poc')

df = spark.read.parquet('gs://datalake-raw-poc/ingested/parquet/20240414/part-00000.snappy.parquet')
df.show(5, truncate=False)
datetime_str = datetime.now().strftime('%Y-%m-%d %H:%M:%S')

df_select = df.select(["PassengerId","Name","Sex"])
df_select = df_select.withColumn("process_datetime", lit(datetime_str))

df_select.show(5, truncate=False)
df_select.printSchema()
df_select.write.format("bigquery").option("table","raw_titanic.tb_passengers_2").mode("append").save()
