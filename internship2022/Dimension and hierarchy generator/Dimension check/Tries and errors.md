Made this 
![[Pasted image 20220715075835.png]]
With 
```scala
val df = session.read.option("header", true).option("delimiter", "|").csv("src/main/resources/ga_input/ga_dimensions_input/dimension_configuration.csv")  
df.printSchema()  
df.show()  
// how many columns ?  
val nbCol = df.columns.length  
val parentsColName = df.columns(nbCol-1)  
val levelColName = df.columns(nbCol-2)  
  
val newdf = df.groupBy(levelColName).agg(collect_set(parentsColName).alias(parentsColName))  
newdf.show()  
val levelsArr = df.select(levelColName).collect().map(_(0)).map(row=>row.toString).toList  
//val parentsArr = df.select(parentsColName).collectAsList.toArray()  
val parentsArr = df.select(parentsColName).collect().map(_(0)).map(row=>row.toString.split(",").toList).toList.map(sublist=>sublist.map(s=>s.trim()))  
val nestedList = levelsArr.zip(parentsArr).foreach(println(_))
```

**The problem is that the nestedList contains duplicate entries with different values so turning this into a map will lose some data**
=> Why not grouping by the values using the Dataframe first then we cast/convert it to a map
```scala
val newdf = df.groupBy(levelColName)
	.agg(collect_set(parentsColName).alias(parentsColName))  
newdf.show()
```

Executing the code above will yield this :

![[Pasted image 20220715080241.png]]

As we can see here, dimension_level column has distinct values but the parent_levels has duplicate values.

![[Pasted image 20220715080717.png]]
and the nested list will look like this
![[Pasted image 20220715080751.png]]

I think we better **explode** the rows based on the parents column then drop duplicate rows then group by level dimension column then use **collect_set** on it

#### Convert the string to an array of strings
```scala
val dfArr = df.select(split(col(parentsColName), ",").as(parentsColName))  
dfArr.printSchema()  
dfArr.show(false)
```

yields
![[Pasted image 20220715092652.png]]

```scala
val dfArr = df.select(col(levelColName), split(col(parentsColName), ",").as(parentsColName))
```

#### Explode the array into rows
```scala
dfArr.select(col(levelColName), explode(col(parentsColName)).as(parentsColName)).show(false)
```
![[Pasted image 20220715093202.png]]
**notice those white spaces, we need to remove them in order to eliminate comparison bugs**

```scala
val dfExploded = dfArr.select(col(levelColName), explode(col(parentsColName)).as(parentsColName)).withColumn(parentsColName, trim(col(parentsColName).as(parentsColName)))
```

#### Groupy dimension
```scala
val newdf = dfExploded.groupBy(levelColName).agg(collect_set(parentsColName).alias(parentsColName)).show(false)
```

This will yields
![[Pasted image 20220715094016.png]]

#### From columns to list of tuples
```scala
val levelsArr = newdf.select(levelColName).collect().map(_(0)).toList  
val parentsArr = newdf.select(parentsColName).collect().map(_(0)).toList  
val nestedList = levelsArr.zip(parentsArr).foreach(println(_))
```

```scala
val x = dfArr.collect().map(row=>{  
  row.getString(0)->row.getList(1).toArray().map(entry=>(entry.toString).trim()).toList  
}).toMap
```

```scala
val df = session.read.option("header", true).option("delimiter", "|").csv("src/main/resources/ga_input/ga_dimensions_input/dimension_configuration.csv")  
df.printSchema()  
//df.show(false)  
val nbCol = df.columns.length  
val parentsColName = df.columns(nbCol-1)  
val levelColName = df.columns(nbCol-2)

val dfArr = df.select(col(levelColName), split(col(parentsColName), ",").as("parent"))  
dfArr.show(false)  
val x = dfArr.collect().map(row=>{  
  row.getString(0)->row.getList(1).toArray().map(entry=>(entry.toString).trim()).toList  
}).toMap
```


**My way of detecting cycles in a graph**
Rank the graph, if we're unable to -> there is a cycle otherwise the graph is acyclic. PERT
We start by looking for nodes that have no prior connections -> Extract nodes that are not in the **DIMENSION_PARENT column**, those will be the starting position

We can do that with spark
We add a contains column that says if a **node** has a precceeding node

# First of all...
1. Seperate on tenant_id
2. Seperate on dimension_id
3. Construct your graph
4. Check if it is valid or not

### Loading the data
Loading the data from a csv file called `dimension_configuration.csv` into a **DataFrame** structered as **dimension_configuration** defined as a `case class`.  
- **tenant_id** : Array[Int]
- **dimension_id** : String
- **dimension_level** : String
- **parents_level** : Array[String]

The code snippet below will illustrate what have been discussed up above.
```SCALA
case class dimension_configuration(tenant_id : Array[Int], dimension_id : String, dimension_level : String, parents_level : Array[String])

def main(args: Array[String]): Unit = {
	val df = session.read.option("header", true)
	.option("delimiter","|")
	.csv("dimension_configuration.csv")  
	.map(e => dimension_configuration(e.getString(0).split(",").map(_.toInt), e.getString(1), e.getString(2), e.getString(3).split(",").map(_.trim)))
}
```

### Preparing the data / Compose and decompose the data
At this point we have 