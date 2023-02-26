Benchmarking caching strategies
cache
cassandra
api service
security service

Scala futures

Security service
login
verify

security micros cache api micro database service

---
Actually we should implement a database server that does the under the hood caching and a seperate server that will reroute all the requests without needing to look at them. The reason to why this is important is because other services that would need to query data from database would need to also implement the caching stategy. If we only have one server that accepts requests we are better off without the database server.

