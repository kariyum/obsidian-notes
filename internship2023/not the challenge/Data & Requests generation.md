generating data


---
We can generate requests with rules like this:
	if a client has fetched an event, there is like a 80% propability that it will fetch some of the slotsets
	same for other objects (slotsets, slots and promotions)
How are we going to implement this ?

**Implementation**
for read
1. pick event (from csv file that should be updated for each event creation) 
2. 0.8 chance to pick a slotset from that event if not all
	1. 0.2 chance to pick another event (missclick scenerio)
3. 0.9 chance to pick a slot from the fetched slotset if not all
4. 0.99 chance to pick a promotion from those slots if not all

for write
1. write event
2. 0.8 chance to write 1 or MANY slotsets for that event
3. 0.8 chance to write 1 or MANY slots for a subset or ALL slotsets
4. 0.8 chance to write 1 promotion for each slot or some
this method is much more realistic but we defenitely need to visualize the type of requests that are being sent 
for implementation wise, we can implement a User class that will have methods as such
1. create methods for all models
2. read methods for all models
3. **behave** method will be used to generate this user's request
-> collect all generated requests, merge them then simulate

---
**Gaussian method**
for now, we are generating normally distributed data that will be used to map each unique point to a resource_id, and so every resource_id will be requested many times
1. Generate an array of normally distributed floats centered on zero
2. Bucket the observations to reduce the number of unique floats to the number of resource_ids
	1. instead of bucketing we can implement a function that squeezes \[data.min, data.max] to \[1, resource_id.length]
3. Map each data point to a resource_id

well this method is pretty simple but it favors the **LFU** strategy...
