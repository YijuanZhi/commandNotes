# MongoBD Commands Notes -- Yijuan Zhi

## Under MongoDB
- (mongod  ~~~~ run mongodb in the background in order to use mongo)
- mongo  ~~~~ start the mongo
- show dbs  ~~~~ show all the databases
- use [somedb]  ~~~~ now use the database: somedb, if it doesn't exist, create it
***

## Under a Database
- show collections  ~~~~ show all the collections under the current database
- db.dropDatabase()  ~~~~ delete the current database
- db.createCollection("someCollection") ~~~~ create a new collection under the current database
***

## Manipulation of Collection
- db.someCollection.**insert**({ title: "mac", price: 1200, year: 2018, type: "pro", date: Data()})
- db.someCollection.**insertMany(an array of json objs)**

### Find()
- db.someCollection.find().**pretty()**  ~~~~ show all the items under the current collection, in a pretty way
- db.someCollection.find(**{ title: "mac"}**)  ~~~~ show the items that has the passed property under the current collection
- db.someCollection.find().**count()**  ~~~~ count the total number of items under someCollection
- db.someCollection.find().**limit(3)**  ~~~~ only show 3 items of someCollection
- db.someCollection.**sort**({ year: 1})  ~~~~ show the sorted items in ascending order(1) or descending order(-1)

### update()
- db.someCollection.update(**{title: "mac"}, {year: 2019}**)  ~~~~ update the item which has the first property, add or replace with the second property
- db.someCollection.update({title: "mac"}, {year: 2019}, **{upsert: true}**)  ~~~~ the third property allow create new one if the item is not found
- db.someCollection.update({title: "mac"}, { **$set**: { year:2020, price: 1300} })  ~~~~ $set operator will replace the value of a field with specific value
- db.someCollection.update({title: "mac"}, { **$inc**: { year:2} })  ~~~~  increase the year field by 2
- db.someCollection.update({title: "mac"}, { **$rename**:{ title: "name"}})  ~~~~ rename the field with a new field name

### Others
- db.someCollection.remove({name: "mac"})  ~~~~ remove row(whole object) with specific value of field
- 