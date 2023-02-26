Checks:
- keyspace exists
```scala
def keyspaceExists(spark: SparkSession): Boolean = {  
  val connector: CassandraConnector = CassandraConnector.apply(spark.sparkContext)  
  val linesCount: Int = spark.sparkContext.cassandraTable("system_schema", "keyspaces")  
    .filter(row => row.getString("keyspace_name") == "challenge")  
    .collect()  
    .count(_ => true)  
  linesCount == 1  
}
```
- tables exist
```

```
- tables are not empty

[An introduction to Akka HTTP routing (freecodecamp.org)](https://www.freecodecamp.org/news/an-introduction-to-akka-http-routing-697b00399cad/)
[akka-http-user-authorization-using-jwt/UserManagementRoutes.scala at master · UtkarshaMusmade/akka-http-user-authorization-using-jwt (github.com)](https://github.com/UtkarshaMusmade/akka-http-user-authorization-using-jwt/blob/master/src/main/scala/routes/UserManagementRoutes.scala)
[Akka Http Authorization using JWT - Knoldus Blogs](https://blog.knoldus.com/authorization-using-jwt/)
[Building a fully functional REST service using Akka actors | by Ian Shiundu | Medium](https://medium.com/@ian.shiundu/building-a-fully-functional-rest-service-using-akka-actors-8b5c12978380)
