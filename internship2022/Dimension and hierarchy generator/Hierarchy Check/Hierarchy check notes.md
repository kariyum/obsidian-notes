As for hierarchies, we would check if the path concerned is valid or not.
## Data structure
- **tenant_id** : Array[Int]
- **dimension_id** : String
- **hierarchy_id** : String
- **from_level** : String
- **hierarchy_name** : String
- **to_level** : String

As far as i can understand, we need to groupy by **hierarchy_id** to construct the path.
The path type would be a **map[String, String]** 

#### Case class definition
```scala
case class hierarchy_configuration(tenant_id : Array[Int],
								   dimension_id : String,
								   hierarchy_id : String,
								   from_level : String,
								   hierarchy_name : String,
								   to_level : String
								   )
```

### Loading the data
```scala
val df = session.read.option("header", true)
	.option("delimiter","|")
	.csv("dimension_configuration.csv")  
	.map(e => dimension_configuration(
		  e.getString(0).split(",").map(_.toInt), 
		  e.getString(1), 
		  e.getString(2), 
		  e.getString(3).split(",").map(_.trim))
	  )
```
![[show date untreated.png]]

Schema
![[schema1.png]]

Constructing the map between **from_level** and **to_level**
![[subpath.png]]

Now we can group the maps that corresponds to the **hierarchy_id**.
![[grouped on hierarchy_id + path.png]]

At this step, we have a dataframe that contains **distinct hierarchy_id** mapped to their **path**.
We would like to check the validity of the paths for their corresponding **dimension_id**

Let's start by loading the **map** of every **dimension_id** that will be called **dimension** and then perfrom our checks on the **hierarchies** if they are valid -> generate data specific to that hierarchy.

This time we will use a **UDF** *user defined function* to go over every hierarchy in the dataframe and perform the validity check

The **UDF** is used like this
```scala
dfPath.withColumn("validity", validatePathUDF(col("dimension_id"), 
											  col("path"))
											  )
```

The **UDF** is implemented like so 
```scala
val validatePath = (dimension_id : String, path : Map[String, String]) =>{  
  hierarchyValidation(dimensionMap(dimension_id), path)  
}
```

**hierarchyValidation**  function is implemented as 
```scala
def hierarchyValidation(dimension : Map[String, mutable.WrappedArray[String]], 
						path : Map[String, String]) : Boolean = {  
  // for each key in the path check whether the value in the path map exists in the array of the dimension(key)  
  for (path_key <- path.keys) {  
    val nextNode = path(path_key)  
    val exists = dimension.get(path_key) match {  
      case Some(n) => n.contains(nextNode)  
      case None => false  
    }  
    if (!exists) return false  
  }  
  true  
}
```

**NEEDS TO BE UPDATED**
This functions takes the dimension **required** to perform the checks then outputs a **boolean** value.
![[part 1 of final output.png]]
![[part 2 of final table.png]]

Now that we have validated all hierarchies we would like to start generating data base on the valid hierarchies.
![[department category hierarchy.png]]
![[Product dimension.png]]
The algorithm starts by detecting the **root** node that is not pointed at by any other node, then starts traversing the path. 
First iteration : Find the root node, checks if the next linked node is in the dimension, if yes, it procceeds with the next linked node as the current node for the next iteration, until the path is traversed or until there are nodes that don't align.
I don't feel like there are any edge cases here. 

