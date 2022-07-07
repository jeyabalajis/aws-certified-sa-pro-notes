# ElastiCache

## Salient features
- Memcache is multi-threaded, non-persistent data
- Redis is multi-AZ and backup & restore feature is available


## Redis

- Lazy loading allows for stale data but doesn't fail with empty nodes. (Fills cache only for requested queries, but initially met with cache miss)
- Write-through ensures that data is always fresh, but can fail with empty nodes and can populate the cache with superfluous data. (Perform caching during write itself, but superfluos data can fill cache) 
- By adding a time to live (TTL) value to each write, you can have the advantages of each strategy.

> Lazy Loading, fill cache during read. Write-through, fill cache during write. Add TTL along with write-through to reduce over-population of data.