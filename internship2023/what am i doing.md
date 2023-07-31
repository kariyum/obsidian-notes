### 15th of may
- test if you can run the simulation with many users at the same time ? possible situations: 
	- will there be bugs
		- when generating requests ?
			- could a user request resource that is not his ?
		- server side ?
			- handle many users not in terms of performance but of reading and writing data, no conflicts. There shouldn't be any conflicts because we are using server side cookies which will verfiy who the user is.
-> ALL IS GOOD


```cmd
start python3 cache-sse.py & start python3 main.py
```
