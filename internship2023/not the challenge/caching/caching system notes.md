[Brief Overview of Caching and Cache Invalidation | CodeAhoy](https://codeahoy.com/2022/04/03/cache-invalidation/)
[Caching Overview - DEV Community](https://dev.to/kapiluthra/caching-overview-40hl)
[Alexey Ragozin: Data Grid Pattern - Proactive caching](http://blog.ragozin.info/2011/10/grid-pattern-proactive-caching.html)
[Database caching: Overview, types, strategies and their benefits. (prisma.io)](https://www.prisma.io/dataguide/managing-databases/introduction-database-caching) for rapport probs



# Eviction policy
Once the cache is full, any requests to add items to the cache might cause existing items to be removed.
Cache eviction policies: Least-Recently-Used, Least-Frequently-Used or FIFO.

[Caching Strategies and How to Choose the Right One | CodeAhoy](https://codeahoy.com/2017/08/11/caching-strategies-and-how-to-choose-the-right-one/)

# Cache-Aside - Lazy caching
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

[Brief Overview of Caching and Cache Invalidation | CodeAhoy](https://codeahoy.com/2022/04/03/cache-invalidation/)

## When to Not Use a Cache

Caches are **not** always the right choice. They may not add any value and in some cases, may actually degrade performance. Here are some questions you need to answer to determine if you need a cache or not.

1.  The original source of data is slow (e.g. a query that does complex JOINs in a relational database.)
2.  The data doesn’t need to change for each request (e.g. caching real-time sensor data that your car needs when it’s in the self-driving mode or live medical data from patients… not good ideas.)
3.  The operation to fetch the data must not have any **side-effects** (e.g. a Relational DB Transaction that fetches data and updated KPI counters is not a good caching candidate due to side-effect of updating counters.)
4.  The data is frequently accessed and needed more than once.
5.  Good cache hit:miss ratio and total cost of cache misses. For example, suppose I put a cache for user requests as they come in and it takes **10 ms** to check if the data _exists_ in the cache or not, vs the original time of **60 ms**. If only **5%** of requests are cached, I’m **adding** an additional 10ms to 95% of the requests that result in a cache-miss. Doing rough calculations, we can see that cache is actually hurting performance:

-   Before cache: `1,000,000 requests * 60 milliseconds per request = 60,000,000 milliseconds total`
-   After cache: `(0.95 * 1,000,000 * 10) + (0.05 * 1,000,000 * (60 + 10) ) = 67,000,000 milliseconds total` Each cache miss results in 60+10 millisecond That’s poorer than using no cache, assuming all requests are equal in value/distribution.
