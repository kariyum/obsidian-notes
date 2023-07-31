# Implicit
- How to organize implicit values logically
For example:
```scala
case class Purchase(nUnits: Int, unitPrice: Double)

/**
- Most used api: Sort by totalPrice
- Less used api: Sort by unitPrice
- Less used api: Sort by nUnits
*/

object Purchase { 
	// companion object by default this sorting function will be used 
	implicit val orderByTotalPrice : Ordering[Purchase] = Ordering.fromLessThan(..)
}

// importing these objects._ will override the ordering function
object OrderBynUnits {
	implicit val orderBynUnits : Ordering[Purchase] = Ordering.fromLessThan(..)
}

object OrderByUnitPrice {
	implicit val orderByUnitPrice : Ordering[Purchase] = Ordering.fromLessThan(..)
}
```


```
hor, depth, aim
forward (+5, +previous_aim * head, 0)
down    (+0, +0, +head=5)
forward (+8, +5 * head, +0)
up      (0, 0         , -head)

((a, b), c)
forward (5, 0, 0)
down    (5, 0, 5)
forward (13, 0, 5)
up      (13, 0, 2)


```

