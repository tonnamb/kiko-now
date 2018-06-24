---
layout: post
title: Scala - Apache Spark DataFrame API Cheatsheet
tags:
  - spark
---

Having a good cheatsheet at hand can significantly speed up the development process.
One of the best cheatsheet I have came across is <a href="https://ugoproto.github.io/ugo_r_doc/sparklyr-cheatsheet.pdf" target="_blank">sparklyr's cheatsheet</a>.

For my work, I'm using Spark's DataFrame API in Scala to create data transformation pipelines. These are some functions and design patterns that I've found to be extremely useful.

### Load data
```scala
val df = spark.read.parquet("filepath")
```

### Get SparkContext information
```scala
println(sc.getConf.getAll.mkString("\n"))
```

### Get Spark version
```scala
sc.version
```

### Get number of partitions
```scala
df.rdd.getNumPartitions
```

### Count number of rows
```scala
df.count
```

### Print schema
```scala
df.printSchema
```

### Preview top 20 rows
```scala
df.show
```

### Design pattern for constructing as data transformation pipeline
```scala
import org.apache.spark.sql.DataFrame
 
def filter_slim_cat_dog(df: DataFrame): DataFrame = {
  df.filter(($"animal_type" isin ("cat", "dog")) && ($"weight" <= 100))
}
 
def join_vet(df: DataFrame): DataFrame = {
  df.join(vet_provider, Seq("VETID", "ANIMALID"))
}
 
val animal_slim_cat_dog = 
  animal.transform(filter_slim_cat_dog)
        .transform(join_vet)
```

### Drop duplicate rows
```scala
df.dropDuplicates(Seq("VETID", "ANIMALID"))
```

For an exhaustive list of the functions, you can check out the <a href="https://spark.apache.org/docs/latest/api/scala/index.html#org.apache.spark.sql.Dataset" target="_blank">Spark's Dataset class documentation</a>.

Hope you've found this cheatsheet useful. If you did, please share it with your friends. Thank you!
