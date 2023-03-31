
There are many redis clients that could be used with scala / java. Filtered them to these options :
- Jedis : Java client
- redis-scala: Scala client. Single threaded but able to open clientPool
Both are maintained, a response in SO back in 2011 used jedis with scala said that is good enough

--> redis-scala

---

### communication between cache-service and event-service
- http : 
	- this allows a secure connection between the cache-service and event-service with the keep-alive header
	- easy to use and can send cookies through headers
- udp :
	- faster than http
	- doesn't require handshake and connection is persistent through the whole runtime
- websockets :
	- I haven't looked into this, but i think it's nice since it does not rely on request/response type of communication. Therefore the event service will not wait for the response from the cache-service when posting data into the cache
- message queues (producer / consumer)
	- requests are asynchronous and does not require a response

---

### cache invalidation
- after a post request
	- invalidate / update the **user_id/model?** key