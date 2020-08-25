# MongoInitiate

Get Started with MongoDb

MongoDB is a document database, which means it stores data in JSON-like documents. We believe this is the most natural way to think about data,
and is much more expressive and powerful than the traditional row/column model.

## Install MongoDB On Windows

> To install MongoDB on Windows, first download the latest release of MongoDB from (MongoDb Download)[https://www.mongodb.com/download-center].

> Enter the required details, select the **On-Premises** tab, in it you can choose the version of MongoDB, operating system

> Now install the downloaded file, by default, it will be installed in the folder "C:\Program Files\".

> MongoDB requires a data folder to store its files. The default location for the MongoDB data directory is "c:\data\db". So you need to create this folder using the Command Prompt. Execute the following command sequence.

    ```
    mkdir C:\data\db
    ```

> Then you need to specify set the dbpath to the created directory in mongod.exe. For the same, issue the following commands.

> In the command prompt, navigate to the bin directory current in the MongoDB installation folder. Suppose my installation folder is "C:\Program Files\MongoDB"

```
C:\Users\XYZ>d:cd C:\Program Files\MongoDB\Server\4.2\bin
C:\Program Files\MongoDB\Server\4.2\bin>mongod.exe --dbpath "C:\data"
```

This will show waiting for connections message on the console output, which indicates that the mongod.exe process is running successfully.

Now to run the MongoDB, you need to open another command prompt and issue the following command.

```
C:\Program Files\MongoDB\Server\4.2\bin>mongo.exe
MongoDB shell version v4.2.1
connecting to: mongodb://127.0.0.1:27017/?compressors=disabled&gssapiServiceName=mongodb
Implicit session: session { "id" : UUID("4260beda-f662-4cbe-9bc7-5c1f2242663c") }
MongoDB server version: 4.2.1
>
```

This will show that MongoDB is installed and run successfully. Next time when you run MongoDB, you need to issue only commands.

```
C:\Program Files\MongoDB\Server\4.2\bin>mongod.exe --dbpath "C:\data"
C:\Program Files\MongoDB\Server\4.2\bin>mongo.exe
```

## Start using MongoDB database

### View list of available databases

```
> show dbs
```

this gives the list of all the databases inside of the Mongo Server
**Note: ** the command `show dbs` only show databse with existing collections,
unless the database won't appear on the list if it has not collections inside

### Create a new database or Use an existing one

```
> use db_name
```

this automatically selects the **'db_name'** database

### Show database collections

```
> show collections
```

this gives the list of all the collections inside of the database

### Create a collection

to create a collection we use the command `db.createCollection('posts')`
for our example the collection name is _posts_

### Inserting data to the collection post

_Syntax_

```
db.posts.insert({
    title: 'Post One',
    body: 'Body of Post One',
    category:'News',
    likes: 4,
    tags: ['News', 'events'],
    user:{
        name:'John Doe',
        status:'author'
    },
    date: Date()
})
```

to insert many documents we will use this syntax:

```
db.posts.insertMany([
    {
    title: 'Post Two',
    body: 'Body of Post Two',
    category:'Technology',
    date: Date()
    },
    {
    title: 'Post Three',
    body: 'Body of Post TThreewo',
    category:'Entertainment',
    date: Date()
    },
    {
    title: 'Post Four',
    body: 'Body of Post Four',
    category:'Branding',
    date: Date()
    }
])
```

### FInd Data in a collection

```
db.posts.find()
```

we can also use the function **pretty** for a good display as followed:

```
db.posts.find().pretty()
```

Needing to find one document, we use:

```
db.posts.findOne({category:'News'})
```

to find a post which has got **Branding** as category we use this syntax:

```
db.posts.find({category:'Branding'}).pretty()
```

we can **sort** some data doing: `db.posts.find().sort({title: 1}).pretty()`

here 1 is ascending and -1 is for descending

### Counting data

_Syntax_

```
db.posts.find().count()
```

### Limit data

_Syntax_

```
db.posts.find().pretty().limit(2)
```

### Retrieve using **Foreach** function

_Syntax_

```
db.posts.find().forEach(function(doc){print('Blog Post: '+doc.title)})

```

### Update data in a collection

_Syntax_
the first way to update document in a collection is:

```
db.posts.update({ title: 'Post Two'},
    {
        title: 'Post Two',
        body: 'New Post 2 Body',
        date: Date()
    },
    {
        upsert: true
    }
)
```

**upsert**: if it doesn't find the where data (**{ title: 'Post Two'}**) it will create it
_Note_: with this syntax we ommited the category, and it goes away after update, cause this update replace everything by what we have put,
but if we want to only replace where is change we do:

```
db.posts.update({ title: 'Post Two'},
    {
        $set: {
            body: 'Body of the post 2',
            category: 'technology'
        }
    }
)
```

### Update by increment

```
db.posts.update({title: 'Post One'},{$inc:{likes: 2}})
```

Some other Update code

```
db.posts.update({title:'Post Two'},
    {
        $set: {
            views:10
        }
    }
)
```

```
db.posts.find({
    views: {
        $gt:7
    }
}).pretty()
```

the property **gt** means **greater than**, **gte** for **greater and equal to**,
**lt** for **less than** and **lte** fro **less than equal to**

### Renaming a field

```
db.posts.update({title: 'Post One'},{$rename:{likes: 'views'}})
```

### Remove a document

```
db.posts.remove({title: 'Post Four'})
```

let's add a comment into "Post one"

```
db.posts.update({ title: 'Post One'},
    {
        $set: {
            comment:[
                {
                    user: 'Brain Joshua',
                    body: 'Comment Three',
                    date: Date()
                },
                {
                    user: 'Harry White',
                    body: 'Comment Two',
                    date: Date()
                },

            ]
        }
    }
)
```

Doing a research on the Post One / comment, we will do:

```
db.posts.find({
    comments:{
        $elemMatch:{
            user: 'Harry White'
        }
    }
})
```

### Create index

```
db.posts.createIndex({title: 'text'})
```

if we want to find by this index we do

```
db.posts.find({
    $text: {
        $search: "\"Post O\""
    }
})
```
