[Brief Overview of Caching and Cache Invalidation | CodeAhoy](https://codeahoy.com/2022/04/03/cache-invalidation/)

# Eviction policy
Once the cache is full, any requests to add items to the cache might cause existing items to be removed.
Cache eviction policies: Least-Recently-Used, Least-Frequently-Used or FIFO.

[Caching Strategies and How to Choose the Right One | CodeAhoy](https://codeahoy.com/2017/08/11/caching-strategies-and-how-to-choose-the-right-one/)

# Cache-Aside
![[Pasted image 20230227132637.png]]
- General purpose, **read-heavy workloads** 
- Systems using cache-aside are resilient to cache failures because they are independent; database, cache
- The data model in cache can be very different than the data model in the database
- The application is responsible for fetching data from the database and populating the cache

Cons:
- Write strategy: writes to database and ignores the cache 
- The cache will eventually be outdated / inconsistent with the database
	- use TTL, continue serving stale data until TTL expires OR
	- invalidate the cache OR
	- use appropriate write strategy
- Loads data lazily, only when it is first read

# Read-Through Cache
Read-through cache stays in-line with the database. Where there is a cache miss, it loads missing data from database, populates the cache and returns it to the application.
![[Pasted image 20230228100757.png]]
- Loads data lazily, only when it is first read
- The cache is responsible for loading data from database
- The data model of the cache cannot be different than that in the database

- Work best for **read-heavy** workloads
- The data can become inconsistent / stale, depending on the write-strategy

# Write-Through Cache
In this write strategy, data is first written to the cache and then to the database. The cache sits in-line with the database and writes always go through the cache to the main database. This helps cache maintain consistency with the database.
![[Pasted image 20230228102416.png]]
1. The application writes data directly to the cache
2. The cache updates the main database. --> the cache remains consistent is immune to cache failure
Paired with read-through caches will have all the benefits of read-through caches and also have data consistency, freeing us from cache invalidation

# Write-Around
Data is written directly to the main database and only the data that is read makes it way into the cache.
Write-around can be combined with read-through to provide good performance in situations where data is written once and read less frequently or never. real-time logs, chatroom messages

# Write-Back / Write-Behind
Writes data to the cache and acknowledges to the application immeldiately. Then later, the cashe writes the data back to the database.
In write-through the data written in cache is **synchronously** updated in the main database. In write-back the data written in cache is **asynchronously** updated in the main database. From the application perspective, writes to Write-Back are faster because only the cache needed to be updated before returning to the application.
![[Pasted image 20230228105412.png]]
Pros:
Good for **write-heavy** workloads when combined with read-through cache, it works good for mixed workloads, where the recently updated and accessed data is always in cache.