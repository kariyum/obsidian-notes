lua scripts
[Chapter 11: Scripting Redis with Lua | Redis](https://redis.com/ebook/part-3-next-steps/chapter-11-scripting-redis-with-lua/)

### LUA
Technically speaking, when executing a Lua script with EVAL or EVALSHA, the first  
group of arguments after the script or hash is the keys that will be read or written  
within Lua (I mentioned this in two notes in sections 11.1.1 and 11.1.2). This is primarily  
to allow for later Redis cluster servers to reject scripts that read or write keys  
that aren’t available on a particular shard. If we don’t know what keys will be read/  
written in advance, we shouldn’t be using Lua (we should instead use WATCH/MULTI/  
EXEC or locks). As such, any time we’re reading or writing keys that weren’t provided as part of the KEYS argument to the script, we risk potential incompatibility or breakage  
if we transition to a Redis cluster later.

