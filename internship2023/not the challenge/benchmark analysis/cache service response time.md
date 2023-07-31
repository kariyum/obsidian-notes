## Comparing cache service response time and server response time when using cache in read through and write through/back

- read through / write through
	- LFU
	![[Pasted image 20230529154325.png]]

	- FIFO
	![[Pasted image 20230529154628.png]]
	- LRU
	![[Pasted image 20230529154734.png]]


- read through/ write back
	![[Pasted image 20230529155010.png]]
	
	---

	![[Pasted image 20230529155036.png]]

	---
	
	![[Pasted image 20230529155103.png]]

# write through 0.2

- Average response time
	![[Pasted image 20230529211146.png]]

- Cache service average response time
	![[Pasted image 20230529211318.png]]

Notice here how the mean is so different when we are measuring the cache service response time. Note that the cache_service_response time is measured in the cache service. Before completing the request. I think the difference is because of http protocal. We can look into this in the perspectives.

==PLEASE ANNOTATE X AND Y AXIS==


![[Pasted image 20230529212727.png]]
