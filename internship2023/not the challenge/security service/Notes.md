Was like this
```scala
val session = CassandraConnector.apply(new SparkConf(true).set("spark.cassandra.connection.host", "127.0.0.1")).openSession()  
  val session = Cluster.builder().addContactPoint("localhost").withoutJMXReporting().build().connect()  
  val res = session.execute("select * from challenge.users")  
  case class Users(email: String, password: String)  
  val res_users = res.all().map(row => Users(row.getString("email"), row.getString("password"))).toArray[Users]
```

JWT authentikat doesn't work with scala 2.12, no matter what version i use
using this instead
[JWT Scala: Native (jwt-scala.github.io)](https://jwt-scala.github.io/jwt-scala/jwt-core-jwt.html)

add ID for the user
encrypt the passord with secret key
verify from cookie