# What will be discussed in this note ?
- This is a documentation about the whole **dimension check** work. Starting from loading the data to the actual algorithm that validates the dimension. The better approach.
 
### Loading the data
Loading the data from a csv file called `dimension_configuration.csv` into a **DataFrame** structered as **dimension_configuration** which defined as a `case class`.  
- **tenant_id** : Array[Int]
- **dimension_id** : String
- **dimension_level** : String
- **parents_level** : Array[String]

The code snippet below will illustrate what we have so far.
```SCALA
case class dimension_configuration(tenant_id : Array[Int], dimension_id : String, dimension_level : String, parents_level : Array[String])

def main(args: Array[String]): Unit = {
	val df = session.read.option("header", true)
	.option("delimiter","|")
	.csv("dimension_configuration.csv")  
	.map(e => dimension_configuration(e.getString(0).split(",").map(_.toInt), e.getString(1), e.getString(2), e.getString(3).split(",").map(_.trim)))
}
```

The **print_schema** method will output this
![[Pasted image 20220719085345.png]]

### Preparing the data / Compose and decompose the data
This section is critical because our goal is to construct the graphs stored as a **map** data sctructure of every **dimension_id** for every **tenant_id**. 

This is an image that shows the **initial state** and the **goal state** of the dataframe.
- Initial sate
![[Pasted image 20220719085520.png|550]]
- Final state
![[Pasted image 20220719085823.png|1000]]
Final state schema
![[Pasted image 20220719085943.png]]

#### Why did we set this as our goal ?
This dataframe structure will make it easy for us to **track** each **dimension_id** and mention whether it is **valid** or not. *probably will have a new column storing the validity of the dimension_id*

#### Steps taken :
- After having the strings of the **parents_level column** trimmed ( to ensure that the comparison is done correctly later on ) we would construct a new column containing the graph as a **map**[String, Array[String]] that will be needed to validate the dimension hierarchy just as follows.

**We have this**
![[Pasted image 20220719085520.png|550]]
This code snippet is enough to construct the map column between **dimension_level** and **parents_level**
```scala
val dfGroupedOnDimensionLvl = df.withColumn("mymap", functions.map(col(df.columns(2)), col(df.columns(3)))).drop(df.columns(2)).drop(df.columns(3))
```

![[Pasted image 20220719093346.png|600]]

This is the corresponding schema
![[grouped on dimension level schema.png]]

The next step is to concatinate the maps grouped by **dimension_id** 
![[array of maps schema.png]]
Code snippet
```scala
val dfGroupedOnDimensionId = dfGroupedOnDimensionLvl.groupBy(df.columns(1)).agg(first(df.columns(0)) as df.columns(0), collect_list("mymap").as("mymap"))
```

The corresponding schema
![[Grouped on dimension_level schema.png]]
Notice here that we have an array of maps, it would be great if would have a map of strings

That it achieved with this code snippet
```scala
val joinMap = udf { values: Seq[Map[String, Array[String]]] => values.flatten.toMap }  
val dff = dfGroupedOnDimensionId.withColumn("mymap", joinMap(col("mymap")))
```
![[map of strings to arrays.png]]

The schema
![[goal dataframe schema.png]]

It would have been great to collect all sub maps as maps but it wasnt possible since there is no equivalent **collect_list** aggregate function to **collect_map**.

Now that the rows are grouped into maps it is easy to fetch each map and check its' validity.
Next section.

### Check the validity of the dimension
### Approach
To find cycles in a graph a straightforward solution would be placing nodes in their corresponding levels and if somehow we find a node that cannot be placed that means there is a cycle otherwise we iterate until the nodes are all placed.
**Placing nodes on levels rule** : The node is eligible to be placed in this level only if it doesn't point to any other node. In that case we pop that node and we iterate over.

#### Spark way
The spark solution is very slow since it has to manipulate dataframes.
![[spark check configuration.png]]

```scala
def checkConfiguration(ds : Dataset[Row], session : SparkSession): Boolean = {  
  // delete lines where the parents_level isn't in the dimension_level column  
  // repeat until there are no lines or until you can't delete anymore => that's a cycle  def rec(dsAux : Dataset[Row], session : SparkSession, previousCount : Long) : Boolean = {  
    if (dsAux.count() == 0) {  
      true  
    } else {  
      dsAux.createOrReplaceTempView("table")  
      var ds1 = session.sql("select * from table MINUS (select * from table where table.parent_level not in (select table.dimension_level from table))")  
      if (ds1.count() == previousCount) return false  
      rec(ds1, session, ds1.count())  
    }  
  }  
  rec(ds, session, ds.count())  
}
```

#### Scala way
#### Reading the data

```scala
val dimensionIdMappedToGraph : Map[String, Map[String, mutable.WrappedArray[String]]]= dff.select("dimension_id", "mymap").collect().map(k=>{  
  val entry = k.getString(0)-> k.getAs[Map[String, mutable.WrappedArray[String]]](1)  
  entry  
}).toMap
```

![[map fetched from dataframe.png|1400]]
I think we can use seq instead of WrappedArray here... still not sure
The most important part is that we should inform the compiler the expected type.
```scala
// for every dimension_id we're going to verify whether the graph == dimensionIdMappedToGraph(dimension_id) is acyclic or not  
for (dimension_id <- dimensionIdMappedToGraph.keys) {  
  val subMap = dimensionIdMappedToGraph(dimension_id)  
  // 1. extract a set of parent_level Strings that aren't present in the set of keys  
  // 2. remove the entries (keys) from the map      
  // 3. loop until there aren't any keys left -> returns true VALID      
  //    or until we can't delete anymore keys -> return false CYCLE DETECTED  

  def rec(argMap : Map[String, mutable.WrappedArray[String]]) : Boolean = {  
	// 1.        
	// a. Get the set of parent_level        
	val parentLvlSet = argMap.values.flatten.toSet  

	// b. Get the set of keys        
	val dimensionLvlSet = argMap.keySet  

	// c. Compute the difference        
	val toGoSet = parentLvlSet.diff(dimensionLvlSet).map(element => {  
	  element  
	}).toArray  

	// 2. Remove those values from the m(dimension_id) values        
	val newMap = argMap.mapValues(e =>{  
	  e.diff(toGoSet)  
	})  

	// 2.bis. Remove keys from the map        
	val updatedSet = newMap.filterKeys(key=>{  
	  newMap(key).count(_=>true) != 0  
	})  

	// 3. loop or return        
	val oldSetCount = parentLvlSet.count(_=>true)  
	val newSetCount = updatedSet.values.flatten.toSet.count(_=>true)  

	if (newSetCount == oldSetCount) return false  
	if (newSetCount == 0) return true  

	rec(updatedSet)  
  }  
  val valid = rec(subMap)  
  println("Dimension " + dimension_id + " validity is " + valid)  
}
```
Over.

