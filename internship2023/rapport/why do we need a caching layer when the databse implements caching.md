[java - Why use your application-level cache if database already provides caching? - Stack Overflow](https://stackoverflow.com/questions/2963819/why-use-your-application-level-cache-if-database-already-provides-caching)
```
Because to get the data from the database's cache, you still have to:

1.  Generate the SQL from the ORM's "native" query format
2.  Do a network round-trip to the database server
3.  Parse the SQL
4.  Fetch the data from the cache
5.  Serialise the data to the database's over-the-wire format
6.  Deserialize the data into the database client library's format
7.  Convert the database client librarie's format into language-level objects (i.e. a collection of whatevers)

By caching at the application level, you don't have to do any of that. Typically, it's a simple lookup of an in-memory hashtable. Sometimes (if caching with memcache) there's still a network round-trip, but all of the other stuff no longer happens.```
```

```
Here are a couple of reasons why you may want this:

-   An application caches just what it needs so you should get a better cache hit ratio
-   Accessing a local cache will probably be a couple of orders of magnitude faster than accessing the database due to network latency - even with a fast network
```
