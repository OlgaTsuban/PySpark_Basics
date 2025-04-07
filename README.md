# PySpark_Basics

# PySpark Cheat Sheet 🚀

## **1. Initialization**
```python
from pyspark.sql import SparkSession
spark = SparkSession.builder \
    .appName("MyApp") \
    .config("spark.some.config.option", "value") \
    .getOrCreate()
```

## **2. Read data**
# CSV
```python
df = spark.read.csv("path.csv", header=True, inferSchema=True)
```

# Parquet
```python
df = spark.read.parquet("path.parquet")
```

# JSON
```python
df = spark.read.json("path.json")
```

# From list of tuples
```python
data = [("Alice", 34), ("Bob", 45)]
df = spark.createDataFrame(data, ["Name", "Age"])
```

## **3. Basic Operations**
```python
df.show(5)          # Display first 5 rows
df.printSchema()    # Show schema
df.columns          # List columns
df.count()          # Row count
df.describe().show() # Summary stats
```

## **4. Column Operations**
```python
from pyspark.sql.functions import col, lit
# Select columns
df.select("col1", "col2").show()
# Add new column
df = df.withColumn("new_col", lit("value"))
# Rename column
df = df.withColumnRenamed("old", "new")
# Filter
df.filter(col("age") > 30).show()
df.filter("age > 30").show()  # SQL syntax
```

## **5. Common Functions**
```python
from pyspark.sql.functions import *

# String
df.withColumn("upper", upper(col("name")))
df.withColumn("length", length(col("name")))
# Math
df.withColumn("sq", pow(col("num"), 2))
# Date
df.withColumn("today", current_date())
df.withColumn("year", year(col("date_col")))
# Aggregation
df.groupBy("department").agg(
    avg("salary").alias("avg_salary"),
    count("*").alias("count")
).show()
```


## **6. Handling Nulls**
```python
df.na.fill(0)                          # Fill nulls with 0
df.na.fill({"age": 30, "name": "N/A"}) # Column-specific fill
df.na.drop()                           # Drop rows with nulls
```


## **7. Joins**
```python
df1.join(df2, df1.id == df2.id, "inner")  # Also: left, right, outer
```


## **8. SQL Queries**
```python
df.createOrReplaceTempView("people")
spark.sql("SELECT * FROM people WHERE age > 30").show()
```

## **9. Writing Data**
```python
df.write.csv("output.csv", header=True)
df.write.parquet("output.parquet")
df.write.saveAsTable("table_name")  # For Hive
```
## **10. Performance Tips**
```python
df.cache()  # Cache frequently used DataFrames
df.repartition(4)  # Control partition count
spark.conf.set("spark.sql.shuffle.partitions", "200")  # Tune shuffles
```

## **11. UDFs (User Defined Functions)**
```python
from pyspark.sql.functions import udf
from pyspark.sql.types import StringType

# Register UDF
square_udf = udf(lambda x: x*x, IntegerType())
df.withColumn("squared", square_udf(col("num")))

# Pandas UDF (Vectorized - faster)
@pandas_udf(IntegerType())
def pandas_square(s: pd.Series) -> pd.Series:
    return s * s
```

## **12. Troubleshooting**
```python
df.explain()  # Show execution plan
spark.sparkContext.uiWebUrl  # Spark UI URL
Pro Tip: Use df.limit(1000).toPandas() for quick EDA in Jupyter notebooks!
```





