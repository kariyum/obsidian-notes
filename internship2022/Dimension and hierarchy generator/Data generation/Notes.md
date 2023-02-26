At the start we would get valid hierarchies then we can generate data for a corresponding hierarchy.
We need a dataframe that has the child_level and parent_level columns
We either can get them from the **hierarchies dataframe** and **explode the path column** otherwise we can fetch the **hierarchy csv file** and then filter by **tenant_id**, **dimension_id** and **hierarchy_id** to finally have the valid hierarchies.

#### Which one is better ?
**Common computations** :
	- Validate the dimensions
	- Validate hierarchies

**Explode path column**
	- Create a column names **from_level** and **to_level**
	- Create rows for every entry. *NOTE : hierarchy path is always 1 to 1 but have many keys*

**Filter unvalid hierarchies**
	- Load all hierarchies
	- Go through all the rows and keep only the valid ones that are checked by 
		- **tenant_id**, **dimension_id** and **hierarchy_id**
- For now I'm going with the explode path column it seems more robust since we don't have to compare three strings if we're going with the filter solution.
### Solution
We fetch valid hierarchies then explode the each path column to 2 columns labled **from_level** and **to_level** and **duplicate** rows for each entry in the path map.

#### Step 1 
At this point the map in the path column for each row contains many entries, we would like to explode on that to have two columns one that holds **keys** and the other holds **values**.

The code snippet below will take care of this step.
**Starting position** 
![[Initial dataframe.png]]
```scala
df.select(df.columns.map(c=>col(c)):_*, explode(col("path")).as(Seq("from_level", "to_level"))).show(false)
```

The `withColumn` method cannot append 2 columns at a time so we couldn't use it here.
Rather than typing all column names i thought it would be nice to pass an array of column names like so `df.columns.map(c=>col(c)):_*`, it threw an error.
If we're keeping the same schema and only appending 2 columns we could use `df.select($"*", explode(...))`, import `import sparksession.implicits._`

```scala
val dfExploaded = df.select(df("tenant_id"), df("dimension_id"), df("hierarchy_id"), df("hierarchy_name"), explode(col("path")).as(Seq("from_level", "to_level")))
```

The code snippet above will yield this
![[exploaded dataframe.png]]

#### Step 2
The schema that we're looking for is :
![[goal schema.png]]
The schema that we currenctly have after dropping **hierarchy_id** and **hierarchy_name** columns
#### Ready to go data
- **tenant_id**
- **dimension_id**
- **child_level** --> from_level
- **parent_level** --> to_level

#### Generated data
- **parent_element_id**
- **child_element_id**
- **child_element_label**
- **parent_element_label**

for each row we can add a new column that contains a tuple of (parent_id, child_id, parent_label, child_label)

We somehow need to detect and understand the type of the data that we're going to generate upon to be logic :
	- Ad week -> generate a "yyyy-mm-dd" format starting on wednesdays 
	- Fiscal week -> "yyyy-mm-dd" format starting on wednesdays 
		- **what is the difference between *ad week* and *fiscal week* ?**

At this points i don't know what should be done, so i will start by implementing UDFs for each new generated column such as
	- parent_element_id
	- child_element_id
	- child_element_label
	- parent_element_label
- It seems like we should be either generating column per column, since we probably need to refer to the label if we're going to generate an id or generating rows to have access to the 2 columns ( id, label ) at once.

#### Solution 1: Generating column per column
- which column should be generated first ? the id or the label ?
- Is it more efficient to generate column per column ?
--> This will defenitely not work, since there is a link between parent and child elements for example : Day at a specific Fiscal week, the day value should be in the Fiscal week range...
**~~VALID~~**

#### Solution 2 : Generating row per row 
- After generating 4 columns we could join the two dataframes but that would take lots of time...

#### Solution 3 : Generating one column that contains generated data
- I think we better off using ==select== on the dataframe with a function that returns 4 columns... this needs some work obviously
- Generate a map like data structure that maps the 
	- child_id -> a String
	- parent_id -> a String
	- child_label -> a String
	- parent_label -> a String
- I have opted for this solution since it was the most convenient one, we do need to perfom the explode function on the generated column to turn it into 4 columns total.

**Success !** This last solution does work and it is relatively efficient, since we're doing many operation on the dataframe we better off **cache** each result of the dataframe.

#### Steps taken :
1. Implement UDF function that generates a map of 4 attributes like so
```scala
val generateColumns = (dimension_id : String, 
					   from_level : String, 
					   to_level : String) =>{ 

  generateColumnsFunction(dimension_id : String, 
						  from_level : String, 
						  to_level : String)  
}
val generateColsUDF = udf(generateColumns)

```

The generateColumnsFunction needs some work but here's a first look
![[generateColumnsFunction.png]]

Here's how we apply the UDF 
```scala
val dfParentToMany = dfExploaded.select($"*", generateColsUDF(col("dimension_id"), col("parent_level"), col("child_level")).as("parent_to_many")).cache()
```

**Note** here that we have **cached** the result, since we have more operations to perfrom on the resulting dataframe and we know that spark re-evaluates the dataframe each time we perfrom an action.

## Generating time/calendar specific data
For **ad week** and **ad period** we would like to give arguments as the beginning year date and start counting from there.

## Inconsistency problem
Now after noticing the inconsistency in the **hierarchy path** we would like to fix that.
**The problem**
![[problem pic 1.png]]
In this hierarchy we have 3 nodes. The inconsistency occurs when we're generating some data. The p1 node is called the queen, we will only have 1 instance of the queen. The queen now will have many instances of it's child. That child also will have many instances of its child. 

My first solution which is dividing the path into 2 columns; from_level and to_level; that way we have already lost the meaning of the path and so when we want to regroup the generated data we wouldn't be able to link the middle node that is a parent and a child at the same time to the path, since the data doesn't match.

The solution is to generate the data when they are still in the same map, that way we make sure that they the node that is parent and child at the same time shares the same data. This way we are able to regroup the nodes and reconstruct the path having consistent data.

As of my first iteration of this solution this is what i came up with
![[parent and child tree iter 1.png]]
I have drawn this tree manually from the output below
![[output iter 1.png|1000]]
This is just a test case for the algorithm tested with 1 child, 2 children and 3 children for each node other than the leaf node (the node that points to no other node)
![[algo iter 1.png|1000]]
I used ![[counter object.png]] just to distinguish keys since we can't have duplicate keys in the map. This is just for testing.

Now we just need to populate the children with some logic / readable data. But **the question here is do we need to use dimension_id to distinguish between the data that we're going to generate?** 
I'm generating data with respect to the given hierarchies. I'm making sure that the data is readable and consistent within the hierarchy.

```scala
  def recChild(parent : Node, path : Map[String, String], accumulatedMapRes : Map[Node, Array[Node]] ): Map[Node, Array[Node]] = {  
//    path.foreach(println)  
//    println(path.count(_=>true))  
    val newPath : Map[String, String] = path.filterKeys(_!=parent.level)  
//    println("The queen is "+ parent.level)  
    val children = generateChildren(path.get(parent.level) match {  
      case Some(n) => n  
      case _ => ""  
    }, parent)  
    val res : Map[Node, Array[Node]] = children.flatMap(child=>{  
      recChild(child, newPath, accumulatedMapRes)  
    }).toMap  
    if (newPath.count(_=>true)==0) return Array(accumulatedMapRes, Map(parent->children)).flatten.toMap  
//    Array(recChild(Node("id","label", newPath.keySet.diff(newPath.values.toSet).head), newPath, accumulatedMapRes), Map(parent->children), res).flatten.toMap  
    Array(res, Map(parent->children)).flatten.toMap  
  }
  
```


![[tree.png]]
New way to look at the tree is this
A parent has many children and so a parent points to it's children.