### Word count
```scala
val spark = SparkSession.builder  
  .master("local[*]")  
  .appName("Spark Word Count")  
  .getOrCreate()  
  
val lines = spark.sparkContext.parallelize(  
  Seq("Spark Intellij Idea Scala test one",  
    "Spark Intellij Idea Scala test two",  
    "Spark Intellij Idea Scala test three"))  
  
val counts = lines  
  .flatMap(line => line.split(" "))  
  .map(word => (word, 1))  
  .reduceByKey(_ + _)  
  
counts.foreach(println)
```

### DataFrame
```scala
val df = session.createDataFrame(  
  List(  
    ("scala", 25000),  
    ("Spark", 35000),  
    ("PHP", 21000))  
).toDF("Language", "Users count")

df.show()
```

### TempView
Make a view that is 
```scala
val session = getSession()  
val df = session.createDataFrame(  
  List(  
    ("scala", 25000),  
    ("Spark", 35000),  
    ("PHP", 21000))  
).toDF("language", "users count")  
df.show()  
df.createOrReplaceTempView("sample_table")  
val df2 = session.sql("Select language FROM sample_table")  
df2.show()
```

### Structype
```
 val struct = (new StructType)
   .add(StructField("a", IntegerType, true))
   .add(StructField("b", LongType, false))
   .add(StructField("c", StringType, true))
```

```scala
val schema = StructType(
    StructField("firstName", StringType, true) ::
      StructField("lastName", IntegerType, false) ::
      StructField("middleName", IntegerType, false) :: Nil)
```

```scala
// Define case class outside the main function otherwise it will throw an error
case class Name(firstName:String, lastName:String, middleName:String)

main function : 
	val spark:SparkSession = SparkSession.builder()  
	  .master("local[1]")  
	  .appName("SparkByExamples.com")  
	  .getOrCreate()  
	import org.apache.spark.sql.types._  
	import spark.implicits._  
	  
	val schema = StructType(  
	  StructField("firstName", StringType, true) ::  
	    StructField("lastName", IntegerType, false) ::  
	    StructField("middleName", IntegerType, false) :: Nil)  
	  
	val colSeq = Seq("firstName","lastName","middleName")  
	  
	val ds1 = spark.emptyDataset[Name]  
	ds1.printSchema()
```