## Hello MongoDB with mongo shell

### Start with Mongo shell
```
$mongo

MongoDB shell version v5.0.4
connecting to: mongodb://127.0.0.1:27017/?compressors=disabled&gssapiServiceName=mongodb
Implicit session: session { "id" : UUID("2ff72cc6-12cc-4dab-a320-e95fc939ee1f") }
MongoDB server version: 5.0.4
```

### List all databases
```
$show dbs

admin   0.000GB
config  0.000GB
local   0.000GB
```

### Create database
```
$db
$use demo01
switched to db demo01
```

### Create collection
```
$db.createCollection("products")
$show collections
```

### Create data
```
$db.products.insertOne({name:"paper",price:10, description:"A4 size papers"})
```

### Read data => Find all
```
$db.products.count()
$db.products.find()
$db.products.find().pretty()
```

### Drop collection
```
$db.products.drop()
$db.dropDatabase()
```

### More information
```
$help
$db.help()
```
