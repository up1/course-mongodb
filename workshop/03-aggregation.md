## [Aggregation pipeline](https://docs.mongodb.com/manual/aggregation/)

### [Import data from file (JSON, CSV)](https://docs.mongodb.com/compass/current/import-export/)
* Export
* Import

```
mongoimport -d demo01 --collection=universities --drop universities.json --jsonArray
```

### Query data in Bangkok, Thailand
```
db.universities.aggregate([
{ $match : { country : 'Thailand', city : 'Bangkok' } }
])
```

Return data only country, city and name
```
db.universities.aggregate([
  { $project : { _id : 0, country : 1, city : 1, name : 1 } } ])
```

### Unwind with students
```
db.universities.aggregate([
  { $unwind : '$students' }
])
```

### Write aggregation pipeline

Return numbers students by year
```
db.universities.aggregate([
  { $match : { name : 'CU' } },
  { $unwind : '$students' },
  { $project : { _id : 0, 'students.year' : 1, 'students.number' : 1 } },
  { $sort : { 'students.number' : -1 } }
])
```

Group by name and sum
```
db.universities.aggregate([
  { $unwind : '$students' },
  { $group : { _id : '$name', totalalumni : { $sum : '$students.number' } } },
  { $sort : { totalalumni : -1 } }
])
```
