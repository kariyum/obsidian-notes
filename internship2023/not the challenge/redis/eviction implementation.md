There are many ways to implement the eviction policy
1. scala collection
2. redis sorted sets & redis transactions
	- holding an independent sorted set will introduce many troubles to sync
	- O(1) to determine which key to evict
3. lua script without redis sorted sets
	- fast and reliable
	- O(n) to decide which key to evict
	- only executed when we want to evict a key

### data model
we will still have the key same as before with the correspoding resource but we will add a **hashmap** collection that will hold **last_access_times** or **frequent_calls** that later will be used by the eviction strategy.

### LRU
```
-- Define the maximum number of keys to store
local max_keys = 1000

-- Get the current number of keys
local num_keys = redis.call('DBSIZE')

-- If we're already at the maximum number of keys, evict the least recently used key
if num_keys >= max_keys then
    -- Get all the keys and their last access time
    local keys = redis.call('KEYS', '*')
    local last_access_times = {}
    for i,key in ipairs(keys) do
        last_access_times[key] = redis.call('GET', key .. ':last_access_time')
    end

    -- Sort the keys by their last access time
    table.sort(keys, function(a,b) return last_access_times[a] < last_access_times[b] end)

    -- Delete the least recently used key
    redis.call('DEL', keys[1])
end

-- Set the last access time for this key
redis.call('SET', KEYS[1] .. ':last_access_time', ARGV[1])

-- Return nil to indicate success
return nil
```

# problemo
There are still some concurrent behaviour with lua scripts as it is evaluated with scala.. that's why i pushed all logic in the lua script; meaning that we run the lua script to add a key wether or not the cache can hold more keys

# why lua
since the eviction strategy will read too much data from redis cluster it is wise to execute it all in the redis cluster that way we improuve network traffic