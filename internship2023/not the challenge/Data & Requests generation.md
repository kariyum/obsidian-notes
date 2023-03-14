Random data shouldn't be a problem
Random requests is an issue because the algorithm would not be able to pick patterns of the requests. Take for example LRU (least recently used) if the requests are random, there is a small chance that the same request would appear next to each other, which does not describe well the real world case.

