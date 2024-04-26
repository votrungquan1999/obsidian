# References
- [youtube Byte Go Go](https://www.youtube.com/watch?v=zvWKqUiovAM)

# TL;DR
- Should not prematurely optimize code unless you have done tests to make sure the API is the choke point
- Methods: caching, connection pool, avoid N+1 problem, pagination, fast and light-weight json serializer, compression, asynchonous logging.
# Main Point
## Caching
- have a cache layer to store the result of the query to get in later call of the same query
- remember to revalidate cache. there multiple ways: stale while revalidation, periodically update cache, …
## Connection pool
- having connection pool that the server can use right away would reduce the time it takes to establish a new connection on each request
- server less may require a third party service to manage connection pool since it can scale up fast and overwhelm the database
## Avoid N + 1 problem
- N + 1 problems are cases that we need to query the dependencies of an entity. e.g. When we need to query the comments detail of a post, it is possible that we use 1 query for the post, and N query for the comments detail, hence N + 1
- This can cause massive traffic back and forth to the database
- Should batch the queries to reduce the traffic
## Pagination
- Divide the data into small chunks and use page size and limit to control
## Fast JSON serialization
- Use a fast json serialization lib to minimize the time it takes to serialize and deserialize the response
## Compression
- Compress large data before transferring data through the network
## Asynchronous Logging
- Use asynchronous logging to eliminate the time that it takes to write logs. Which can help a lot in large systems where a lot of logs are written.
- However, using asynchronous logging can make you loose some logs if the system crashes before the logs can be written.