**objects** name **extends** name => means whatever we write in the curled brackets will be executable as its own application

**Types** : Int, Boolean, Char, Double, Float, String

Expressions | Methods
---|--
**String concatination**| "hello " ++ "world"
**String size** | "hello".size => prints 5 
**Range of numbers** | Range(1, 2, 3, 4, 5, 6, 7, 8, 9, 10)

#### String manipulation
Concatination => "Hello" ++ "world"
Size => "hello".size, prints 5
"hello".toUpperCase

#### Tail recursion
It is executed in constant space
It doesn't use stack / heap
```scala
def factorial(n: Int): Int = {
  @tailrec
  def iter(x: Int, result: Int): Int =
    if (x == ) result
    else iter(x - 1, result * x)

  iter(n, )
}
```

#### Case class
*I think case classes are used like a **data structure** where there are not methods defined.*
Like so
```scala
case class Song{
	title : String,
	singer : String,
	duration : Int,
	year : Int
}
```
create a value of this type
```scala
val s1 = Song("Sugar", "Robin Schulz", 300, 2015)
```

They are most likely used to match cases like dis
```scala
calc match {
  case Calculator("HP", "20B") => "financial"
  case Calculator("HP", "48G") => "scientific"
  case Calculator("HP", "30B") => "business"
  case _ => "Calculator of unknown type"
}
```

#### Array
Arrays preserve order, can contain duplicates, and are mutable.
```scala
val numbers = Array(1, 2, 3, 4)
println(numbers.mkString)
println(numbers(1))
```

#### List
Lists preserve order, can contain duplicates, and are immutable.
```scala
val numbers = List(1, 2, 3, 4)
```

#### Sets
Sets do not preserve order and have no duplicates
```scala
val setOfNumbers = Set(1, 1, 2, 3, 3)
```

#### Tuple
A tuple groups together simple logical collections of items without using a class.
```scala
val hostPort = ("localhost", 80) OR "localhost" -> 80
val host = hostPort._1
val port = hostPort._2
```
Matching
```scala
hostPort match {
  case ("localhost", port) => ...
  case (host, port) => ...
}
```

#### Maps
```scala
Map(1->2)
Map("foo" -> "bar")
```
Maps can contain maps as values or even functions like so
```scala
Map("timesTwo" -> { timesTwo(_) })
```

Run case
```scala
val m = Map("one"->1, "two"->2)
val option = m.get("one") // return an option
val value = if (option.isDefined) option.get else 0
// for simplicity
val value = option.getOrElse(0) // returns the value if defined else 0
```

Pattern marching fits naturally with **option**
```scala
val result = m.get("key") match{
	case some(n) => n * 2
	case None => 0
}
```

### Functional Combinators
#### map
```scala
val numbers = List(1, 2, 3, 4) // immutable
numbers.map((i: Int) => i *2 ) // List(2, 4, 6, 8)
numbers.map( timesTwo ) // List(2, 4, 6, 8)
numbers map timesTwo
```

#### foreach
It is like map but return **nothing** it is used for side-effects only
```scala
numbers.foreach( (i:Int)=> i*2) // return nothing (void == unit)
```

**filter**
Removes any elements where the function you pass in evaluates to false.
```scala
def isEven(i : Int) : Boolean = i % 2 == 0
val res = numbers.filter(isEven)
OR 
val res = numbers.filter( _ % 2 == 0 )
// print res
res.foreach( (i: Int)=> print(i + " ") ) //=> 2 4 6 8 10
```

**zip**
Zip aggregates the contents of two lists into a single list of pairs
Aka makes a tuple out of two lists just like python
```scala
List(1, 2, 3, 4).zip(List("one", "two", "three", "four"))
=> List( (1,"one"), (2, "two"), (3, "three"), (4, "four"))
```

#### partition
Paritions the list with the predicate
```scala
val res = range.partition( _ %2 == 0)  
res._1.foreach((a:Int)=> print(a + " "))  
println("")  
res._2.foreach((a:Int)=> print(a + " "))
```

#### find
returns the first element of a collection that matchs a predicate function
```scala
numbers.find( (i:Int) => i > 5) // returns an option
```

#### drop & dropwhile
`drop` drops the first i elements
```scala
numbers.drop(5) 
// 1, 2, 3, 4, 5, 6, 7, 8 => 6, 7, 8
```

`dropwhile` removes the first element that match a predicate function.
aka drop until the predicate is false
```scala
numbers.dropWhile( _ % 2 != 0 ) 
// from 1, 2, 3, 4, 5, 6 => 2, 3, 4, 5, 6
// notice here that other odd elements weren't dropped that's be cause they are shielded by "2" the first element that is not odd.
```

#### flatMap
combines flattening and mapping aka applies a function then flattens the lists
```scala
nestedNumbers.flatMap( x=> x.map( _ * 2 ))
// is equivalent to
nestedNumbers.map ((x : List[Int]) => x.map( _ * 2 )).flatten
// applies a function for every list in nestedNumbers then flattens the result
```

**All of these combinators also work on map**
**filter on map**
```scala
extensions.filter( (namePhone: (String, Int)) => name._2 < 200)
// better way to write it
extensions.filter ( {case (name, extension) => extension < 200})
```

#### seq
Scala Seq is a trait to represent immutable sequences. This structure provides index based access and various utility methods to find elements, their occurences and subsequences. A **Seq maintains the insertion order**.
```scala
val seq: Seq[Int] = Seq(1, 2, 3, 4, 5)
val isPresent = seq.contains(4);
val contains = seq.endsWith(Seq(4,5));
var lastIndexOf = seq.lasIndexOf(5);
```

#### Word occurrences
```scala
val s = Seq("apple", "oranges", "apple", "banana", "apple", "oranges", "oranges")

s.groupBy(identity).mapValues(_.size)
```
