we need a function that sets a key in the Redis cluster

**Redis.set**(key: String, value: String) **shoud do the following**
- **dbsize < DB_LIMIT**
	- calls **RedisClient.set**
		- sets pair of provided key:value
	- calls the **EvictionStrategy.init**
		- initializes the attribute that the eviction strategy will use (frequency OR timestamp: we can call it **score**)
- **ELSE**
	- calls **EvictionStrategy.evict**(key_to_add: String, value: String)
		- will **EVAL**uate the lua script that will remove a key and add the key_to_add + inits it in the hashmap

**Redis.get**(key: String) **should do the following**
- checks if the key exists --> 
	- **FALSE** : pass
	- **TRUE** : return the resource info **AND**
		- call **EvictionStrategy.got**(key: String)
			- the call will **update score** of the correspoding resource, either timestamp or frequency