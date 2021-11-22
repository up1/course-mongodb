## [Indexing](https://docs.mongodb.com/manual/indexes/)
* Signle field indexes
* Index sorting
* Compound indexes

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

### Signle field indexes

Create index
```
db.products.createIndex({ <field>: <direction> })


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

### Index sorting

```
```

### Compound indexes

```
```
