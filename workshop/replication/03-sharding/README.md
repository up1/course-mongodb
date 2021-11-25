## Sharded cluster
* [Sharding](https://docs.mongodb.com/manual/sharding/)
* [Deploy sharded cluster](https://docs.mongodb.com/manual/tutorial/deploy-shard-cluster/)

## Sharded cluster's components
* Shard => deploy as a replica set
* Mongos => query router (interface between client and sharded cluster)
* Config servers

## Step to deployment

### 1. Create the Config Server Replica Set
* For production >= 3 members

Create data directory for config server
```
$mkdir -p data/config{1,2,3}
```

Start Nodes with configuration file
```
$mongod -f config-node1.conf
$mongod -f config-node2.conf
$mongod -f config-node3.conf
```

Initial the replicat set
```
$mongo --host localhost --port 27001

$rs.initiate(
  {
    _id: "config-demo03",
    configsvr: true,
    members: [
      { _id : 0, host : "localhost:27001" },
      { _id : 1, host : "localhost:27002" },
      { _id : 2, host : "localhost:27003" }
    ]
  }
)

$rs.status()
$rs.isMaster()
```

### 2. Create the Shard Replica Sets
* For production >= 3 members

Create data directory for config server
```
$mkdir -p data/rs{1,2,3}
```

Start Nodes with configuration file
```
$mongod -f shard-node1.conf
$mongod -f shard-node2.conf
$mongod -f shard-node3.conf
```

Initial the replicat set
```
$mongo --host localhost --port 27017

$rs.initiate(
  {
    _id: "demo03",
    members: [
      { _id : 0, host : "localhost:27017" },
      { _id : 1, host : "localhost:27018" },
      { _id : 2, host : "localhost:27019" }
    ]
  }
)

$rs.status()
$rs.isMaster()
```

### 3. Connect a mongos to the Sharded Cluster

Start mongos
```
$mongos -f mongos.conf
```

Connect to mongos
```
$mongo --host localhost --port 26000
$sh.status()
```

### 4. Add Shards to the Cluster
```
$sh.addShard( "demo03/localhost:27017")
```

### 5. Enable Sharding for a Database
```
$sh.enableSharding("<database>")
```

### 6. Shard a Collection
* [Hashed sharding](https://docs.mongodb.com/manual/core/hashed-sharding/#std-label-sharding-hashed)
* [Range-based sharding](https://docs.mongodb.com/manual/core/ranged-sharding/#std-label-sharding-ranged)
```
// Hashed
$sh.shardCollection("<database>.<collection>", { <shard key field> : "hashed" } )

// Range-based
$sh.shardCollection("<database>.<collection>", { <shard key field> : 1, ... } )
```

Example :: Access to mongos server (Router)
```
$sh.enableSharding("demo")
$sh.shardCollection("demo.mycollection", {_id:1})

use demo
db.mycollection.insert({name: "Me"})
db.mycollection.find()

sh.status()
```


Example of Hashed shard
```
$sh.addShard( "demo04/localhost:27018")

$sh.enableSharding("demo3")

$sh.shardCollection(
  "demo3.xyz",
  { "age" : "hashed" }
)

$use demo3
$for (i=0; i<1000; i++) {db.xyz.insert({age: i})}
```
