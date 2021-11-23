## Step to create replication
* Setup with 3 nodes
* Using [Internal/membership authentication](https://docs.mongodb.com/manual/core/security-internal-authentication/)
  * Keyfiles
  * x.509

### 1. Generate Key File
```
$mkdir -p mongodb/key/
$openssl rand -base64 741 > mongodb/key/keyfile
$chmod 400 mongodb/key/keyfile
```


### 2. Create data directories for node 1, 2, 3
```
$mkdir -p data/rs{1,2,3}
```

### 3. Start Nodes with configuration file
```
$mongod -f node1.conf
$mongod -f node2.conf
$mongod -f node3.conf
```

### 4. Access to Node 1 without authentication
* Create replica set
* Create user
```
$mongo --port 27001
$rs.initiate()

$use admin
$db.createUser({
  user: "demo",
  pwd: "demo",
  roles: [
    {role: "root", db: "admin"}
  ]
})

$exit
```

Try to access Node 1 with authentication
```
$mongo --host "demo02/localhost:27001" -u "demo" -p "demo" --authenticationDatabase "admin"

$rs.status()
$rs.isMaster()
```

### 5. Add 2 nodes to replica set
```
$rs.add("localhost:27002")
$rs.add("localhost:27003")
```

Status of replication
```
$rs.status()

"members" : [
		{
			"_id" : 0,
			"name" : "localhost:27001",
			"health" : 1,
			"state" : 1,
			"stateStr" : "PRIMARY"
		},
		{
			"_id" : 1,
			"name" : "localhost:27002",
			"health" : 1,
			"state" : 2,
			"stateStr" : "SECONDARY"
		},
		{
			"_id" : 2,
			"name" : "localhost:27003",
			"health" : 1,
			"state" : 2,
			"stateStr" : "SECONDARY",
		}
	]
```

### 6. Delete nodes
```
$ps -ef | grep mongod
$kill <pid>
```