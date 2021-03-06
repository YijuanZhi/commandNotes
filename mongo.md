# MongoBD and Mongoose Commands Notes -- Yijuan Zhi

## MongoDB
### Setup and Usage
- (mongod  ~~~~ run mongodb in the background in order to use mongo)
- mongo  ~~~~ start the mongo
- show dbs  ~~~~ show all the databases
- use [somedb]  ~~~~ now use the database: somedb, if it doesn't exist, create it

### Under a Database
- show collections  ~~~~ show all the collections under the current database
- db.dropDatabase()  ~~~~ delete the current database
- db.createCollection("someCollection") ~~~~ create a new collection under the current database

### Manipulation of Collection (CRUD) -- IFUR
#### Insert()
- db.someCollection.**insert**({ title: "mac", price: 1200, year: 2018, type: "pro", date: Data()})
- db.someCollection.**insertMany(an array of json objs)**

#### Find()
- db.someCollection.find().**pretty()**  ~~~~ show all the items under the current collection, in a pretty way
- db.someCollection.find(**{ title: "mac"}**)  ~~~~ show the items that has the passed property under the current collection
- db.someCollection.find().**count()**  ~~~~ count the total number of items under someCollection
- db.someCollection.find().**limit(3)**  ~~~~ only show 3 items of someCollection
- db.someCollection.**sort**({ year: 1})  ~~~~ show the sorted items in ascending order(1) or descending order(-1)

#### Update()
##### update **entire** object except _id value
- db.someCollection.update(**{title: "mac"}, {year: 2019}**)  ~~~~ overwrite the object with specific value in a field with only the second param
- db.someCollection.update({title: "mac"}, {year: 2019}, **{upsert: true}**)  ~~~~ the third property allow create new one if the item is not found
##### update **specific** field
- db.someCollection.update({title: "mac"}, { **$set**: { year:2020, price: 1300} })  ~~~~ $set operator will replace the value of a field with specific value
- db.someCollection.update({title: "mac"}, { **$inc**: { year:2} })  ~~~~  increase the year field by 2
- db.someCollection.update({title: "mac"}, { **$rename**:{ title: "name"}})  ~~~~ rename the field with a new field name

#### Remove()
- db.someCollection.**remove**({name: "mac"})  ~~~~ remove row(whole object) with specific value of field
- db.someCollection.drop()  ~~~~ drop the collection: someCollection

***

## Mongoose
Mongoose is an Object Data Modeling (ODM) library for MongoDB and Node.js. It manages relationships between data, provides schema validation, and is used to translate between objects in code and the representation of those objects in MongoDB.

We can install mongoose through: 
- **npm install --save mongoose**

### Basics: save create and find
```javascript
// Basic Setup ======================================================================
// Connect to mongodb
// Create a database named "cat_app" inside the mongo database 
let mongoose = require("mongoose");
mongoose.connect("mongodb://localhost/cat_app", {useNewUrlParser: true});

// Create a Schema
// it contains the basic pattern for future use
let catSchema = new mongoose.Schema({
    name: String,
    age: Number,
    breed: String
});

// Create a model out of the Schema
// Create a collection "Cat" in the database "cat_app" in mongoDB
let Cat = mongoose.model("Cat", catSchema);

//Manipulation of database: IFUR ====================================================

// Insert -- new+save
// Create a row(object) out of model, but not saved to "Cat" collection yet
let zac = new Cat({
    name: "Zacboi",
    age: 24,
    breed: dick
});
// We save the zac Cat to the "Cat collection" and check if there are errors
zac.save((err, cat)=>{
    if(err){
        console.log("Something went wrong, here is the err: ");
        console.log(err);
    }else{
        // the returned cat is the object we just saved into the collection
        console.log("We just add a new cat: ");
        console.log(cat);
    }
});

// Insert -- create
Cat.create({
    name: "yijuan",
    age: 23,
    male: true
},(err, cat)=>{
    if(err){
        console.log("Something went wrong.");
        console.log(err);
    }else{
        console.log("We have the following cats:");
        console.log(cat);
});

// Find -- find
Cat.find({}, (err, cat)=>{
    if(err){
        console.log("Something went wrong.");
        console.log(err);
    }else{
        console.log("We have the following cats:");
        console.log(cat);
    }
});

//remove
Cat.remove({},(err, cat)=>{
    if(err){
        console.log(err);
    }else{
        console.log("You have removed all the cats:");
        console.log(cat);
    }
})
```

### Associations: Embed and reference

#### Embed: Embed another Schema inside a Schema:
- let userSchema = new mongoose.Schema({name: String,email: String, **posts: [postSchema]**});

```javascript
// POSTS
let postSchema = new mongoose.Schema({title: String,content: String});
let Post = mongoose.model("Post", postSchema);

// USER
let userSchema = new mongoose.Schema({name: String,email: String,
    // EMBEDED DATA TYPE!!
    // We embeded a array of postSchema type inside of userSchema
    posts: [postSchema]
});

let User = mongoose.model("User", userSchema); 

// Creating newuser and newpost
let newUser = new User({ email: "newboi@gmail.com", name: "Your Boi" });

// 3 Ways to add post into the user 
newUser.post.push({ title: "My First Post Ever!!!", body: "basdfnasdfdslaflsljeovnonon"});
// or directly push the post:
// newUser.post.push(newPost);
// or when creating the newUser:
// let newUser = new User({ email: "newboi@gmail.com", name: "Your Boi",posts: [newPost]});

newUser.save(function(err, user){
    if(err){
        console.log(err);
    }else{
        console,log(user);
    }
});
//Saving is necessary
```

#### Reference: reference the id of other objects
- use an object which refer to the ObjectId

```javascript
let userSchema = new mongoose.Schema({name: String,email: String,
    // REFERENCE!!
    // An array of objects
    posts: [
        {
            type: mongoose.Schema.Types.ObjectId,
            ref: "Post"
        }
    ]
});
let User = mongoose.model("User", userSchema);
let newUser2 = new User({
    name: "New Boi",
    email: "newboi@gmail.com"
});

// create and save the new post id into the posts array inside the user
Post.create({
    title: "New post by our New Boi",
    content: "dsajfkadsjlflsadjfldsajlkfdksl"
}, (err, post)=>{
    User.findOne({name: "New Boi"}, (err, user)=>{
        // Directly push the new post object, they will handle the rest
        user.posts.push(post);
        user.save((err, user)=>{
            console.log("successfully save an new post id to the user");
            console.log(user);
        });
    });
});


// In this situation, finding the user
// and find all the posts of the user
User.findOne({name: "New Boi"}).populate("posts").exec((err, user)=>{
    if(err){
        console.log(err);
    }else{
        // This will print the user with detailled posts objects instead of ids
        console.log(user);
    }
});
```