```

javac -classpath lib/jade.jar -d classes "src/examples/venteEnchere/*.java"

java -cp lib/jade.jar;classes jade.Boot -gui -agents ty:examples.venteEnchere


java -cp lib/jade.jar;classes jade.Boot -gui -agents buyer1:examples.venteEnchere.BuyerAgent;buyer2:examples.venteEnchere.BuyerAgent;buyer3:examples.venteEnchere.BuyerAgent;s:examples.venteEnchere.SellerAgent

```

