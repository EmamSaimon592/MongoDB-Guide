# <p align="center">mongodb-fundamentals</p>

<p align="center">
  <img src="./assets/mdb.png" alt="Cover Image" width="100%" />
</p>

# QUICK SETUP
1. Go to: https://www.mongodb.com/try/download/community  <br>
2. Choose Windows/macOS/Linux → Download  <br>
3. Install (default path is fine) <br>
4. Compass is included in the installer (tick it) <br>
5. Open MongoDB Compass → It auto-detects local server <br>
<b><i><u>Your local MongoDB is running on mongodb://localhost:27027 (default port) </u></i></b>
 
 
# Check Version & Start Server 
```
mongod --version
// or
mongo --version
```

# Check MongoDB Service (Server running or not) 
cmd = win + R 
```
services.msc 
```
Find MongoDB Server 
If it exists → MongoDB is installed. 

# Vs code 

<p align="center">
  <img src="./assets/mdb01.png" alt="Cover Image" width="50%" />
</p>

# MongoDB Shell (mongosh) Pro Commands – A to Z Cheat Sheet

A comprehensive, alphabetically organized reference of the most powerful and frequently used **mongosh** commands for developers, DBAs, and performance tuners.

Perfect for quick copy-paste during troubleshooting, development, and production incidents.

---

## A-to-Z Command Reference

| Letter | Command / Syntax                                          | Description / Use Case                                                                 |
|--------|-----------------------------------------------------------|----------------------------------------------------------------------------------------|
| A      | `db.collection.aggregate([pipeline])`                     | Run aggregation pipelines (`$match`, `$group`, `$lookup`, `$unwind`, etc.)            |
|        | `use <database>`                                          | Switch to (or create) a database                                                       |
| B      | `db.collection.bulkWrite([operations])`                   | Multiple insert/update/delete operations in a single command                          |
| C      | `db.createCollection("name", options)`                    | Create collection with options (capped, validation, collation, etc.)                  |
|        | `db.collection.countDocuments(query)`                     | Accurate count using aggregation (preferred over deprecated `.count()`)              |
|        | `db.currentOp()`                                          | View in-progress operations (find long-running queries)                                |
|        | `db.collection.createIndex({field: 1}, options)`          | Create indexes (compound, TTL, text, geospatial, hashed, etc.)                         |
| D      | `db.dropDatabase()`                                       | Delete the current database                                                            |
|        | `db.collection.deleteOne()` / `deleteMany()`              | Remove one or many documents                                                           |
|        | `db.collection.distinct("field")`                         | Get array of unique values                                                             |
| E      | `.explain("executionStats")`                              | Attach to `find()`, `aggregate()`, etc. to see query plan & performance                 |
| F      | `db.collection.find(query).pretty()`                      | Query documents with nice formatting                                                   |
|        | `db.collection.findOne(query)`                            | Return a single document                                                               |
| G      | `db.getCollectionNames()`                                 | List all collections in the current database                                           |
|        | `db.getSiblingDB("name")`                                 | Access another database without `use`                                                  |
| H      | `db.hostInfo()`                                           | System/OS information                                                                  |
|        | `db.hello()`                                              | Connection handshake info (replaces deprecated `isMaster()`)                           |
| I      | `it`                                                      | Fetch next batch of cursor results                                                     |
|        | `db.collection.insertOne()` / `insertMany()`              | Insert single or multiple documents                                                    |
| K      | `db.killOp(opid)`                                         | Terminate a running operation (use opid from `currentOp()`)                           |
| L      | `db.load("script.js")`                                    | Execute an external JavaScript file in mongosh                                         |
| M      | `db.collection.mapReduce(map, reduce, options)`           | Legacy Map-Reduce (prefer aggregation framework)                                       |
| O      | `show dbs` \| `show databases`                            | List all databases                                                                     |
|        | `show collections`                                       | List collections in current database                                                   |
|        | `show users` \| `show roles`                              | List users and roles                                                                   |
| P      | `db.printCollectionStats()`                               | Storage stats for all collections                                                      |
|        | `db.printReplicationInfo()`                               | Oplog window size (primary)                                                            |
|        | `db.printSecondaryReplicationInfo()`                      | Replication lag (secondaries)                                                          |
| R      | `rs.status()`                                             | Full replica set health and status                                                     |
|        | `rs.stepDown()`                                           | Force primary to step down                                                             |
|        | `rs.reconfig(cfg)`                                        | Update replica set configuration (use with extreme care)                              |
|        | `db.runCommand({command})`                                | Execute any admin or database command directly                                         |
| S      | `sh.status()`                                             | Sharding cluster overview                                                              |
|        | `sh.enableSharding("db")`                                 | Enable sharding on a database                                                          |
|        | `sh.shardCollection("db.coll", key)`                      | Shard a collection                                                                     |
|        | `db.serverStatus()`                                       | Rich server metrics (memory, connections, caches, etc.)                                |
|        | `db.stats()` \| `db.collection.stats()`                   | Database/collection size and index stats                                               |
| T      | `db.collection.totalIndexSize()`                          | Total size of indexes on a collection                                                  |
| U      | `db.collection.updateOne()` / `updateMany()`              | Modern update operators                                                                |
| V      | `db.version()`                                            | MongoDB server version                                                                 |
|        | `db.collection.validate({full: true})`                    | Deep collection validation (indexes + data)                                            |
| W      | `session.withTransaction(() => { ... })`                  | Multi-document ACID transactions (MongoDB 4.0+)                                        |

---

## Pro One-Liners You’ll Use Every Day

```js
use mydb
db.mycol.find().limit(20).pretty()
db.mycol.find({status:"error"}).sort({ts:-1}).limit(50)
db.currentOp(true).inprog.forEach(op => { if (op.secs_running > 10) printjson(op) })
db.mycol.find({price: {$gt: 100}}).explain("executionStats")
db.orders.aggregate([{$group: {_id: "$customer", total: {$sum: "$amount"}}}])
rs.status()
sh.status()
db.serverStatus().metrics.query.slowQueries   // check slow query count
```

# Tips from Real-World MongoDB Experts  
*(The hard-won lessons you won’t find in most tutorials)*

Here’s the distilled wisdom from MongoDB DBAs, architects, and developers who’ve been on-call at scale. Keep this open — it will save you hours (or nights) of pain.

### Must-Know Best Practices

1. **Never trust `.count()`**  
   ```js
   // Deprecated & can be wildly wrong on sharded clusters
   db.collection.count()

   // Always use these instead
   db.collection.countDocuments({})                   // accurate, uses aggregation
   db.collection.estimatedDocumentCount({})           // fast (metadata only, no filters)
   db.collection.aggregate([{ $count: "total" }])
   ```
2. Always explain before you index
```js
db.collection.find({ status: "A" }).explain("executionStats")
db.collection.aggregate([...]).explain("executionStats")
```
Look for: totalDocsExamined ≈ totalKeysExamined ≈ 0 → your index is good.

3. currentOp() + killOp() = your incident super-power
```
// Find queries running > 30 seconds
db.currentOp(true).inprog.forEach(op => {
  if (op.secs_running > 30) printjson(op);
})

// Kill the worst offender
db.killOp(123456789)   // opid from above
```

4. Bulk operations: always prefer the new API + ordered: false
```
db.collection.bulkWrite([
  { insertOne: { document: doc1 } },
  { updateOne: { filter: { _id: 2 }, update: { $set: { x: 1 } } } },
  { deleteOne: { filter: { _id: 3 } } }
], { ordered: false })   // continues on error → much faster & resilient
```
5. Sharded cluster rule #1: NEVER connect directly to a mongod shard
Always connect to mongos for queries, writes, and administration. <br>
Direct shard connections bypass the balancer, chunk routing, and can corrupt data if used incorrectly.

6. Slow queries? Check the profiler first
```
db.setProfilingLevel(1, { slowms: 100 })   // log queries > 100ms
db.system.profile.find().sort({ ts: -1 }).limit(20).pretty()
```
7. Use proper write concern & read concern in production
```
db.collection.insertOne(doc, { writeConcern: { w: "majority", wtimeout: 5000 } })
db.collection.find().readConcern("majority")
```
8. Transactions: don’t forget the session
```
const session = db.getMongo().startSession();
session.withTransaction(() => {
  // your multi-collection ops here
}, { readConcern: { level: "snapshot" }, writeConcern: { w: "majority" } });
```
9. Index bloat is real — clean up duplicates
```
db.collection.getIndexes()                     // spot duplicates or unused ones
db.collection.dropIndex("oldIndexName")
```
10. Oplog too small? You’re living on borrowed time
```
// On primary
db.printReplicationInfo()
// If oplog window < 24h on a busy system → increase it NOW
```
### Bonus One-Liners Every Expert Has Memorized]
```
rs.status().members.forEach(m => print(m.name + " → " + m.stateStr))
sh.status().shards.forEach(s => print(s._id + ": " + s.host))
db.serverStatus().tcmalloc.tcmalloc.aggressiveMemoryDecompress   // memory fragmentation
db.stats().dataSize / db.stats().storageSize                     // compression ratio
```

