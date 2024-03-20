# TP 4 - Spark
### 1/
```scala
val textFile = sc.textFile("tp4/test.txt")
```
![[Pasted image 20221011145611.png]]
### 2/
```scala
textFile.collect()
```
![[Pasted image 20221011145704.png]]
### 3/
```scala
textFile.count()
```
![[Pasted image 20221011145816.png]]
### 4/
```scala
val lower = textFile.map(_.toLowerCase)
```
![[Pasted image 20221011150026.png]]

### 5/
```scala
val lower_flat = lower.flatMap(_.split("\\s+"))
```
![[Pasted image 20221011150233.png]]

### 6/
```scala
lower_flat.collect()
```
![[Pasted image 20221011150333.png]]

### 7/
```scala
val words = lower_flat.map((_,1))
```
![[Pasted image 20221011150946.png]]

**Affichage de donnée**
![[Pasted image 20221011151138.png]]

### 9/
```scala
val wordCount = words.reduceByKey(_+_)
```
![[Pasted image 20221011151259.png]]

**Caching data**
![[Pasted image 20221011151319.png]]

```scala
wordCount.collect()
```
![[Pasted image 20221011151418.png]]
### 11/
```scala
wordCount.count()
```

### 12/
![[Pasted image 20221011151600.png]]
```scala
filered.saveAsTextFile("output")
```
![[Pasted image 20221011151915.png]]
La sortie de spark est bien enregistrée sur le système hadoop

### Top 3
```scala
filtere.map(_.swap).top(3)
```
![[Pasted image 20221011151842.png]]

### Trouver le caractère le plus fréquent
```scala
words.flatMap(_._1.toCharArray).map((_,1)).collect()
```
![[Pasted image 20221011152146.png]]

### 15-16/
```scala
val caracters = words.flatMap(_._1.toCharArray).map((_,1))
caracters.reduceByKey(_+_).map(_.swap).top(1)
```
![[Pasted image 20221011152349.png]]
