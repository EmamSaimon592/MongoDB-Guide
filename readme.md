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
  <img src="./assets/mdb01.png" alt="Cover Image" width="100%" />
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