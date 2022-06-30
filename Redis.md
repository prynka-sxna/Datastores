**Redis 6** 

Documentation: https://redis.io/docs/ 
Commands: https://redis.io/commands/ 

- In-memory, key(String)-value(Data Structure) data-store.
- Expiration condition can be set for keys.
- Transaction support using MULTI/EXEC. MULTI marks the start of transaction commands, Redis start executing the transaction after EXEC is received. 
  Clients won't see a partial state and partial states are not committed to the disk as well.
  WATCH (Optimistic Locking) can be used to ensure that a transaction is executed on EXEC only if the value of a key doesn't change after MULTI is called.
- Disk persistence: Periodic snapshotting persistence (rdb) or aof based persistence that appends every incoming write. Automatic/Manual trigger.
  - Snapshotting can either be done by a background process or via a blocking operation.
  - AOF can trigger a blocking write to disk either after writing some commands to a buffer or after each write.
- Replication: Single master database sends writes out to multiple slaves, with the slaves performing the read queries. 
  Non-blocking for the master.
  Slaves can have slaves as well.
- Pub/Sub: Push based messaging.
- Horizontal Sharding.
- Lua scripts can be uploaded and then executed on the server. Scripts are executed atomically.
  TD: Read documentation for Lua.
- High availability via sentinels for automatic fail-over.
- Data is automatically sharded across a redis cluster via hash slots.
- Client-side caching or Tracking: 
  - Default Mode: Server remembers what keys a given client accessed, and sends invalidation messages when the same keys are modified.
  - Broadcast Mode: Clients subscribe to key prefixes and receive a notification message every time a key matching a subscribed prefix changes.

**Data Structures** 
Manual: https://redis.io/docs/manual/data-types/data-types-tutorial/
1. String: Binary safe string. Implemented using C dynamic strings to avoid cost of memory allocation.
2. List: LL of string elements sorted according to the order of insertion. Supports blocking operations with timeout.
3. Set: Collections of unique, unsorted string elements. O(1) insertion/deletion/membership check. Hash table implementation.
4. Sorted Set: Sets where every string element is sorted based on the associated score(float). Skip list implementation.
5. Hash Map: [field->value] pairs. Hash Table implementation.
Note: 
- Lists/Sorted Sets/Maps that have a small size are encoded in ziplist format (length of previous entry, length of current entry, string).
- Sets that have a small size are encoded in intset format (sorted array of integers). 
6. Bit Map: Allows strings to be handled like array of bits. Can be extended to support Bloom Filters.
7. HyperLogLog: 
- Probabilistic data structure encoded as string that is used to estimate the cardinality of a set. 
- Uses constant memory to estimate with an error < 1%. 
- TD: Algo
8. Stream: 
- Entries are structured as field-value pairs, with entryId <ms>-<seqNo> key for accessing latest entries ordered by time in a blocking or non-blocking manner.
- Radix tree(Compact prefix tree) of delta compressed nodes. 
- Append-only collections of map-like entries that provide an abstract log data type. 
- Consumer groups allow a stream to be processed in subsets atmost once between multiple consumers.
9. Geospatial Index: Field->Lat-long pairs are stored in a sorted set.
  
**Features Supported by Redis Stack**
TD: https://redis.io/docs/stack
- Queryable JSON documents
- Full-text search
- Time series data (ingestion & querying)
- Graph data models with the Cypher query language
- Probabilistic data structures [RedisBloom]: Bloom filter, a cuckoo filter, a count-min sketch, and a top-k.
