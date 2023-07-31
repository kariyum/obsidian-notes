# Cache invalidation
- **Purge**: The purge method removes cached content for a specific object, URL, or a set of URLs. It’s typically used when there is an update or change to the content and the cached version is no longer valid. When a purge request is received, the cached content is immediately removed, and the next request for the content will be served directly from the origin server.

-  **Refresh**: Fetches requested content from the origin server, even if cached content is available. When a refresh request is received, the cached content is updated with the latest version from the origin server, ensuring that the content is up-to-date. Unlike a purge, a refresh request doesn’t remove the existing cached content; instead, it updates it with the latest version.

-  **Ban**: The ban method invalidates cached content based on specific criteria, such as a URL pattern or header. When a ban request is received, any cached content that matches the specified criteria is immediately removed, and subsequent requests for the content will be served directly from the origin server.

-  **Time-to-live (TTL) expiration**: This method involves setting a time-to-live value for cached content, after which the content is considered stale and must be refreshed. When a request is received for the content, the cache checks the time-to-live value and serves the cached content only if the value hasn’t expired. If the value has expired, the cache fetches the latest version of the content from the origin server and caches it.

-  **Stale-while-revalidate**: This method is used in web browsers and CDNs to serve stale content from the cache while the content is being updated in the background. When a request is received for a piece of content, the cached version is immediately served to the user, and an asynchronous request is made to the origin server to fetch the latest version of the content. Once the latest version is available, the cached version is updated. This method ensures that the user is always served content quickly, even if the cached version is slightly outdated.

# Cache Performance Metrics
When implementing caching, it’s important to measure the performance of the cache to ensure that it is effective in reducing latency and improving system performance. Here are some of the most common cache performance metrics:

- **Hit rate:** The hit rate is the percentage of requests that are served by the cache without accessing the original source. A high hit rate indicates that the cache is effective in reducing the number of requests to the original source, while a low hit rate indicates that the cache may not be providing significant performance benefits.

- **Miss rate:** The miss rate is the percentage of requests that are not served by the cache and need to be fetched from the original source. A high miss rate indicates that the cache may not be caching the right data or that the cache size may not be large enough to store all frequently accessed data.

- **Cache size:** The cache size is the amount of memory or storage allocated for the cache. The cache size can impact the hit rate and miss rate of the cache. A larger cache size can result in a higher hit rate, but it may also increase the cost and complexity of the caching solution.

- **Cache latency:** The cache latency is the time it takes to access data from the cache. A lower cache latency indicates that the cache is faster and more effective in reducing latency and improving system performance. The cache latency can be impacted by the caching technology used, the cache size, and the cache replacement and invalidation policies.

- ==**Hit rate**==
- **Number of cassandra requests**
- ==**Number of requests incoming total**==
- **How many cassandra queries per second ? and in total ?**
- **Average response time sampeled from last 100 requests**
- **Cache response time sampeled from last max 100 requests**
- **Cassandra response time sampeled from last max 100 requests**
- Average read requests per second and in total
- Average write requests per second and in total
- Read reponse times; average of (cache response times + cassandra response times) ? Nooo
- Write response times; average of 
- ==lantency by percentile distribution==


Actually how about making this whole thing as a library, we can market this as a generic solution that other application can integrate easily to test their cache performance ? Let's leave this as the next BIG step ?