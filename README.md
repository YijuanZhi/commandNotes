# MongoBD and Mongoose Commands Notes

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

```

------

# Git learning note

## Git Command

### basic setup

1. git --version >>> print out the version of my git
2. git config --global user.name "Yijuan Zhi" >>> set up my name
3. git config --global user.email "zjzyj520@gmail.com" >>> set up my email

### git command

#### git basics

1. git init . >>> Initialize empty Git repository
2. git status >>> showing the stage for the time being, files added but not committed yet
3. git add file.f >>> tracking the file(s), can add serveral files into one stage
4. git add . >>> add all files into track list
5. git commit -m "note" >>> commit file(s) and throw a message
6. git log >>> showing the log of the file
7. git diff file.f >>> showing the difference since last time modified
   --- git add file.f git commit -m "note2" >>>to commit the new version
8. git commit -am "note" >>> commit all the files, then throw a message

#### git branch, merge and delete

1. git branch newBranch >>> create a new branch
2. git branch >>> showing us what branches we have and which one we are in
3. git checkout newBranch >>> switch to the branch newBranch
4. git checkout -b newBranch2 >>> create a branch newBranch2 and switch to it (newBranch2 is created base on the current branch, say it is newBranch)
5. git merge newBranch2 >>> merge newBranch2 into newBranch when are inside the branch newBranch
   (it also commit changes into the newBranch)
6. git branch -d newBranch2 >>> delete the branch newBranch2

#### git flow

1. git flow init -d >>> initialize git flow and use the default setting (create a branch develop and switch to it)
2. git flow feature start newfeature1 >>> create a branch feature/newfeature1 and switch to it
3. git flow feature finish newfeature1 >>> merge the feature/newfeature1 to develop and delete the branch (use 'i' to insert comments, 'esc' to exit insert mode, write ':wq' to finish commenting)
4. git flow feature publish new-feature >>> push the feature/new-feature, if we don't have this branch in github, it's going to create the branch in github and our computer
5. git flow feature >>> show the branches we have in feature

### git to github

1. ssh-keygen -t rsa -C 'zjzyj520@gmail.com' >>>create a key pair, and connect computer to github
2. cd ~/.ssh >>> the folder we used to execute commands in github remotely
3. (in .ssh foler) cat id_rsa.pub >>> get the key which is to verify your computer and your github account is the same person
4. (in the folder I want to upload to github) git remote add origin url >>> add the folder in the github folder
5. git remote -v >>> showing the folder we have in github
6. git push origin master >>> push the master branch content to master branch in github
7. git push origin develop >>> push the develop branch content to develop branch in github
8. git flow feature publish new-feature >>> push the feature/new-feature, if we don't have this branch in github, it's going to create the branch in github and our computera

#### synchronize files between computers

##### clone

- git clone url >>> this is going to clone the target files into the directory that you are in

##### pull modified files back to another computer

- git pull origin master >>> fetch and merge the files back into the computer
- this is a test, what is this change?

------

# Web Development Commands Notes

## npm
- npm init ~~~~ initialized npm setup files, walk through the json creation process, create the package.json file
- npm install express --save ~~~~ install express and its dependent components, --save write this into our init package json file.
- npm install ejs --save ~~~~ for the ejs support
- npm uninstall [packagename] ~~~~ will uninstall the specified package
- npm list -g --depth=0 ~~~~ will list all the major packages that was installed globally, does not include their dependent packages
- common options: [-P|--save-prod|-D|--save-dev|-O|--save-optional] [-E|--save-exact] [-B|--save-bundle] [--no-save] [--dry-run]
  

## Running
- node [app name] ~~~~ will start the application inside node environment
- nodemon ~~~~ will start the application and restart it when modified


## Others
- curl [url] ~~~~ a tool used for transfer from or to a server

## For ESLint syntax checking
- eslint --init

------

# A note of Emacs hot keys

## Info look up(counsel & swiper)

- C-h t >>> tutorial
- C-h k >>> hot keys info
- C-h v >>> variable info
- C-h f >>> function info
- C-s >>> search(swiper)
- M-x find-function >>> find where and what is the function
- M-x find-variable
- M-x find-function-on-key

## Files

- C-x C-f >>> find or create file
- C-x C-s >>> save the file(buffer)
- C-x C-e >>> run the line
- C-x r >>> open recent history list(moded)
- C-x b >>> switch between buffers
- C-x 1 >>> keep and only keep the current
- C-x 2 >>> keep two windows up and down
- C-x 3 >>> keep two windwos right and left
- M-x load-file >>> load the current file
- C-c c g >>> open counsel-git to find files(moded)
- C-c f >>> open the file's directory in finder(moded)
- M-x counsel-git-grep >>> find the expression in all files

## js2 mode

- C-x C-e >>> nodejs-repl-send-buffer(moded)

## Dired mode (directory)

- C-x C-j >>> open dired mode in the current directory
- C-x d >>> open dired mode
- - > > > create a directory
- C-x C-f >>> create a file
- g >>> refresh dired buffer
- C(capital c) >>> copy file
- D >>> delete file or directory
- R >>> rename
- c >>> marked as copy
- d >>> marked as deletion
- u >>> clear the marks
- x >>> run the marks

## Others

- C-x C-+(or-) to change the font size
- C-g >>> quit current command
- C-M \ >>> indent region(moded)
- S-/ >>> hippie expand

### Org-mode

- C-c C-t >>> switch among TODO, DONE and normal
- M-(return) 自动排序， 并且建立下一行

* <s(tab) >>> begin source emacs-lisp

* C-c ' >>> to edit or quit edit in source code block

#### Agenda

- C-c C-s >>> agenda scheduled
- C-c C-d >>> agenda deadline
- C-c a >>> org-agenda(moded) ------d >>> day || w >>> week || r >>> refresh
