# Notes
4. Im not taking into account the capacity of the slotset when linking slots with that slotset

# Questions
2. the eviction policty stores it's attributes like frequency or timestamp in a seperate scala collection that could effectively scale safely with concurrency, but is it the best way though ?
	1. the other way would be to store the eviction attributes along with the value of the resource in redis meaning; in redis we would store a pair like this (key, {"resource": "jsonStringValue", "frequency": 123, "timestamp": 19401489})
	2. benefits of each ?
	3. **seperate collection** qualities
		1. maintained soloely which is not okay since the collection and Redis should be tightly synced
		2. faster; using a HashMap for frequency is O(1) access
		3. deployment issues
	4. **in redis** qualities
		1. the key has it's attributes and the eviction policy's attributes along with it. easy, very nice
		2. when evicting a key, should iterate on all keys then decide which key should be evicted. does this seem slower ?
		3. loosely coupled
	5. **LUA SCRIPTS**
		1. lua scripts executes atomically in Redis server meaning that all while executing the lua script all other operations are blocked. The scripts could be used to determine which key to evict !
		2. [[#lua scripts tests]]

# What is done
2. Cache-aside strategy 100%
3. Read-Through strategy 100%
4. Verified that cassandra doesn't cache query results

# TO DO:
1. security-service:
	1. in the cookie validation step, check note. [[internship2023/not the challenge/security service/Notes#future updates]]

5. ==verify that the cache and the application are using the same caching strategy==
	1. sending a request to the event-service to set the cache config
	2. the event service should take care of sending the cache configuration to the cache service
	3. **setting the caching strategy will maybe result in some problems when deploying on kubernetes..** 
	4. either deploy server for each strategy or make it dynamic
		1. dynamic setting => not going great...

7. implement tests for each eviction policy
	1. FIFO
	2. LFU
	3. LRU
	4. OPR will be determined while generating the data + single threaded env since the order of incoming requests matter significantly

8. improuve the way we generate requests
9. dynamic load increase ?
10. when evicting a key, how about evivcting all keys that are under **min_score * 1.2** ? that way we ensure that when the cache is at full capacity adding a new key won't always result in evicting

## final product
Benchmarking caching strategies
	--> architecture wise 
		--> Read strategy & Write strategy + eviction strategy

we need write requests for the final benchmarks
verify that cassandra doesn't cache query results

# lua script tests
in many places, people do not encourage calling keys in lua sripts that are not passed as arguments which is what we are doing and it makes me drift away from this approach
1. scalabitlity tests
we are going to send multiple concurrent requests and check if the cache tops out at the specified CACHE_CAPACITY = 10
![[Pasted image 20230413095011.png]]
here as you can see we have 14 keys in redis which is more than the limit
To make sure that this behaviour is from the concurrency let's send sequential requests. We should expect 10 after the simulation when executing dbsize
![[Pasted image 20230413095322.png]]
Well that concludes it. We now know that this approach is not the best way to go

Let's add redis.replicate_commands() and try it out
	did not make a difference

Checking scala code, there might be a bug
let's push all logic in lua script
- lua script is always called even for a simple set statement ? not great since all other operations will be blocked... but just for curiosity
=> works fine and going forward with this approach

# Questions !
1. generating requests
	1. should we extend the available_resources with duplicates ??????? check user.py line 91 OR add fetch frequencies for all of those resources that would help
	2. write requests cannot be replayed, even for some read requests that fetches unavailable data should we take care of it ?

# Bugs + todo
1. event service
	1. please handle edge cases with exceptions. not important for now
	2. can create many promotions for the same slot. FIXED
	3. ==write strategy; cache invalidation==
2. cache service
	1. ==write strategy==
	2. metrics [[cache invalidation + performance metrics]]
3. python bugs; one of which, fetching an event that doesn't exit lmao ~~FIXED~~
4. generate write requests in a very logical way; frequency based just like read requests
5. visualize the incoming requests in a meaningful manner
	1. how many new resources have been created
	2. how many unique resources have been fetched
		1. at each point of time !!
		2. this should be UI based. Can python subscribe to an event source ? for sure
6. when issuing write requests sometimes we get a 400 bad request especially when we are  using write strategy such as WriteThrough or WriteBack. WriteAround won't make a difference. ==weird ass bug and it's fixed LOL==
7. remove the Await.result when caching the entity
	1. ![[Pasted image 20230505114945.png]]
	2. DONE

5th of may
- metric python plots. start-15th of may
- enhance requests generation
	- poisson rule when sending requests
	- make many users DONE
- kubernetes deployment
- when using any other write strategies it might query cassandra directly without the cache when reading if a resource exists or not; when posting a new resource essentially -- OK
- should i clean data before hypothesis testing ?
- problem while sending many requests, we're apparently creating new connection each request which will exhaust the number of ports eventually.
	- create a session for each client/actor. --solved
- punishing keys that are not referenced
- evicting keys that are less to a certain score e.g. 120% of minimum score
- apparently cassandra could handle the load, which is weird. but it doesn't matter
- save benchmarks
	- for each run save every plot in a separate directory
	- steps
		- create directory of the strategy
		- create sub-directory for that run
		- save every plots in its corresponding folder
- add user read probability for each strategy in the name
- grid search like benchmarks with logarithmic scale ?

![[Pasted image 20230517094239.png]]

![[Pasted image 20230517103248.png]]


![[Pasted image 20230517103300.png]]

make a video of 100 plot charts and then draw a normal distribuion mean and variance... good for presentation, very good.


At first let's test test for random requests, this will benchmark caching strategies without prior knowledge on clients' behavior. Later on we will specify some patterns for the requests to tell if it is worth it to study your own client's behavior and implement your own eviction strategy, if there are any changes.

![[Pasted image 20230521171926.png]]

# Analysis
how are we going to analyse all of these benchmarks ?
well my first instinct will be to select the client's behavior and within that we will determine which is the best strategy for the company to implement. To make this as generic as possible is to provide a way to select equal attributes that way we can for example set the comparison on other levels.

compare a metric from a selected pool of strategies.
0.2 read
![[Pasted image 20230522224625.png]]

![[Pasted image 20230522225326.png]]


