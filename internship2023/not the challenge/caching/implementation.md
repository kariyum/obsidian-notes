
There are many redis clients that could be used with scala / java. Filtered them to these options :
- Jedis : Java client
- redis-scala: Scala client. Single threaded but able to open clientPool
Both are maintained, a response in SO back in 2011 used jedis with scala said that is good enough

--> redis-scala
[jedis vs scala-redis in mget (github.com)](https://gist.github.com/felipehummel/3909583)
mget performance

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

[Communicating Through RabbitMQ In Scala - Knoldus Blogs](https://blog.knoldus.com/communicating-through-rabbitmq-in-scala/)
[Custom atomic operations in Redis using LUA | by Rafael Sales | Medium](https://medium.com/@rafasales/custom-atomic-operations-in-redis-using-lua-a56e37e79299)

---

#### write strategy implementation
starting with the simplest one which is the **write-around** strategy
> data is written directily in the database and only makes it to the cache after a read request

write strategies are very difficult in this situation because the models are linked
what if i make many changes in the cache would they also be registered later on in cassandra ?
the most complex one is the **WriteBack** strategy, w'll have to figure that out later, for now let's focus on **WriteThrough** and **WriteAround**

**WriteThrough**
- post | put | delete ->
	- validate input params before caching it
	- if valid input params
		- either export cassandra queries that are going to be executed so the cache service will just issue the requests directly
		- the cache service will execute the same event service functions, this is the simplest way