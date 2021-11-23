## Step to create replication
* Setup with 3 nodes

### 1. Create data directories for node 1, 2, 3
```
$mkdir -p data/rs{1,2,3}
```

### 2. Start MongoDB server with mongod
```
$mongod --replSet demo01 --dbpath data/rs1 --port 27017 --oplogSize 200
$mongod --replSet demo01 --dbpath data/rs2 --port 27018 --oplogSize 200
$mongod --replSet demo01 --dbpath data/rs3 --port 27019 --oplogSize 200
```



### 2. Start MongoDB server with mongod
```
$rsconf = {
    _id: "demo01",
    members: [
      {_id: 0, host: "localhost:27017"},
      {_id: 1, host: "localhost:27018"},
      {_id: 2, host: "localhost:27019"} 
    ]
  }

$rs.initiate(rsconf)

or

$rs.reconfig(rsconf)
```

Status of replication
```

$db.isMaster()
$rs.status()

"members" : [
		{
			"_id" : 0,
			"name" : "localhost:27017",
			"health" : 1,
			"state" : 1,
			"stateStr" : "PRIMARY"
		},
		{
			"_id" : 1,
			"name" : "localhost:27018",
			"health" : 1,
			"state" : 2,
			"stateStr" : "SECONDARY"
		},
		{
			"_id" : 2,
			"name" : "localhost:27019",
			"health" : 1,
			"state" : 2,
			"stateStr" : "SECONDARY",
		}
	]
```

### 3. Create data in Primary server
```
$mongo --port 27017
demo01:PRIMARY>
```

Create new data in database = test
```
$use test

$for (i=0; i<1000; i++) {db.demo.insert({count: i})}
WriteResult({ "nInserted" : 1 })

$db.demo.count()
1000
```

Check data in Secondary server
* port 27018
* port 27019

```
$mongo --port 27018
demo01:SECONDARY>

$db.setSecondaryOk()
$show collections
$db.demo.count()
1000
```

### 4. Add and remove node from replica set (Run in Primary node only)
```
$rs.status()
$rs.config()

$rs.add("localhost:27020")

$rs.remove("localhost:27018")
```

Chnage status from primary to secondary
```
$rs.stepDown()
```