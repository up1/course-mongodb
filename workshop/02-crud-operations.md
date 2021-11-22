## CRUD operations with MongoDB


### Create data
```
$db.products.insertOne({name:"paper",price:10, description:"A4 size papers"})

$db.products.insertMany([
    {
    "name": "MongoDB book",
    "price": 100
  },
  {
    "name": "Design patterns",
    "price": 200
  }
])
```

### [Import data from file (JSON, CSV)](https://docs.mongodb.com/compass/current/import-export/)
* Export
* Import

```
$mongoimport -d demo01 --collection=products --drop products.json

$mongoexport -d demo01 --collection=products --out=products.json
```

### Read data => Find all
```
// Returms only top 20
$db.products.find()
$db.products.find().pretty()

// Get all records
$db.products.find().toArray()
```

### Find data
* [Query and Projection Operators](https://docs.mongodb.com/manual/reference/operator/query/)

Find by id
```
$db.products.findOne({_id:ObjectId("619b6592ce950edda06355d6")})
$db.products.find({_id:ObjectId("619b6592ce950edda06355d6")})
```

Find by name
```
$db.products.find({name:"target"})
```

Working with filter + [comparison operators](https://docs.mongodb.com/manual/reference/operator/query-comparison/#std-label-query-selectors-comparison)
```
$db.products.find( { "price": { $gt: 50 } } )

$db.products.find( { price: { $ne: 100, $exists: true } } )

$db.products.find( { "price": { $in: [ 100, 200 ] } } )
```

### Update data

Update a single document (partial update)
```
$db.products.updateOne({_id:ObjectId("619b6592ce950edda06355d6")}, {
    $set: { name: "new name"}
})
```

Update all document in collection (partial update)
```
$db.products.updateMany({}, {
    $set: { name: "new name"}
})
```

Replace a whole document
```
$db.products.replaceOne({_id:ObjectId("619b6592ce950edda06355d6")}, { message: "Hello World"})

$db.products.update({_id:ObjectId("619b6592ce950edda06355d6")}, { message: "Hello World 2"})
```

### Delete data
Delete by id
```
$db.products.deleteOne({_id: "xyz"})
```

Delete all docuement in collection
```
$db.products.drop()
```
