## [Indexing](https://docs.mongodb.com/manual/indexes/)
* Query criteria
* Sort order
  * 1 = ascending
  * -1 = descending

Default index is `_id` field
## Types of indexes
* Single field indexes
* Compound indexes
* Multikey indexes => For array
* Geospatial indexes => Geo location
* Text indexes => For search
* Wildcard indexes
* Hashed indexes

### Explain query
```
db.products.find({"name": "demo"}).explain("executionStats")
```

Result
```
{
	"explainVersion" : "1",
	"queryPlanner" : {
		"namespace" : "demo01.products",
		"indexFilterSet" : false,
		"parsedQuery" : {
			"name" : {
				"$eq" : "demo"
			}
		},
		"maxIndexedOrSolutionsReached" : false,
		"maxIndexedAndSolutionsReached" : false,
		"maxScansToExplodeReached" : false,
		"winningPlan" : {
			"stage" : "COLLSCAN",
			"filter" : {
				"name" : {
					"$eq" : "demo"
				}
			},
			"direction" : "forward"
		},
		"rejectedPlans" : [ ]
	},
	"executionStats" : {
		"executionSuccess" : true,
		"nReturned" : 0,
		"executionTimeMillis" : 0,
		"totalKeysExamined" : 0,
		"totalDocsExamined" : 3,
		"executionStages" : {
			"stage" : "COLLSCAN",
			"filter" : {
				"name" : {
					"$eq" : "demo"
				}
			},
			"nReturned" : 0,
			"executionTimeMillisEstimate" : 0,
			"works" : 5,
			"advanced" : 0,
			"needTime" : 4,
			"needYield" : 0,
			"saveState" : 0,
			"restoreState" : 0,
			"isEOF" : 1,
			"direction" : "forward",
			"docsExamined" : 3
		}
	},
	"command" : {
		"find" : "products",
		"filter" : {
			"name" : "demo"
		},
		"$db" : "demo01"
	},
	"serverInfo" : {
		"host" : "66132f22f038",
		"port" : 27017,
		"version" : "5.0.4",
		"gitVersion" : "62a84ede3cc9a334e8bc82160714df71e7d3a29e"
	},
	"serverParameters" : {
		"internalQueryFacetBufferSizeBytes" : 104857600,
		"internalQueryFacetMaxOutputDocSizeBytes" : 104857600,
		"internalLookupStageIntermediateDocumentMaxSizeBytes" : 104857600,
		"internalDocumentSourceGroupMaxMemoryBytes" : 104857600,
		"internalQueryMaxBlockingSortMemoryUsageBytes" : 104857600,
		"internalQueryProhibitBlockingMergeOnMongoS" : 0,
		"internalQueryMaxAddToSetBytes" : 104857600,
		"internalDocumentSourceSetWindowFieldsMaxMemoryBytes" : 104857600
	},
	"ok" : 1
}
```

### Single field indexes

Create index
```
db.products.createIndex({ <field>: <direction> })
```
Directions values
* 1 = ascending
* -1 = descending

Example
```
db.products.createIndex({ name: 1 })

db.products.createIndex({ name: 1 }, { name: "first index" })
'''

Explain again
'''
db.products.find({"name": "demo"}).explain("executionStats")
```

Result
```
...
"winningPlan" : {
			"stage" : "FETCH",
			"inputStage" : {
				"stage" : "IXSCAN",
				"keyPattern" : {
					"name" : 1
				},
				"indexName" : "name_1",
...
```

List of all indexes in collection
```
db.products.getIndexes()

[
	{
		"v" : 2,
		"key" : {
			"_id" : 1
		},
		"name" : "_id_"
	},
	{
		"v" : 2,
		"key" : {
			"name" : 1
		},
		"name" : "first index"
	}
]
```

Drop index
```
db.products.dropIndex("name_1")

db.products.dropIndex({"name": 1})
```

Remove all indexes
```
db.products.dropIndexes()
```

### Compound indexes

```
db.products.createIndex({ name: 1, price: -1j })
```

### wildcard indexes

Example
```
db.userData.createIndex( { "data.$**" : 1 } )
```

Support queries
```
db.userData.find({ "data.likes" : "dogs" })
db.userData.find({ "data.dislikes" : "pickles" })
db.userData.find({ "data.age" : { $gt : 30 } })
db.userData.find({ "data" : "inactive" })
```

### [Indexes Strategies](https://docs.mongodb.com/manual/applications/indexes/)


### Working with sorting data
* Default :: sorting data in memory
* Improve sorting data with indexes

Import data
```
$mongoimport --db perf --collection persons --file persons.json --jsonArray
```

Check amount of data = 5,000
```
$mongo
$use perf
$db.persons.count()
```

Query sample data
```
$db.persons.findOne()

{
	"_id" : ObjectId("619bd1d47793a5e2dbf00937"),
	"gender" : "male",
	"name" : {
		"title" : "mr",
		"first" : "elijah",
		"last" : "lewis"
	},
	"location" : {
		"street" : "2623 paddock way",
		"city" : "virginia beach",
		"state" : "wyoming",
		"postcode" : 54880,
		"coordinates" : {
			"latitude" : "-42.6128",
			"longitude" : "-18.5996"
		},
		"timezone" : {
			"offset" : "+3:30",
			"description" : "Tehran"
		}
	},
	"email" : "elijah.lewis@example.com",
	"login" : {
		"uuid" : "2292b7c7-a9bf-4341-abbd-c5841444ab6e",
		"username" : "angrygorilla267",
		"password" : "toonarmy",
		"salt" : "DUtMcvWR",
		"md5" : "258eaa742373ee70976d53d1b84d4764",
		"sha1" : "62f168f38fa3f6efbd815b58518775d3c6cf0080",
		"sha256" : "a4ab496047b9de7df39adbdabfecc813b8da428087029c94c2b748cef092f85e"
	},
	"dob" : {
		"date" : "1986-03-29T06:40:18Z",
		"age" : 32
	},
	"registered" : {
		"date" : "2007-12-24T10:32:11Z",
		"age" : 10
	},
	"phone" : "(187)-486-3727",
	"cell" : "(986)-974-0857",
	"id" : {
		"name" : "SSN",
		"value" : "127-66-9786"
	},
	"picture" : {
		"large" : "https://randomuser.me/api/portraits/men/57.jpg",
		"medium" : "https://randomuser.me/api/portraits/med/men/57.jpg",
		"thumbnail" : "https://randomuser.me/api/portraits/thumb/men/57.jpg"
	},
	"nat" : "US"
}
```

Query with sorting
```
db.persons.find({}, {_id: 0, name: 1, dob: 1}).sort({ "dob.age": 1 })
```

Explain query
```
db.persons.find({}, {_id: 0, name: 1, dob: 1}).sort({ "dob.age": 1 }).explain('executionStats')


var exp = db.persons.explain('executionStats')
exp.find({}, {_id: 0, name: 1, dob: 1}).sort({ "dob.age": 1 })
```

Create index for sorting
```
db.persons.dropIndexes()
db.persons.createIndex({ "dob.age": 1 })
db.persons.getIndexes()
```

Explain query again !!
