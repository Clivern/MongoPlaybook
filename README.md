# Run Mongo
```
> mongo
```

# Import Collection
```
> mongoimport --db test --collection restaurants --drop --file primer-dataset.json
```
# Show All Databases
```
> show databases
```

# Get Current Database
```
> db
```

# Use Another Database
```
> use test
```

# Show Database Collections
```
> show collections
```

# Datatypes
```
{'x': null} # null
{'x': true} # Boolean
{'x': 3} # Integer
{'x': 3.2} # Float Point
{'x': 'hello world'} # String
{'x': new Date()} # Date
{'x': /foobar/i} # Regex
{'x': ['a','b','c']} # Array
{'x': {'foo':'bar'}} # Embedded Documents
{'x': ObjectId()} # Id
```

# Counting Collection Records
```
> db.restaurants.count()
25359
```

# Insert Record
```
> db.posts.insert({'title': 'hello world', 'author': 5, 'created_at': new Date()})
WriteResult({ "nInserted" : 1 })
```

# Batch Insert
```
> db.posts.insert([{'title': 'hello world1', 'author': 6, 'created_at': new Date()}, {'title': 'hello world2', 'author': 7, 'created_at': new Date()}])
BulkWriteResult({
    "writeErrors" : [ ],
    "writeConcernErrors" : [ ],
    "nInserted" : 2,
    "nUpserted" : 0,
    "nMatched" : 0,
    "nModified" : 0,
    "nRemoved" : 0,
    "upserted" : [ ]
})
```

# Find All
```
> db.posts.find()
{ "_id" : ObjectId("571bde257c1d1340ef01c29f"), "title" : "hello world", "author" : 5, "created_at" : ISODate("2016-04-23T20:42:13.971Z") }
{ "_id" : ObjectId("571bdf367c1d1340ef01c2a0"), "title" : "hello world1", "author" : 6, "created_at" : ISODate("2016-04-23T20:46:46.132Z") }
{ "_id" : ObjectId("571bdf367c1d1340ef01c2a1"), "title" : "hello world2", "author" : 7, "created_at" : ISODate("2016-04-23T20:46:46.132Z") }
```

# Remove With Query
```
> db.posts.find()
{ "_id" : ObjectId("571bde257c1d1340ef01c29f"), "title" : "hello world", "author" : 5, "created_at" : ISODate("2016-04-23T20:42:13.971Z") }
{ "_id" : ObjectId("571bdf367c1d1340ef01c2a0"), "title" : "hello world1", "author" : 6, "created_at" : ISODate("2016-04-23T20:46:46.132Z") }
{ "_id" : ObjectId("571bdf367c1d1340ef01c2a1"), "title" : "hello world2", "author" : 7, "created_at" : ISODate("2016-04-23T20:46:46.132Z") }
> db.posts.remove({'title':'hello world'})
WriteResult({ "nRemoved" : 1 })
> db.posts.find()
{ "_id" : ObjectId("571bdf367c1d1340ef01c2a0"), "title" : "hello world1", "author" : 6, "created_at" : ISODate("2016-04-23T20:46:46.132Z") }
{ "_id" : ObjectId("571bdf367c1d1340ef01c2a1"), "title" : "hello world2", "author" : 7, "created_at" : ISODate("2016-04-23T20:46:46.132Z") }
```

# Find One
```
> db.posts.findOne({'title':'hello world'})
null
> db.posts.findOne({'title':'hello world1'})
{
    "_id" : ObjectId("571bdf367c1d1340ef01c2a0"),
    "title" : "hello world1",
    "author" : 6,
    "created_at" : ISODate("2016-04-23T20:46:46.132Z")
}
```

# Find All
```
> db.posts.find()
{ "_id" : ObjectId("571bdf367c1d1340ef01c2a0"), "title" : "hello world1", "author" : 6, "created_at" : ISODate("2016-04-23T20:46:46.132Z") }
{ "_id" : ObjectId("571bdf367c1d1340ef01c2a1"), "title" : "hello world2", "author" : 7, "created_at" : ISODate("2016-04-23T20:46:46.132Z") }
> db.posts.find({'author':6})
{ "_id" : ObjectId("571bdf367c1d1340ef01c2a0"), "title" : "hello world1", "author" : 6, "created_at" : ISODate("2016-04-23T20:46:46.132Z") }
```

# Replace Document
```
> post = db.posts.findOne()
{
    "_id" : ObjectId("571bdf367c1d1340ef01c2a0"),
    "title" : "hello world1",
    "author" : 6,
    "created_at" : ISODate("2016-04-23T20:46:46.132Z")
}
> post.title = "new hello world"
new hello world
> post
{
    "_id" : ObjectId("571bdf367c1d1340ef01c2a0"),
    "title" : "new hello world",
    "author" : 6,
    "created_at" : ISODate("2016-04-23T20:46:46.132Z")
}
> db.posts.update({'title': 'hello world1'}, post)
WriteResult({ "nMatched" : 1, "nUpserted" : 0, "nModified" : 1 })
> db.posts.findOne()
{
    "_id" : ObjectId("571bdf367c1d1340ef01c2a0"),
    "title" : "new hello world",
    "author" : 6,
    "created_at" : ISODate("2016-04-23T20:46:46.132Z")
}
```

# Using Modifiers

## `$inc` Modifier

```
> post = db.posts.findOne()
{
    "_id" : ObjectId("571bdf367c1d1340ef01c2a0"),
    "title" : "new hello world",
    "author" : 6,
    "created_at" : ISODate("2016-04-23T20:46:46.132Z")
}
> post.views = 0
0
> db.posts.findOne()
{
    "_id" : ObjectId("571bdf367c1d1340ef01c2a0"),
    "title" : "new hello world",
    "author" : 6,
    "created_at" : ISODate("2016-04-23T20:46:46.132Z")
}
> db.posts.update({'title': 'new hello world'}, post)
WriteResult({ "nMatched" : 1, "nUpserted" : 0, "nModified" : 1 })
> db.posts.findOne()
{
    "_id" : ObjectId("571bdf367c1d1340ef01c2a0"),
    "title" : "new hello world",
    "author" : 6,
    "created_at" : ISODate("2016-04-23T20:46:46.132Z"),
    "views" : 0
}
> db.posts.update({'title': 'new hello world'}, {'$inc':{'views': 1}})
WriteResult({ "nMatched" : 1, "nUpserted" : 0, "nModified" : 1 })
> db.posts.findOne()
{
    "_id" : ObjectId("571bdf367c1d1340ef01c2a0"),
    "title" : "new hello world",
    "author" : 6,
    "created_at" : ISODate("2016-04-23T20:46:46.132Z"),
    "views" : 1
}

> db.posts.update({'title': 'new hello world'}, {'$inc':{'views': -1}})
WriteResult({ "nMatched" : 1, "nUpserted" : 0, "nModified" : 1 })
> db.posts.findOne()
{
    "_id" : ObjectId("571bdf367c1d1340ef01c2a0"),
    "title" : "new hello world",
    "author" : 6,
    "created_at" : ISODate("2016-04-23T20:46:46.132Z"),
    "views" : 0
}

> db.posts.update({'title': 'new hello world'}, {'$inc':{'views': 50}})
WriteResult({ "nMatched" : 1, "nUpserted" : 0, "nModified" : 1 })
> db.posts.findOne()
{
    "_id" : ObjectId("571bdf367c1d1340ef01c2a0"),
    "title" : "new hello world",
    "author" : 6,
    "created_at" : ISODate("2016-04-23T20:46:46.132Z"),
    "views" : 50
}
```

## `$set` Modifier
```
> db.posts.update({'title': 'new hello world'}, {'$set':{'comments': []}})
WriteResult({ "nMatched" : 1, "nUpserted" : 0, "nModified" : 1 })
> db.posts.findOne()
{
    "_id" : ObjectId("571bdf367c1d1340ef01c2a0"),
    "title" : "new hello world",
    "author" : 6,
    "created_at" : ISODate("2016-04-23T20:46:46.132Z"),
    "views" : 1,
    "comments" : [ ]
}
```

## `$unset` Modifier
```
> db.posts.update({'title': 'new hello world'}, {'$unset':{'comments': 1}})
WriteResult({ "nMatched" : 1, "nUpserted" : 0, "nModified" : 1 })
> db.posts.findOne()
{
    "_id" : ObjectId("571bdf367c1d1340ef01c2a0"),
    "title" : "new hello world",
    "author" : 6,
    "created_at" : ISODate("2016-04-23T20:46:46.132Z"),
    "views" : 1
}
```

## `$push` Modifier
```
> db.posts.update({'title': 'new hello world'}, {'$set':{'comments': []}})
WriteResult({ "nMatched" : 1, "nUpserted" : 0, "nModified" : 1 })
> db.posts.findOne()
{
    "_id" : ObjectId("571bdf367c1d1340ef01c2a0"),
    "title" : "new hello world",
    "author" : 6,
    "created_at" : ISODate("2016-04-23T20:46:46.132Z"),
    "views" : 50,
    "comments" : [ ]
}
> db.posts.update({'title': 'new hello world'}, {'$push':{'comments': {'author': 'hello@clivern.com', 'comment': 'bla bla bla', 'created_at': new Date()}}})
WriteResult({ "nMatched" : 1, "nUpserted" : 0, "nModified" : 1 })
> db.posts.findOne()
{
    "_id" : ObjectId("571bdf367c1d1340ef01c2a0"),
    "title" : "new hello world",
    "author" : 6,
    "created_at" : ISODate("2016-04-23T20:46:46.132Z"),
    "views" : 50,
    "comments" : [
        {
            "author" : "hello@clivern.com",
            "comment" : "bla bla bla",
            "created_at" : ISODate("2016-04-23T21:27:51.863Z")
        }
    ]
}
```

## `$push` and `$each` Modifier
```
> db.posts.update({'title': 'new hello world'}, {'$push':{'comments': {'$each':[{'author': 'hello1@clivern.com', 'comment': 'bla bla bla1', 'created_at': new Date()}, {'author': 'hello2@clivern.com', 'comment': 'bla bla bla2', 'created_at': new Date()}, {'author': 'hello@clivern.com3', 'comment': 'bla bla bla3', 'created_at': new Date()}]}}})
WriteResult({ "nMatched" : 1, "nUpserted" : 0, "nModified" : 1 })
> db.posts.findOne()
{
    "_id" : ObjectId("571bdf367c1d1340ef01c2a0"),
    "title" : "new hello world",
    "author" : 6,
    "created_at" : ISODate("2016-04-23T20:46:46.132Z"),
    "views" : 50,
    "comments" : [
        {
            "author" : "hello@clivern.com",
            "comment" : "bla bla bla",
            "created_at" : ISODate("2016-04-23T21:27:51.863Z")
        },
        {
            "author" : "hello1@clivern.com",
            "comment" : "bla bla bla1",
            "created_at" : ISODate("2016-04-23T21:30:21.597Z")
        },
        {
            "author" : "hello2@clivern.com",
            "comment" : "bla bla bla2",
            "created_at" : ISODate("2016-04-23T21:30:21.597Z")
        },
        {
            "author" : "hello@clivern.com3",
            "comment" : "bla bla bla3",
            "created_at" : ISODate("2016-04-23T21:30:21.597Z")
        }
    ]
}
```

## `$push`, `$each` and `$slice` Modifier
```
> db.posts.update({'title': 'new hello world'}, {'$push':{'comments': {'$each':[{'author': 'hello1@clivern.com', 'comment': 'bla bla bla1', 'created_at': new Date()}, {'author': 'hello2@clivern.com', 'comment': 'bla bla bla2', 'created_at': new Date()}, {'author': 'hello@clivern.com3', 'comment': 'bla bla bla3', 'created_at': new Date()}]}}})
WriteResult({ "nMatched" : 1, "nUpserted" : 0, "nModified" : 1 })
> db.posts.findOne()
{
    "_id" : ObjectId("571bdf367c1d1340ef01c2a0"),
    "title" : "new hello world",
    "author" : 6,
    "created_at" : ISODate("2016-04-23T20:46:46.132Z"),
    "views" : 50,
    "comments" : [
        {
            "author" : "hello@clivern.com",
            "comment" : "bla bla bla",
            "created_at" : ISODate("2016-04-23T21:27:51.863Z")
        },
        {
            "author" : "hello1@clivern.com",
            "comment" : "bla bla bla1",
            "created_at" : ISODate("2016-04-23T21:30:21.597Z")
        },
        {
            "author" : "hello2@clivern.com",
            "comment" : "bla bla bla2",
            "created_at" : ISODate("2016-04-23T21:30:21.597Z")
        },
        {
            "author" : "hello@clivern.com3",
            "comment" : "bla bla bla3",
            "created_at" : ISODate("2016-04-23T21:30:21.597Z")
        }
    ]
}
> db.posts.update({'title': 'new hello world'}, {'$push':{'comments': {'$each':[{'author': 'hello1@clivern.com', 'comment': 'bla bla bla1', 'created_at': new Date()}, {'author': 'hello2@clivern.com', 'comment': 'bla bla bla2', 'created_at': new Date()}, {'author': 'hello@clivern.com3', 'comment': 'bla bla bla3', 'created_at': new Date()}], '$slice': -4}}})
WriteResult({ "nMatched" : 1, "nUpserted" : 0, "nModified" : 1 })
> db.posts.findOne()
{
    "_id" : ObjectId("571bdf367c1d1340ef01c2a0"),
    "title" : "new hello world",
    "author" : 6,
    "created_at" : ISODate("2016-04-23T20:46:46.132Z"),
    "views" : 50,
    "comments" : [
        {
            "author" : "hello@clivern.com3",
            "comment" : "bla bla bla3",
            "created_at" : ISODate("2016-04-23T21:30:21.597Z")
        },
        {
            "author" : "hello1@clivern.com",
            "comment" : "bla bla bla1",
            "created_at" : ISODate("2016-04-23T21:33:50.653Z")
        },
        {
            "author" : "hello2@clivern.com",
            "comment" : "bla bla bla2",
            "created_at" : ISODate("2016-04-23T21:33:50.653Z")
        },
        {
            "author" : "hello@clivern.com3",
            "comment" : "bla bla bla3",
            "created_at" : ISODate("2016-04-23T21:33:50.653Z")
        }
    ]
}
```

## `$push`, `$each`, `$slice` and `$sort` Modifier
```
> db.posts.update({'title': 'new hello world'}, {'$push':{'comments': {'$each':[{'author': 'hello1@clivern.com', 'comment': 'bla bla bla1', 'created_at': new Date()}, {'author': 'hello2@clivern.com', 'comment': 'bla bla bla2', 'created_at': new Date()}, {'author': 'hello@clivern.com3', 'comment': 'bla bla bla3', 'created_at': new Date()}], '$slice': -4}}})
WriteResult({ "nMatched" : 1, "nUpserted" : 0, "nModified" : 1 })
> db.posts.findOne()
{
    "_id" : ObjectId("571bdf367c1d1340ef01c2a0"),
    "title" : "new hello world",
    "author" : 6,
    "created_at" : ISODate("2016-04-23T20:46:46.132Z"),
    "views" : 50,
    "comments" : [
        {
            "author" : "hello@clivern.com3",
            "comment" : "bla bla bla3",
            "created_at" : ISODate("2016-04-23T21:35:56.109Z")
        },
        {
            "author" : "hello1@clivern.com",
            "comment" : "bla bla bla1",
            "created_at" : ISODate("2016-04-23T21:36:12.697Z")
        },
        {
            "author" : "hello2@clivern.com",
            "comment" : "bla bla bla2",
            "created_at" : ISODate("2016-04-23T21:36:12.697Z")
        },
        {
            "author" : "hello@clivern.com3",
            "comment" : "bla bla bla3",
            "created_at" : ISODate("2016-04-23T21:36:12.697Z")
        }
    ]
}
> db.posts.update({'title': 'new hello world'}, {'$push':{'comments': {'$each':[{'author': 'hello1@clivern.com', 'comment': 'bla bla bla1', 'created_at': new Date()}, {'author': 'hello2@clivern.com', 'comment': 'bla bla bla2', 'created_at': new Date()}, {'author': 'hello@clivern.com3', 'comment': 'bla bla bla3', 'created_at': new Date()}], '$slice': -4, '$sort': {'author': 1}}}})
WriteResult({ "nMatched" : 1, "nUpserted" : 0, "nModified" : 1 })
> db.posts.findOne()
{
    "_id" : ObjectId("571bdf367c1d1340ef01c2a0"),
    "title" : "new hello world",
    "author" : 6,
    "created_at" : ISODate("2016-04-23T20:46:46.132Z"),
    "views" : 50,
    "comments" : [
        {
            "author" : "hello2@clivern.com",
            "comment" : "bla bla bla2",
            "created_at" : ISODate("2016-04-23T21:36:21.669Z")
        },
        {
            "author" : "hello@clivern.com3",
            "comment" : "bla bla bla3",
            "created_at" : ISODate("2016-04-23T21:35:56.109Z")
        },
        {
            "author" : "hello@clivern.com3",
            "comment" : "bla bla bla3",
            "created_at" : ISODate("2016-04-23T21:36:12.697Z")
        },
        {
            "author" : "hello@clivern.com3",
            "comment" : "bla bla bla3",
            "created_at" : ISODate("2016-04-23T21:36:21.669Z")
        }
    ]
}
```

## `$addToSet` Modifier
```
> db.posts.findOne()
{
    "_id" : ObjectId("571bdf367c1d1340ef01c2a0"),
    "title" : "new hello world",
    "author" : 6,
    "created_at" : ISODate("2016-04-23T20:46:46.132Z"),
    "views" : 50,
    "comments" : [
        {
            "author" : "hello2@clivern.com",
            "comment" : "bla bla bla2",
            "created_at" : ISODate("2016-04-23T21:36:21.669Z")
        },
        {
            "author" : "hello@clivern.com3",
            "comment" : "bla bla bla3",
            "created_at" : ISODate("2016-04-23T21:35:56.109Z")
        },
        {
            "author" : "hello@clivern.com3",
            "comment" : "bla bla bla3",
            "created_at" : ISODate("2016-04-23T21:36:12.697Z")
        },
        {
            "author" : "hello@clivern.com3",
            "comment" : "bla bla bla3",
            "created_at" : ISODate("2016-04-23T21:36:21.669Z")
        }
    ]
}
> db.posts.update({'title': 'new hello world'}, {'$set':{'tags':['php']}})
WriteResult({ "nMatched" : 1, "nUpserted" : 0, "nModified" : 1 })
> db.posts.findOne()
{
    "_id" : ObjectId("571bdf367c1d1340ef01c2a0"),
    "title" : "new hello world",
    "author" : 6,
    "created_at" : ISODate("2016-04-23T20:46:46.132Z"),
    "views" : 50,
    "comments" : [
        {
            "author" : "hello2@clivern.com",
            "comment" : "bla bla bla2",
            "created_at" : ISODate("2016-04-23T21:36:21.669Z")
        },
        {
            "author" : "hello@clivern.com3",
            "comment" : "bla bla bla3",
            "created_at" : ISODate("2016-04-23T21:35:56.109Z")
        },
        {
            "author" : "hello@clivern.com3",
            "comment" : "bla bla bla3",
            "created_at" : ISODate("2016-04-23T21:36:12.697Z")
        },
        {
            "author" : "hello@clivern.com3",
            "comment" : "bla bla bla3",
            "created_at" : ISODate("2016-04-23T21:36:21.669Z")
        }
    ],
    "tags" : [
        "php"
    ]
}
> db.posts.update({'title': 'new hello world'}, {'$addToSet':{'tags':'php'}})
WriteResult({ "nMatched" : 1, "nUpserted" : 0, "nModified" : 0 })
> db.posts.findOne()
{
    "_id" : ObjectId("571bdf367c1d1340ef01c2a0"),
    "title" : "new hello world",
    "author" : 6,
    "created_at" : ISODate("2016-04-23T20:46:46.132Z"),
    "views" : 50,
    "comments" : [
        {
            "author" : "hello2@clivern.com",
            "comment" : "bla bla bla2",
            "created_at" : ISODate("2016-04-23T21:36:21.669Z")
        },
        {
            "author" : "hello@clivern.com3",
            "comment" : "bla bla bla3",
            "created_at" : ISODate("2016-04-23T21:35:56.109Z")
        },
        {
            "author" : "hello@clivern.com3",
            "comment" : "bla bla bla3",
            "created_at" : ISODate("2016-04-23T21:36:12.697Z")
        },
        {
            "author" : "hello@clivern.com3",
            "comment" : "bla bla bla3",
            "created_at" : ISODate("2016-04-23T21:36:21.669Z")
        }
    ],
    "tags" : [
        "php"
    ]
}
> db.posts.update({'title': 'new hello world'}, {'$addToSet':{'tags':'jQuery'}})
WriteResult({ "nMatched" : 1, "nUpserted" : 0, "nModified" : 1 })
> db.posts.findOne()
{
    "_id" : ObjectId("571bdf367c1d1340ef01c2a0"),
    "title" : "new hello world",
    "author" : 6,
    "created_at" : ISODate("2016-04-23T20:46:46.132Z"),
    "views" : 50,
    "comments" : [
        {
            "author" : "hello2@clivern.com",
            "comment" : "bla bla bla2",
            "created_at" : ISODate("2016-04-23T21:36:21.669Z")
        },
        {
            "author" : "hello@clivern.com3",
            "comment" : "bla bla bla3",
            "created_at" : ISODate("2016-04-23T21:35:56.109Z")
        },
        {
            "author" : "hello@clivern.com3",
            "comment" : "bla bla bla3",
            "created_at" : ISODate("2016-04-23T21:36:12.697Z")
        },
        {
            "author" : "hello@clivern.com3",
            "comment" : "bla bla bla3",
            "created_at" : ISODate("2016-04-23T21:36:21.669Z")
        }
    ],
    "tags" : [
        "php",
        "jQuery"
    ]
}
```

## `$pull` Modifier
```
> db.posts.findOne()
{
    "_id" : ObjectId("571bdf367c1d1340ef01c2a0"),
    "title" : "new hello world",
    "author" : 6,
    "created_at" : ISODate("2016-04-23T20:46:46.132Z"),
    "views" : 50,
    "comments" : [
        {
            "author" : "hello2@clivern.com",
            "comment" : "bla bla bla2",
            "created_at" : ISODate("2016-04-23T21:36:21.669Z")
        },
        {
            "author" : "hello@clivern.com3",
            "comment" : "bla bla bla3",
            "created_at" : ISODate("2016-04-23T21:35:56.109Z")
        },
        {
            "author" : "hello@clivern.com3",
            "comment" : "bla bla bla3",
            "created_at" : ISODate("2016-04-23T21:36:12.697Z")
        },
        {
            "author" : "hello@clivern.com3",
            "comment" : "bla bla bla3",
            "created_at" : ISODate("2016-04-23T21:36:21.669Z")
        }
    ],
    "tags" : [
        "php",
        "jQuery"
    ]
}
> db.posts.update({'title': 'new hello world'}, {'$pull':{'tags':'jQuery'}})
WriteResult({ "nMatched" : 1, "nUpserted" : 0, "nModified" : 1 })
> db.posts.findOne()
{
    "_id" : ObjectId("571bdf367c1d1340ef01c2a0"),
    "title" : "new hello world",
    "author" : 6,
    "created_at" : ISODate("2016-04-23T20:46:46.132Z"),
    "views" : 50,
    "comments" : [
        {
            "author" : "hello2@clivern.com",
            "comment" : "bla bla bla2",
            "created_at" : ISODate("2016-04-23T21:36:21.669Z")
        },
        {
            "author" : "hello@clivern.com3",
            "comment" : "bla bla bla3",
            "created_at" : ISODate("2016-04-23T21:35:56.109Z")
        },
        {
            "author" : "hello@clivern.com3",
            "comment" : "bla bla bla3",
            "created_at" : ISODate("2016-04-23T21:36:12.697Z")
        },
        {
            "author" : "hello@clivern.com3",
            "comment" : "bla bla bla3",
            "created_at" : ISODate("2016-04-23T21:36:21.669Z")
        }
    ],
    "tags" : [
        "php"
    ]
}
```

## Positional Edits
```
> db.posts.findOne()
{
    "_id" : ObjectId("571bdf367c1d1340ef01c2a0"),
    "title" : "new hello world",
    "author" : 6,
    "created_at" : ISODate("2016-04-23T20:46:46.132Z"),
    "views" : 50,
    "comments" : [
        {
            "author" : "hello2@clivern.com",
            "comment" : "bla bla bla2",
            "created_at" : ISODate("2016-04-23T21:36:21.669Z")
        },
        {
            "author" : "hello@clivern.com3",
            "comment" : "bla bla bla3",
            "created_at" : ISODate("2016-04-23T21:35:56.109Z")
        },
        {
            "author" : "hello@clivern.com3",
            "comment" : "bla bla bla3",
            "created_at" : ISODate("2016-04-23T21:36:12.697Z")
        },
        {
            "author" : "hello@clivern.com3",
            "comment" : "bla bla bla3",
            "created_at" : ISODate("2016-04-23T21:36:21.669Z")
        }
    ],
    "tags" : [
        "php"
    ]
}
> db.posts.update({'title': 'new hello world'}, {'$set':{'comments.0.votes':20}})
WriteResult({ "nMatched" : 1, "nUpserted" : 0, "nModified" : 1 })
> db.posts.findOne()
{
    "_id" : ObjectId("571bdf367c1d1340ef01c2a0"),
    "title" : "new hello world",
    "author" : 6,
    "created_at" : ISODate("2016-04-23T20:46:46.132Z"),
    "views" : 50,
    "comments" : [
        {
            "author" : "hello2@clivern.com",
            "comment" : "bla bla bla2",
            "created_at" : ISODate("2016-04-23T21:36:21.669Z"),
            "votes" : 20
        },
        {
            "author" : "hello@clivern.com3",
            "comment" : "bla bla bla3",
            "created_at" : ISODate("2016-04-23T21:35:56.109Z")
        },
        {
            "author" : "hello@clivern.com3",
            "comment" : "bla bla bla3",
            "created_at" : ISODate("2016-04-23T21:36:12.697Z")
        },
        {
            "author" : "hello@clivern.com3",
            "comment" : "bla bla bla3",
            "created_at" : ISODate("2016-04-23T21:36:21.669Z")
        }
    ],
    "tags" : [
        "php"
    ]
}
```

## Update With Upsert is `True`
```
> db.posts.update({'title': 'new world with upsert'}, {'$set':{'comments':[]}}, true)
WriteResult({
    "nMatched" : 0,
    "nUpserted" : 1,
    "nModified" : 0,
    "_id" : ObjectId("571bed5c9c6a01cfb58206e8")
})
> db.posts.findOne({'title': 'new world with upsert'})
{
    "_id" : ObjectId("571bed5c9c6a01cfb58206e8"),
    "title" : "new world with upsert",
    "comments" : [ ]
}

> db.posts.update({'title': 'new world with upsert'}, {'$setOnInsert':{'comments':[]}}, true)
WriteResult({
    "nMatched" : 0,
    "nUpserted" : 1,
    "nModified" : 0,
    "_id" : ObjectId("571bed5c9c6a01cfb58206e8")
})
> db.posts.findOne({'title': 'new world with upsert'})
{
    "_id" : ObjectId("571bed5c9c6a01cfb58206e8"),
    "title" : "new world with upsert",
    "comments" : [ ]
}
```

# Remove All Documents in Collection
```
> db.posts.remove({})
WriteResult({ "nRemoved" : 5 })
```


# Querying

## Bulk Insert
```
> db.users.insert([{'_id': 1, 'first_name': 'Mark', 'last_name': 'Doe', 'username': 'admin', 'email': 'admin@clivern.com', 'job_title': 'CEO', 'created_at': new Date(), 'updated_at': new Date()}, {'_id': 2, 'first_name': 'Selena', 'last_name': 'Gomez', 'username': 'selena', 'email': 'selena@clivern.com', 'job_title': 'Backend Developer', 'created_at': new Date(), 'updated_at': new Date()}, {'_id': 3, 'first_name': 'Helen', 'last_name': 'Otal', 'username': 'helen', 'email': 'helen@clivern.com', 'job_title': 'Web Designer', 'created_at': new Date(), 'updated_at': new Date()}, {'_id': 4, 'first_name': 'Joe', 'last_name': 'Mar', 'username': 'joe', 'email': 'joe@clivern.com', 'job_title': 'Support Agent', 'created_at': new Date(), 'updated_at': new Date()}])
BulkWriteResult({
    "writeErrors" : [ ],
    "writeConcernErrors" : [ ],
    "nInserted" : 4,
    "nUpserted" : 0,
    "nMatched" : 0,
    "nModified" : 0,
    "nRemoved" : 0,
    "upserted" : [ ]
})
```

## Find All
```
> db.users.find({}).pretty()
{
    "_id" : 1,
    "first_name" : "Mark",
    "last_name" : "Doe",
    "username" : "admin",
    "email" : "admin@clivern.com",
    "job_title" : "CEO",
    "created_at" : ISODate("2016-04-24T17:52:24.040Z"),
    "updated_at" : ISODate("2016-04-24T17:52:24.040Z")
}
{
    "_id" : 2,
    "first_name" : "Selena",
    "last_name" : "Gomez",
    "username" : "selena",
    "email" : "selena@clivern.com",
    "job_title" : "Backend Developer",
    "created_at" : ISODate("2016-04-24T17:52:24.040Z"),
    "updated_at" : ISODate("2016-04-24T17:52:24.040Z")
}
{
    "_id" : 3,
    "first_name" : "Helen",
    "last_name" : "Otal",
    "username" : "helen",
    "email" : "helen@clivern.com",
    "job_title" : "Web Designer",
    "created_at" : ISODate("2016-04-24T17:52:24.040Z"),
    "updated_at" : ISODate("2016-04-24T17:52:24.040Z")
}
{
    "_id" : 4,
    "first_name" : "Joe",
    "last_name" : "Mar",
    "username" : "joe",
    "email" : "joe@clivern.com",
    "job_title" : "Support Agent",
    "created_at" : ISODate("2016-04-24T17:52:24.040Z"),
    "updated_at" : ISODate("2016-04-24T17:52:24.040Z")
}
```

## Exclude Keys
```
> db.users.find({}, {'created_at': 0, 'updated_at': 0}).pretty()
{
    "_id" : 1,
    "first_name" : "Mark",
    "last_name" : "Doe",
    "username" : "admin",
    "email" : "admin@clivern.com",
    "job_title" : "CEO"
}
{
    "_id" : 2,
    "first_name" : "Selena",
    "last_name" : "Gomez",
    "username" : "selena",
    "email" : "selena@clivern.com",
    "job_title" : "Backend Developer"
}
{
    "_id" : 3,
    "first_name" : "Helen",
    "last_name" : "Otal",
    "username" : "helen",
    "email" : "helen@clivern.com",
    "job_title" : "Web Designer"
}
{
    "_id" : 4,
    "first_name" : "Joe",
    "last_name" : "Mar",
    "username" : "joe",
    "email" : "joe@clivern.com",
    "job_title" : "Support Agent"
}
```

## Query Conditionals
```
> db.users.find({'username': 'admin'}, {'created_at': 0, 'updated_at': 0}).pretty()
{
    "_id" : 1,
    "first_name" : "Mark",
    "last_name" : "Doe",
    "username" : "admin",
    "email" : "admin@clivern.com",
    "job_title" : "CEO"
}

> db.users.find({'_id': {'$gt': 2}}, {'created_at': 0, 'updated_at': 0}).pretty()
{
    "_id" : 3,
    "first_name" : "Helen",
    "last_name" : "Otal",
    "username" : "helen",
    "email" : "helen@clivern.com",
    "job_title" : "Web Designer"
}
{
    "_id" : 4,
    "first_name" : "Joe",
    "last_name" : "Mar",
    "username" : "joe",
    "email" : "joe@clivern.com",
    "job_title" : "Support Agent"
}

> db.users.find({'_id': {'$gte': 2}}, {'created_at': 0, 'updated_at': 0}).pretty()
{
    "_id" : 2,
    "first_name" : "Selena",
    "last_name" : "Gomez",
    "username" : "selena",
    "email" : "selena@clivern.com",
    "job_title" : "Backend Developer"
}
{
    "_id" : 3,
    "first_name" : "Helen",
    "last_name" : "Otal",
    "username" : "helen",
    "email" : "helen@clivern.com",
    "job_title" : "Web Designer"
}
{
    "_id" : 4,
    "first_name" : "Joe",
    "last_name" : "Mar",
    "username" : "joe",
    "email" : "joe@clivern.com",
    "job_title" : "Support Agent"
}

> db.users.find({'_id': {'$lt': 2}}, {'created_at': 0, 'updated_at': 0}).pretty()
{
    "_id" : 1,
    "first_name" : "Mark",
    "last_name" : "Doe",
    "username" : "admin",
    "email" : "admin@clivern.com",
    "job_title" : "CEO"
}

> db.users.find({'_id': {'$lte': 2}}, {'created_at': 0, 'updated_at': 0}).pretty()
{
    "_id" : 1,
    "first_name" : "Mark",
    "last_name" : "Doe",
    "username" : "admin",
    "email" : "admin@clivern.com",
    "job_title" : "CEO"
}
{
    "_id" : 2,
    "first_name" : "Selena",
    "last_name" : "Gomez",
    "username" : "selena",
    "email" : "selena@clivern.com",
    "job_title" : "Backend Developer"
}

> db.users.find({'_id': {'$ne': 2}}, {'created_at': 0, 'updated_at': 0}).pretty()
{
    "_id" : 1,
    "first_name" : "Mark",
    "last_name" : "Doe",
    "username" : "admin",
    "email" : "admin@clivern.com",
    "job_title" : "CEO"
}
{
    "_id" : 3,
    "first_name" : "Helen",
    "last_name" : "Otal",
    "username" : "helen",
    "email" : "helen@clivern.com",
    "job_title" : "Web Designer"
}
{
    "_id" : 4,
    "first_name" : "Joe",
    "last_name" : "Mar",
    "username" : "joe",
    "email" : "joe@clivern.com",
    "job_title" : "Support Agent"
}

> db.users.find({'created_at': {'$gt': new Date()}}, {'created_at': 0, 'updated_at': 0}).pretty()
> db.users.find({'created_at': {'$lt': new Date()}}, {'created_at': 0, 'updated_at': 0}).pretty()
{
    "_id" : 1,
    "first_name" : "Mark",
    "last_name" : "Doe",
    "username" : "admin",
    "email" : "admin@clivern.com",
    "job_title" : "CEO"
}
{
    "_id" : 2,
    "first_name" : "Selena",
    "last_name" : "Gomez",
    "username" : "selena",
    "email" : "selena@clivern.com",
    "job_title" : "Backend Developer"
}
{
    "_id" : 3,
    "first_name" : "Helen",
    "last_name" : "Otal",
    "username" : "helen",
    "email" : "helen@clivern.com",
    "job_title" : "Web Designer"
}
{
    "_id" : 4,
    "first_name" : "Joe",
    "last_name" : "Mar",
    "username" : "joe",
    "email" : "joe@clivern.com",
    "job_title" : "Support Agent"
}

> db.users.find({'_id': {'$in': [1, 2]}}, {'created_at': 0, 'updated_at': 0}).pretty()
{
    "_id" : 1,
    "first_name" : "Mark",
    "last_name" : "Doe",
    "username" : "admin",
    "email" : "admin@clivern.com",
    "job_title" : "CEO"
}
{
    "_id" : 2,
    "first_name" : "Selena",
    "last_name" : "Gomez",
    "username" : "selena",
    "email" : "selena@clivern.com",
    "job_title" : "Backend Developer"
}

> db.users.find({'_id': {'$nin': [1, 2]}}, {'created_at': 0, 'updated_at': 0}).pretty()
{
    "_id" : 3,
    "first_name" : "Helen",
    "last_name" : "Otal",
    "username" : "helen",
    "email" : "helen@clivern.com",
    "job_title" : "Web Designer"
}
{
    "_id" : 4,
    "first_name" : "Joe",
    "last_name" : "Mar",
    "username" : "joe",
    "email" : "joe@clivern.com",
    "job_title" : "Support Agent"
}

> db.users.find({'$or': [{'first_name': 'Helen' }, {'first_name': 'Selena'}]}, {'created_at': 0, 'updated_at': 0}).pretty()
{
    "_id" : 2,
    "first_name" : "Selena",
    "last_name" : "Gomez",
    "username" : "selena",
    "email" : "selena@clivern.com",
    "job_title" : "Backend Developer"
}
{
    "_id" : 3,
    "first_name" : "Helen",
    "last_name" : "Otal",
    "username" : "helen",
    "email" : "helen@clivern.com",
    "job_title" : "Web Designer"
}

> db.users.find({'$and': [{'first_name': 'Helen' }, {'first_name': 'Selena'}]}, {'created_at': 0, 'updated_at': 0}).pretty()
> db.users.find({'$and': [{'first_name': 'Helen' }, {'last_name': 'Otal'}]}, {'created_at': 0, 'updated_at': 0}).pretty()
{
    "_id" : 3,
    "first_name" : "Helen",
    "last_name" : "Otal",
    "username" : "helen",
    "email" : "helen@clivern.com",
    "job_title" : "Web Designer"
}

> db.users.find({'votes': null}, {'created_at': 0, 'updated_at': 0}).pretty()
{
    "_id" : 1,
    "first_name" : "Mark",
    "last_name" : "Doe",
    "username" : "admin",
    "email" : "admin@clivern.com",
    "job_title" : "CEO"
}
{
    "_id" : 2,
    "first_name" : "Selena",
    "last_name" : "Gomez",
    "username" : "selena",
    "email" : "selena@clivern.com",
    "job_title" : "Backend Developer"
}
{
    "_id" : 3,
    "first_name" : "Helen",
    "last_name" : "Otal",
    "username" : "helen",
    "email" : "helen@clivern.com",
    "job_title" : "Web Designer"
}
{
    "_id" : 4,
    "first_name" : "Joe",
    "last_name" : "Mar",
    "username" : "joe",
    "email" : "joe@clivern.com",
    "job_title" : "Support Agent"
}

> db.users.find({'votes': {'$exists': true}}, {'created_at': 0, 'updated_at': 0}).pretty()
> db.users.find({'votes': {'$exists': false}}, {'created_at': 0, 'updated_at': 0}).pretty()
{
    "_id" : 1,
    "first_name" : "Mark",
    "last_name" : "Doe",
    "username" : "admin",
    "email" : "admin@clivern.com",
    "job_title" : "CEO"
}
{
    "_id" : 2,
    "first_name" : "Selena",
    "last_name" : "Gomez",
    "username" : "selena",
    "email" : "selena@clivern.com",
    "job_title" : "Backend Developer"
}
{
    "_id" : 3,
    "first_name" : "Helen",
    "last_name" : "Otal",
    "username" : "helen",
    "email" : "helen@clivern.com",
    "job_title" : "Web Designer"
}
{
    "_id" : 4,
    "first_name" : "Joe",
    "last_name" : "Mar",
    "username" : "joe",
    "email" : "joe@clivern.com",
    "job_title" : "Support Agent"
}

> db.users.find({'first_name': /mark/i}, {'created_at': 0, 'updated_at': 0}).pretty()
{
    "_id" : 1,
    "first_name" : "Mark",
    "last_name" : "Doe",
    "username" : "admin",
    "email" : "admin@clivern.com",
    "job_title" : "CEO"
}
> db.users.find({'first_name': /mar/i}, {'created_at': 0, 'updated_at': 0}).pretty()
{
    "_id" : 1,
    "first_name" : "Mark",
    "last_name" : "Doe",
    "username" : "admin",
    "email" : "admin@clivern.com",
    "job_title" : "CEO"
}

> db.users.find({'first_name': /^mar$/i}, {'created_at': 0, 'updated_at': 0}).pretty()
> db.users.find({'first_name': /^mark$/i}, {'created_at': 0, 'updated_at': 0}).pretty()
{
    "_id" : 1,
    "first_name" : "Mark",
    "last_name" : "Doe",
    "username" : "admin",
    "email" : "admin@clivern.com",
    "job_title" : "CEO"
}


> db.users.update({'_id': 1}, {'$set': {'tags': ['php', 'javascript', 'css']}})
WriteResult({ "nMatched" : 1, "nUpserted" : 0, "nModified" : 1 })
> db.users.update({'_id': 2}, {'$set': {'tags': ['jquery', 'javascript', 'python']}})
WriteResult({ "nMatched" : 1, "nUpserted" : 0, "nModified" : 1 })
> db.users.update({'_id': 3}, {'$set': {'tags': ['angular', 'javascript', 'haproxy']}})
WriteResult({ "nMatched" : 1, "nUpserted" : 0, "nModified" : 1 })
> db.users.update({'_id': 4}, {'$set': {'tags': ['angular3', 'vuejs', 'haproxy']}})
WriteResult({ "nMatched" : 1, "nUpserted" : 0, "nModified" : 1 })
> db.users.find({}).pretty()
{
    "_id" : 1,
    "first_name" : "Mark",
    "last_name" : "Doe",
    "username" : "admin",
    "email" : "admin@clivern.com",
    "job_title" : "CEO",
    "created_at" : ISODate("2016-04-24T17:52:24.040Z"),
    "updated_at" : ISODate("2016-04-24T17:52:24.040Z"),
    "tags" : [
        "php",
        "javascript",
        "css"
    ]
}
{
    "_id" : 2,
    "first_name" : "Selena",
    "last_name" : "Gomez",
    "username" : "selena",
    "email" : "selena@clivern.com",
    "job_title" : "Backend Developer",
    "created_at" : ISODate("2016-04-24T17:52:24.040Z"),
    "updated_at" : ISODate("2016-04-24T17:52:24.040Z"),
    "tags" : [
        "jquery",
        "javascript",
        "python"
    ]
}
{
    "_id" : 3,
    "first_name" : "Helen",
    "last_name" : "Otal",
    "username" : "helen",
    "email" : "helen@clivern.com",
    "job_title" : "Web Designer",
    "created_at" : ISODate("2016-04-24T17:52:24.040Z"),
    "updated_at" : ISODate("2016-04-24T17:52:24.040Z"),
    "tags" : [
        "angular",
        "javascript",
        "haproxy"
    ]
}
{
    "_id" : 4,
    "first_name" : "Joe",
    "last_name" : "Mar",
    "username" : "joe",
    "email" : "joe@clivern.com",
    "job_title" : "Support Agent",
    "created_at" : ISODate("2016-04-24T17:52:24.040Z"),
    "updated_at" : ISODate("2016-04-24T17:52:24.040Z"),
    "tags" : [
        "angular3",
        "vuejs",
        "haproxy"
    ]
}
> db.users.find({'tags': ['angular3', 'vuejs', 'haproxy']}).pretty()
{
    "_id" : 4,
    "first_name" : "Joe",
    "last_name" : "Mar",
    "username" : "joe",
    "email" : "joe@clivern.com",
    "job_title" : "Support Agent",
    "created_at" : ISODate("2016-04-24T17:52:24.040Z"),
    "updated_at" : ISODate("2016-04-24T17:52:24.040Z"),
    "tags" : [
        "angular3",
        "vuejs",
        "haproxy"
    ]
}
> db.users.find({'tags': ['angular3', 'haproxy', 'vuejs']}).pretty()
> db.users.find({'tags': {'$all': ['angular3', 'haproxy', 'vuejs']}}).pretty()
{
    "_id" : 4,
    "first_name" : "Joe",
    "last_name" : "Mar",
    "username" : "joe",
    "email" : "joe@clivern.com",
    "job_title" : "Support Agent",
    "created_at" : ISODate("2016-04-24T17:52:24.040Z"),
    "updated_at" : ISODate("2016-04-24T17:52:24.040Z"),
    "tags" : [
        "angular3",
        "vuejs",
        "haproxy"
    ]
}

> db.users.find({'tags': {'$size': 3}}).pretty()
{
    "_id" : 1,
    "first_name" : "Mark",
    "last_name" : "Doe",
    "username" : "admin",
    "email" : "admin@clivern.com",
    "job_title" : "CEO",
    "created_at" : ISODate("2016-04-24T17:52:24.040Z"),
    "updated_at" : ISODate("2016-04-24T17:52:24.040Z"),
    "tags" : [
        "php",
        "javascript",
        "css"
    ]
}
{
    "_id" : 2,
    "first_name" : "Selena",
    "last_name" : "Gomez",
    "username" : "selena",
    "email" : "selena@clivern.com",
    "job_title" : "Backend Developer",
    "created_at" : ISODate("2016-04-24T17:52:24.040Z"),
    "updated_at" : ISODate("2016-04-24T17:52:24.040Z"),
    "tags" : [
        "jquery",
        "javascript",
        "python"
    ]
}
{
    "_id" : 3,
    "first_name" : "Helen",
    "last_name" : "Otal",
    "username" : "helen",
    "email" : "helen@clivern.com",
    "job_title" : "Web Designer",
    "created_at" : ISODate("2016-04-24T17:52:24.040Z"),
    "updated_at" : ISODate("2016-04-24T17:52:24.040Z"),
    "tags" : [
        "angular",
        "javascript",
        "haproxy"
    ]
}
{
    "_id" : 4,
    "first_name" : "Joe",
    "last_name" : "Mar",
    "username" : "joe",
    "email" : "joe@clivern.com",
    "job_title" : "Support Agent",
    "created_at" : ISODate("2016-04-24T17:52:24.040Z"),
    "updated_at" : ISODate("2016-04-24T17:52:24.040Z"),
    "tags" : [
        "angular3",
        "vuejs",
        "haproxy"
    ]
}
> db.users.find({'tags': {'$size': 1}}).pretty()
> db.users.find({'tags': {'$size': 2}}).pretty()

> db.users.find({'tags.1': 'vuejs'}).pretty()
{
    "_id" : 4,
    "first_name" : "Joe",
    "last_name" : "Mar",
    "username" : "joe",
    "email" : "joe@clivern.com",
    "job_title" : "Support Agent",
    "created_at" : ISODate("2016-04-24T17:52:24.040Z"),
    "updated_at" : ISODate("2016-04-24T17:52:24.040Z"),
    "tags" : [
        "angular3",
        "vuejs",
        "haproxy"
    ]
}

> db.users.find({}).pretty()
{
    "_id" : 1,
    "first_name" : "Mark",
    "last_name" : "Doe",
    "username" : "admin",
    "email" : "admin@clivern.com",
    "job_title" : "CEO",
    "created_at" : ISODate("2016-04-24T17:52:24.040Z"),
    "updated_at" : ISODate("2016-04-24T17:52:24.040Z"),
    "tags" : [
        "php",
        "javascript",
        "css"
    ]
}
{
    "_id" : 2,
    "first_name" : "Selena",
    "last_name" : "Gomez",
    "username" : "selena",
    "email" : "selena@clivern.com",
    "job_title" : "Backend Developer",
    "created_at" : ISODate("2016-04-24T17:52:24.040Z"),
    "updated_at" : ISODate("2016-04-24T17:52:24.040Z"),
    "tags" : [
        "jquery",
        "javascript",
        "python"
    ]
}
{
    "_id" : 3,
    "first_name" : "Helen",
    "last_name" : "Otal",
    "username" : "helen",
    "email" : "helen@clivern.com",
    "job_title" : "Web Designer",
    "created_at" : ISODate("2016-04-24T17:52:24.040Z"),
    "updated_at" : ISODate("2016-04-24T17:52:24.040Z"),
    "tags" : [
        "angular",
        "javascript",
        "haproxy"
    ]
}
{
    "_id" : 4,
    "first_name" : "Joe",
    "last_name" : "Mar",
    "username" : "joe",
    "email" : "joe@clivern.com",
    "job_title" : "Support Agent",
    "created_at" : ISODate("2016-04-24T17:52:24.040Z"),
    "updated_at" : ISODate("2016-04-24T17:52:24.040Z"),
    "tags" : [
        "angular3",
        "vuejs",
        "haproxy"
    ]
}
> db.users.find({}).skip(2).pretty()
{
    "_id" : 3,
    "first_name" : "Helen",
    "last_name" : "Otal",
    "username" : "helen",
    "email" : "helen@clivern.com",
    "job_title" : "Web Designer",
    "created_at" : ISODate("2016-04-24T17:52:24.040Z"),
    "updated_at" : ISODate("2016-04-24T17:52:24.040Z"),
    "tags" : [
        "angular",
        "javascript",
        "haproxy"
    ]
}
{
    "_id" : 4,
    "first_name" : "Joe",
    "last_name" : "Mar",
    "username" : "joe",
    "email" : "joe@clivern.com",
    "job_title" : "Support Agent",
    "created_at" : ISODate("2016-04-24T17:52:24.040Z"),
    "updated_at" : ISODate("2016-04-24T17:52:24.040Z"),
    "tags" : [
        "angular3",
        "vuejs",
        "haproxy"
    ]
}
> db.users.find({}).skip(2).limit(1).pretty()
{
    "_id" : 3,
    "first_name" : "Helen",
    "last_name" : "Otal",
    "username" : "helen",
    "email" : "helen@clivern.com",
    "job_title" : "Web Designer",
    "created_at" : ISODate("2016-04-24T17:52:24.040Z"),
    "updated_at" : ISODate("2016-04-24T17:52:24.040Z"),
    "tags" : [
        "angular",
        "javascript",
        "haproxy"
    ]
}

> db.users.find({}).sort({'_id': 1}).pretty()
{
    "_id" : 1,
    "first_name" : "Mark",
    "last_name" : "Doe",
    "username" : "admin",
    "email" : "admin@clivern.com",
    "job_title" : "CEO",
    "created_at" : ISODate("2016-04-24T17:52:24.040Z"),
    "updated_at" : ISODate("2016-04-24T17:52:24.040Z"),
    "tags" : [
        "php",
        "javascript",
        "css"
    ]
}
{
    "_id" : 2,
    "first_name" : "Selena",
    "last_name" : "Gomez",
    "username" : "selena",
    "email" : "selena@clivern.com",
    "job_title" : "Backend Developer",
    "created_at" : ISODate("2016-04-24T17:52:24.040Z"),
    "updated_at" : ISODate("2016-04-24T17:52:24.040Z"),
    "tags" : [
        "jquery",
        "javascript",
        "python"
    ]
}
{
    "_id" : 3,
    "first_name" : "Helen",
    "last_name" : "Otal",
    "username" : "helen",
    "email" : "helen@clivern.com",
    "job_title" : "Web Designer",
    "created_at" : ISODate("2016-04-24T17:52:24.040Z"),
    "updated_at" : ISODate("2016-04-24T17:52:24.040Z"),
    "tags" : [
        "angular",
        "javascript",
        "haproxy"
    ]
}
{
    "_id" : 4,
    "first_name" : "Joe",
    "last_name" : "Mar",
    "username" : "joe",
    "email" : "joe@clivern.com",
    "job_title" : "Support Agent",
    "created_at" : ISODate("2016-04-24T17:52:24.040Z"),
    "updated_at" : ISODate("2016-04-24T17:52:24.040Z"),
    "tags" : [
        "angular3",
        "vuejs",
        "haproxy"
    ]
}
> db.users.find({}).sort({'_id': -1}).pretty()
{
    "_id" : 4,
    "first_name" : "Joe",
    "last_name" : "Mar",
    "username" : "joe",
    "email" : "joe@clivern.com",
    "job_title" : "Support Agent",
    "created_at" : ISODate("2016-04-24T17:52:24.040Z"),
    "updated_at" : ISODate("2016-04-24T17:52:24.040Z"),
    "tags" : [
        "angular3",
        "vuejs",
        "haproxy"
    ]
}
{
    "_id" : 3,
    "first_name" : "Helen",
    "last_name" : "Otal",
    "username" : "helen",
    "email" : "helen@clivern.com",
    "job_title" : "Web Designer",
    "created_at" : ISODate("2016-04-24T17:52:24.040Z"),
    "updated_at" : ISODate("2016-04-24T17:52:24.040Z"),
    "tags" : [
        "angular",
        "javascript",
        "haproxy"
    ]
}
{
    "_id" : 2,
    "first_name" : "Selena",
    "last_name" : "Gomez",
    "username" : "selena",
    "email" : "selena@clivern.com",
    "job_title" : "Backend Developer",
    "created_at" : ISODate("2016-04-24T17:52:24.040Z"),
    "updated_at" : ISODate("2016-04-24T17:52:24.040Z"),
    "tags" : [
        "jquery",
        "javascript",
        "python"
    ]
}
{
    "_id" : 1,
    "first_name" : "Mark",
    "last_name" : "Doe",
    "username" : "admin",
    "email" : "admin@clivern.com",
    "job_title" : "CEO",
    "created_at" : ISODate("2016-04-24T17:52:24.040Z"),
    "updated_at" : ISODate("2016-04-24T17:52:24.040Z"),
    "tags" : [
        "php",
        "javascript",
        "css"
    ]
}
```

## Indexing

```
> db.restaurants.find({'address.street': '3 Avenue'}).explain('executionStats')
{
    "queryPlanner" : {
        "plannerVersion" : 1,
        "namespace" : "test.restaurants",
        "indexFilterSet" : false,
        "parsedQuery" : {
            "address.street" : {
                "$eq" : "3 Avenue"
            }
        },
        "winningPlan" : {
            "stage" : "COLLSCAN",
            "filter" : {
                "address.street" : {
                    "$eq" : "3 Avenue"
                }
            },
            "direction" : "forward"
        },
        "rejectedPlans" : [ ]
    },
    "executionStats" : {
        "executionSuccess" : true,
        "nReturned" : 553,
        "executionTimeMillis" : 20,
        "totalKeysExamined" : 0,
        "totalDocsExamined" : 25359,
        "executionStages" : {
            "stage" : "COLLSCAN",
            "filter" : {
                "address.street" : {
                    "$eq" : "3 Avenue"
                }
            },
            "nReturned" : 553,
            "executionTimeMillisEstimate" : 20,
            "works" : 25361,
            "advanced" : 553,
            "needTime" : 24807,
            "needYield" : 0,
            "saveState" : 198,
            "restoreState" : 198,
            "isEOF" : 1,
            "invalidates" : 0,
            "direction" : "forward",
            "docsExamined" : 25359
        }
    },
    "serverInfo" : {
        "host" : "2c9af55923a4",
        "port" : 27017,
        "version" : "3.2.4",
        "gitVersion" : "e2ee9ffcf9f5a94fad76802e28cc978718bb7a30"
    },
    "ok" : 1
}

> db.restaurants.find({'address.street': '3 Avenue'}).explain('executionStats').executionStats.totalDocsExamined
25359
> db.restaurants.ensureIndex({'address.street': 1})
{
    "createdCollectionAutomatically" : false,
    "numIndexesBefore" : 1,
    "numIndexesAfter" : 2,
    "ok" : 1
}
> db.restaurants.find({'address.street': '3 Avenue'}).explain('executionStats').executionStats.totalDocsExamined
553
> db.restaurants.dropIndex({'address.street': 1})
{ "nIndexesWas" : 2, "ok" : 1 }
> db.restaurants.find({'address.street': '3 Avenue'}).explain('executionStats').executionStats.totalDocsExamined
25359
```

# Aggregation Framework

## `$project` Modifier
```
> db.users.find({}).pretty()
{
    "_id" : 1,
    "first_name" : "Mark",
    "last_name" : "Doe",
    "username" : "admin",
    "email" : "admin@clivern.com",
    "job_title" : "CEO",
    "created_at" : ISODate("2016-04-24T17:52:24.040Z"),
    "updated_at" : ISODate("2016-04-24T17:52:24.040Z"),
    "tags" : [
        "php",
        "javascript",
        "css"
    ]
}
{
    "_id" : 2,
    "first_name" : "Selena",
    "last_name" : "Gomez",
    "username" : "selena",
    "email" : "selena@clivern.com",
    "job_title" : "Backend Developer",
    "created_at" : ISODate("2016-04-24T17:52:24.040Z"),
    "updated_at" : ISODate("2016-04-24T17:52:24.040Z"),
    "tags" : [
        "jquery",
        "javascript",
        "python"
    ]
}
{
    "_id" : 3,
    "first_name" : "Helen",
    "last_name" : "Otal",
    "username" : "helen",
    "email" : "helen@clivern.com",
    "job_title" : "Web Designer",
    "created_at" : ISODate("2016-04-24T17:52:24.040Z"),
    "updated_at" : ISODate("2016-04-24T17:52:24.040Z"),
    "tags" : [
        "angular",
        "javascript",
        "haproxy"
    ]
}
{
    "_id" : 4,
    "first_name" : "Joe",
    "last_name" : "Mar",
    "username" : "joe",
    "email" : "joe@clivern.com",
    "job_title" : "Support Agent",
    "created_at" : ISODate("2016-04-24T17:52:24.040Z"),
    "updated_at" : ISODate("2016-04-24T17:52:24.040Z"),
    "tags" : [
        "angular3",
        "vuejs",
        "haproxy"
    ]
}

> db.users.aggregate({'$project': {'username': 1}})
{ "_id" : 1, "username" : "admin" }
{ "_id" : 2, "username" : "selena" }
{ "_id" : 3, "username" : "helen" }
{ "_id" : 4, "username" : "joe" }

> db.users.aggregate({'$project': {'username': 1, '_id': 0}})
{ "username" : "admin" }
{ "username" : "selena" }
{ "username" : "helen" }
{ "username" : "joe" }

> db.users.aggregate({'$project': {'username': '$_id', '_id': 0}})
{ "username" : 1 }
{ "username" : 2 }
{ "username" : 3 }
{ "username" : 4 }

> db.users.aggregate({'$project': {'id': '$username', '_id': 0}})
{ "id" : "admin" }
{ "id" : "selena" }
{ "id" : "helen" }
{ "id" : "joe" }

> db.users.aggregate({'$project': {'field_name': '$username', '_id': 0}})
{ "field_name" : "admin" }
{ "field_name" : "selena" }
{ "field_name" : "helen" }
{ "field_name" : "joe" }
```

## `$match` Modifier
```
> db.users.aggregate({'$project': {'id': '$username', '_id': 0}}, {'$match': {'id': 'admin'}})
{ "id" : "admin" }

> db.users.aggregate({'$project': {'id': '$_id', '_id': 0}}, {'$match': {'id': {'$gt': 1}}})
{ "id" : 2 }
{ "id" : 3 }
{ "id" : 4 }

> db.users.aggregate({'$project': {'id': '$_id', '_id': 0}}, {'$match': {'id': {'$gte': 1}}})
{ "id" : 1 }
{ "id" : 2 }
{ "id" : 3 }
{ "id" : 4 }

> db.users.aggregate({'$project': {'id': '$_id', '_id': 0}}, {'$match': {'id': {'$lte': 1}}})
{ "id" : 1 }

> db.users.aggregate({'$project': {'id': '$_id', '_id': 0}}, {'$match': {'id': {'$lt': 2}}})
{ "id" : 1 }

> db.users.aggregate({'$project': {'id': '$_id', '_id': 0}}, {'$match': {'id': {'$in': [1, 2, 4]}}})
{ "id" : 1 }
{ "id" : 2 }
{ "id" : 4 }
```

## `$add`, `$subtract`, `$multiply`, `$divide` and `$mod` Modifier
```
> db.results.find({}).pretty()
{
    "_id" : 1,
    "name" : "mark",
    "math_result" : 60,
    "physics_result" : 23,
    "cs_result" : 78
}
{
    "_id" : 2,
    "name" : "helen",
    "math_result" : 50,
    "physics_result" : 48,
    "cs_result" : 74
}
{
    "_id" : 3,
    "name" : "timber",
    "math_result" : 65,
    "physics_result" : 14,
    "cs_result" : 56
}
{
    "_id" : 4,
    "name" : "demi",
    "math_result" : 87,
    "physics_result" : 78,
    "cs_result" : 14
}
{
    "_id" : 5,
    "name" : "jenny",
    "math_result" : 76,
    "physics_result" : 45,
    "cs_result" : 78
}

> db.results.aggregate({'$project': {'total': {'$add': ['$math_result', '$cs_result']}}})
{ "_id" : 1, "total" : 138 }
{ "_id" : 2, "total" : 124 }
{ "_id" : 3, "total" : 121 }
{ "_id" : 4, "total" : 101 }
{ "_id" : 5, "total" : 154 }

> db.results.aggregate({'$project': {'total': {'$add': ['$math_result', '$physics_result' , '$cs_result']}}})
{ "_id" : 1, "total" : 161 }
{ "_id" : 2, "total" : 172 }
{ "_id" : 3, "total" : 135 }
{ "_id" : 4, "total" : 179 }
{ "_id" : 5, "total" : 199 }

> db.results.aggregate({'$project': {'total': {'$subtract': [{'$add': ['$math_result', '$physics_result' , '$cs_result']}, 100]}}})
{ "_id" : 1, "total" : 61 }
{ "_id" : 2, "total" : 72 }
{ "_id" : 3, "total" : 35 }
{ "_id" : 4, "total" : 79 }
{ "_id" : 5, "total" : 99 }

> db.results.aggregate({'$project': {'total': {'$multiply': [{'$add': ['$math_result', '$physics_result' , '$cs_result']}, 100]}}})
{ "_id" : 1, "total" : 16100 }
{ "_id" : 2, "total" : 17200 }
{ "_id" : 3, "total" : 13500 }
{ "_id" : 4, "total" : 17900 }
{ "_id" : 5, "total" : 19900 }

> db.results.aggregate({'$project': {'total': {'$divide': [{'$add': ['$math_result', '$physics_result' , '$cs_result']}, 100]}}})
{ "_id" : 1, "total" : 1.61 }
{ "_id" : 2, "total" : 1.72 }
{ "_id" : 3, "total" : 1.35 }
{ "_id" : 4, "total" : 1.79 }
{ "_id" : 5, "total" : 1.99 }

> db.results.aggregate({'$project': {'total': {'$mod': [{'$add': ['$math_result', '$physics_result' , '$cs_result']}, 100]}}})
{ "_id" : 1, "total" : 61 }
{ "_id" : 2, "total" : 72 }
{ "_id" : 3, "total" : 35 }
{ "_id" : 4, "total" : 79 }
{ "_id" : 5, "total" : 99 }
```

## `$substr`, `$concat`, `$toLower` and `$toUpper` Modifier
```
> db.tuts.find({}).pretty()
{
    "_id" : ObjectId("571e06a776dcd773abde06c7"),
    "title" : "Hello World",
    "author" : "Mark",
    "created_at" : ISODate("2016-04-25T11:59:35.742Z")
}
{
    "_id" : ObjectId("571e06a776dcd773abde06c8"),
    "title" : "Post Title",
    "author" : "Helen",
    "created_at" : ISODate("2016-04-25T11:59:35.742Z")
}
{
    "_id" : ObjectId("571e06a776dcd773abde06c9"),
    "title" : "WordPress Article",
    "author" : "Jenny",
    "created_at" : ISODate("2016-04-25T11:59:35.742Z")
}

> db.tuts.aggregate({'$project': {'title': {'$substr': ['$title', 0, 1]}}})
{ "_id" : ObjectId("571e06a776dcd773abde06c7"), "title" : "H" }
{ "_id" : ObjectId("571e06a776dcd773abde06c8"), "title" : "P" }
{ "_id" : ObjectId("571e06a776dcd773abde06c9"), "title" : "W" }

> db.tuts.aggregate({'$project': {'title': {'$concat': ['title: ', '$title']}}})
{ "_id" : ObjectId("571e06a776dcd773abde06c7"), "title" : "title: Hello World" }
{ "_id" : ObjectId("571e06a776dcd773abde06c8"), "title" : "title: Post Title" }
{ "_id" : ObjectId("571e06a776dcd773abde06c9"), "title" : "title: WordPress Article" }

> db.tuts.aggregate({'$project': {'title': {'$toLower': '$title'}}})
{ "_id" : ObjectId("571e06a776dcd773abde06c7"), "title" : "hello world" }
{ "_id" : ObjectId("571e06a776dcd773abde06c8"), "title" : "post title" }
{ "_id" : ObjectId("571e06a776dcd773abde06c9"), "title" : "wordpress article" }

> db.tuts.aggregate({'$project': {'title': {'$toUpper': '$title'}}})
{ "_id" : ObjectId("571e06a776dcd773abde06c7"), "title" : "HELLO WORLD" }
{ "_id" : ObjectId("571e06a776dcd773abde06c8"), "title" : "POST TITLE" }
{ "_id" : ObjectId("571e06a776dcd773abde06c9"), "title" : "WORDPRESS ARTICLE" }
```

## `$year`, `$month`, `$week`, `$hour`, `$minute`, `$second`, `$dayOfMonth`, `$dayOfWeek` and `$dayOfYear` Modifier
```
> db.tuts.aggregate({'$project': {'created_in_year': {'$year': '$created_at'}}})
{ "_id" : ObjectId("571e06a776dcd773abde06c7"), "created_in_year" : 2016 }
{ "_id" : ObjectId("571e06a776dcd773abde06c8"), "created_in_year" : 2016 }
{ "_id" : ObjectId("571e06a776dcd773abde06c9"), "created_in_year" : 2016 }

> db.tuts.aggregate({'$project': {'created_in_month': {'$month': '$created_at'}}})
{ "_id" : ObjectId("571e06a776dcd773abde06c7"), "created_in_month" : 4 }
{ "_id" : ObjectId("571e06a776dcd773abde06c8"), "created_in_month" : 4 }
{ "_id" : ObjectId("571e06a776dcd773abde06c9"), "created_in_month" : 4 }

> db.tuts.aggregate({'$project': {'created_in_week': {'$week': '$created_at'}}})
{ "_id" : ObjectId("571e06a776dcd773abde06c7"), "created_in_week" : 17 }
{ "_id" : ObjectId("571e06a776dcd773abde06c8"), "created_in_week" : 17 }
{ "_id" : ObjectId("571e06a776dcd773abde06c9"), "created_in_week" : 17 }

> db.tuts.aggregate({'$project': {'created_in_hour': {'$hour': '$created_at'}}})
{ "_id" : ObjectId("571e06a776dcd773abde06c7"), "created_in_hour" : 11 }
{ "_id" : ObjectId("571e06a776dcd773abde06c8"), "created_in_hour" : 11 }
{ "_id" : ObjectId("571e06a776dcd773abde06c9"), "created_in_hour" : 11 }

> db.tuts.aggregate({'$project': {'created_in_minute': {'$minute': '$created_at'}}})
{ "_id" : ObjectId("571e06a776dcd773abde06c7"), "created_in_minute" : 59 }
{ "_id" : ObjectId("571e06a776dcd773abde06c8"), "created_in_minute" : 59 }
{ "_id" : ObjectId("571e06a776dcd773abde06c9"), "created_in_minute" : 59 }

> db.tuts.aggregate({'$project': {'created_in_second': {'$second': '$created_at'}}})
{ "_id" : ObjectId("571e06a776dcd773abde06c7"), "created_in_second" : 35 }
{ "_id" : ObjectId("571e06a776dcd773abde06c8"), "created_in_second" : 35 }
{ "_id" : ObjectId("571e06a776dcd773abde06c9"), "created_in_second" : 35 }

> db.tuts.aggregate({'$project': {'created_in_day_of_month': {'$dayOfMonth': '$created_at'}}})
{ "_id" : ObjectId("571e06a776dcd773abde06c7"), "created_in_day_of_month" : 25 }
{ "_id" : ObjectId("571e06a776dcd773abde06c8"), "created_in_day_of_month" : 25 }
{ "_id" : ObjectId("571e06a776dcd773abde06c9"), "created_in_day_of_month" : 25 }

> db.tuts.aggregate({'$project': {'created_in_day_of_week': {'$dayOfWeek': '$created_at'}}})
{ "_id" : ObjectId("571e06a776dcd773abde06c7"), "created_in_day_of_week" : 2 }
{ "_id" : ObjectId("571e06a776dcd773abde06c8"), "created_in_day_of_week" : 2 }
{ "_id" : ObjectId("571e06a776dcd773abde06c9"), "created_in_day_of_week" : 2 }

> db.tuts.aggregate({'$project': {'created_in_day_of_year': {'$dayOfYear': '$created_at'}}})
{ "_id" : ObjectId("571e06a776dcd773abde06c7"), "created_in_day_of_year" : 116 }
{ "_id" : ObjectId("571e06a776dcd773abde06c8"), "created_in_day_of_year" : 116 }
{ "_id" : ObjectId("571e06a776dcd773abde06c9"), "created_in_day_of_year" : 116 }
```

## `$cmp`, `$strcasecmp`, `$eq`, `$ne`, `$gt`, `$gte`, `$lt`, `$lte`, `$and`, `$or`, `$not`, `$cond` and `$ifNull` Modifier
```
> db.tuts.aggregate({'$project': {'title': {'$cmp': ['Hello World', '$title']}}})
{ "_id" : ObjectId("571e06a776dcd773abde06c7"), "title" : 0 }
{ "_id" : ObjectId("571e06a776dcd773abde06c8"), "title" : -1 }
{ "_id" : ObjectId("571e06a776dcd773abde06c9"), "title" : -1 }

> db.tuts.aggregate({'$project': {'title': {'$cmp': ['hello World', '$title']}}})
{ "_id" : ObjectId("571e06a776dcd773abde06c7"), "title" : 1 }
{ "_id" : ObjectId("571e06a776dcd773abde06c8"), "title" : 1 }
{ "_id" : ObjectId("571e06a776dcd773abde06c9"), "title" : 1 }

> db.tuts.aggregate({'$project': {'title': {'$strcasecmp': ['hello World', '$title']}}})
{ "_id" : ObjectId("571e06a776dcd773abde06c7"), "title" : 0 }
{ "_id" : ObjectId("571e06a776dcd773abde06c8"), "title" : -1 }
{ "_id" : ObjectId("571e06a776dcd773abde06c9"), "title" : -1 }

> db.tuts.aggregate({'$project': {'title': {'$eq': ['Hello World', '$title']}}})
{ "_id" : ObjectId("571e06a776dcd773abde06c7"), "title" : true }
{ "_id" : ObjectId("571e06a776dcd773abde06c8"), "title" : false }
{ "_id" : ObjectId("571e06a776dcd773abde06c9"), "title" : false }

> db.tuts.aggregate({'$project': {'title': {'$ne': ['Hello World', '$title']}}})
{ "_id" : ObjectId("571e06a776dcd773abde06c7"), "title" : false }
{ "_id" : ObjectId("571e06a776dcd773abde06c8"), "title" : true }
{ "_id" : ObjectId("571e06a776dcd773abde06c9"), "title" : true }

> db.tuts.aggregate({'$project': {'title': {'$gt': ['Hello World', '$title']}}})
{ "_id" : ObjectId("571e06a776dcd773abde06c7"), "title" : false }
{ "_id" : ObjectId("571e06a776dcd773abde06c8"), "title" : false }
{ "_id" : ObjectId("571e06a776dcd773abde06c9"), "title" : false }

> db.tuts.aggregate({'$project': {'title': {'$gte': ['Hello World', '$title']}}})
{ "_id" : ObjectId("571e06a776dcd773abde06c7"), "title" : true }
{ "_id" : ObjectId("571e06a776dcd773abde06c8"), "title" : false }
{ "_id" : ObjectId("571e06a776dcd773abde06c9"), "title" : false }

> db.tuts.aggregate({'$project': {'title': {'$lt': ['Hello World', '$title']}}})
{ "_id" : ObjectId("571e06a776dcd773abde06c7"), "title" : false }
{ "_id" : ObjectId("571e06a776dcd773abde06c8"), "title" : true }
{ "_id" : ObjectId("571e06a776dcd773abde06c9"), "title" : true }

> db.tuts.aggregate({'$project': {'title': {'$lte': ['Hello World', '$title']}}})
{ "_id" : ObjectId("571e06a776dcd773abde06c7"), "title" : true }
{ "_id" : ObjectId("571e06a776dcd773abde06c8"), "title" : true }
{ "_id" : ObjectId("571e06a776dcd773abde06c9"), "title" : true }

> db.tuts.aggregate({'$project': {'title': {'$and': [0, '$title']}}})
{ "_id" : ObjectId("571e06a776dcd773abde06c7"), "title" : false }
{ "_id" : ObjectId("571e06a776dcd773abde06c8"), "title" : false }
{ "_id" : ObjectId("571e06a776dcd773abde06c9"), "title" : false }

> db.tuts.aggregate({'$project': {'title': {'$or': [0, '$title']}}})
{ "_id" : ObjectId("571e06a776dcd773abde06c7"), "title" : true }
{ "_id" : ObjectId("571e06a776dcd773abde06c8"), "title" : true }
{ "_id" : ObjectId("571e06a776dcd773abde06c9"), "title" : true }

> db.tuts.aggregate({'$project': {'title': {'$not': '$title'}}})
{ "_id" : ObjectId("571e06a776dcd773abde06c7"), "title" : false }
{ "_id" : ObjectId("571e06a776dcd773abde06c8"), "title" : false }
{ "_id" : ObjectId("571e06a776dcd773abde06c9"), "title" : false }

> db.tuts.aggregate({'$project': {'title': {'$cond': ['$title', 'not empty', 'empty']}}})
{ "_id" : ObjectId("571e06a776dcd773abde06c7"), "title" : "not empty" }
{ "_id" : ObjectId("571e06a776dcd773abde06c8"), "title" : "not empty" }
{ "_id" : ObjectId("571e06a776dcd773abde06c9"), "title" : "not empty" }

> db.tuts.aggregate({'$project': {'title': {'$ifNull': ['$title', 'null value']}}})
{ "_id" : ObjectId("571e06a776dcd773abde06c7"), "title" : "Hello World" }
{ "_id" : ObjectId("571e06a776dcd773abde06c8"), "title" : "Post Title" }
{ "_id" : ObjectId("571e06a776dcd773abde06c9"), "title" : "WordPress Article" }
```

## `$group` Modifier
```
> db.tuts.find({}).pretty()
{
    "_id" : ObjectId("571e06a776dcd773abde06c7"),
    "title" : "Hello World",
    "author" : "Mark",
    "created_at" : ISODate("2016-04-25T11:59:35.742Z"),
    "votes" : 0
}
{
    "_id" : ObjectId("571e06a776dcd773abde06c8"),
    "title" : "Post Title",
    "author" : "Helen",
    "created_at" : ISODate("2016-04-25T11:59:35.742Z"),
    "votes" : 0
}
{
    "_id" : ObjectId("571e06a776dcd773abde06c9"),
    "title" : "WordPress Article",
    "author" : "Jenny",
    "created_at" : ISODate("2016-04-25T11:59:35.742Z"),
    "votes" : 0
}

> db.tuts.aggregate({'$project': {'title': '$title', 'author': '$author'}}, {'$group': {'_id': {'title': '$title', 'author': '$author'} }})
{ "_id" : { "title" : "WordPress Article", "author" : "Jenny" } }
{ "_id" : { "title" : "Post Title", "author" : "Helen" } }
{ "_id" : { "title" : "Hello World", "author" : "Mark" } }

> db.tuts.aggregate({'$project': {'title': '$title', 'author': '$author'}}, {'$group': {'_id': {'title': '$title', 'author': '$author', 'author_name': '$author'} }})
{ "_id" : { "title" : "WordPress Article", "author" : "Jenny", "author_name" : "Jenny" } }
{ "_id" : { "title" : "Post Title", "author" : "Helen", "author_name" : "Helen" } }
{ "_id" : { "title" : "Hello World", "author" : "Mark", "author_name" : "Mark" } }

> db.tuts.find({}).pretty()
{
    "_id" : ObjectId("571e06a776dcd773abde06c7"),
    "title" : "Hello World",
    "author" : "Mark",
    "created_at" : ISODate("2016-04-25T11:59:35.742Z"),
    "votes" : 0
}
{
    "_id" : ObjectId("571e06a776dcd773abde06c8"),
    "title" : "Post Title",
    "author" : "Helen",
    "created_at" : ISODate("2016-04-25T11:59:35.742Z"),
    "votes" : 0
}
{
    "_id" : ObjectId("571e06a776dcd773abde06c9"),
    "title" : "WordPress Article",
    "author" : "Jenny",
    "created_at" : ISODate("2016-04-25T11:59:35.742Z"),
    "votes" : 0
}

> db.tuts.aggregate({'$project': {'title': '$title', 'author': '$author'}}, {'$group': {'_id': {'title': '$title', 'author': '$author'} }})
{ "_id" : { "title" : "WordPress Article", "author" : "Jenny" } }
{ "_id" : { "title" : "Post Title", "author" : "Helen" } }
{ "_id" : { "title" : "Hello World", "author" : "Mark" } }

> db.tuts.aggregate({'$project': {'title': '$title', 'author': '$author'}}, {'$group': {'_id': {'title': '$title', 'author': '$author', 'author_name': '$author'} }})
{ "_id" : { "title" : "WordPress Article", "author" : "Jenny", "author_name" : "Jenny" } }
{ "_id" : { "title" : "Post Title", "author" : "Helen", "author_name" : "Helen" } }
{ "_id" : { "title" : "Hello World", "author" : "Mark", "author_name" : "Mark" } }

> db.tuts.aggregate({'$project': {'title': '$title', 'author': '$author'}}, {'$group': {'_id': '$title'}})
{ "_id" : "WordPress Article" }
{ "_id" : "Post Title" }
{ "_id" : "Hello World" }

> db.tuts.aggregate({'$project': {'title': '$title', 'author': '$author'}}, {'$group': {'_id': '$title'}})
{ "_id" : "WordPress Article" }
{ "_id" : "Post Title" }
{ "_id" : "Hello World" }

> db.tuts.aggregate({'$project': {'title': '$title', 'author': '$author'}}, {'$group': {'_id': '$title', 'num': {'$sum': 1}}})
{ "_id" : "WordPress Article", "num" : 1 }
{ "_id" : "Post Title", "num" : 1 }
{ "_id" : "Hello World", "num" : 1 }

> db.tuts.find({})
{ "_id" : ObjectId("571e06a776dcd773abde06c7"), "title" : "Hello World", "author" : "Mark", "created_at" : ISODate("2016-04-25T11:59:35.742Z"), "votes" : 0 }
{ "_id" : ObjectId("571e06a776dcd773abde06c8"), "title" : "Post Title", "author" : "Helen", "created_at" : ISODate("2016-04-25T11:59:35.742Z"), "votes" : 0 }
{ "_id" : ObjectId("571e06a776dcd773abde06c9"), "title" : "WordPress Article", "author" : "Jenny", "created_at" : ISODate("2016-04-25T11:59:35.742Z"), "votes" : 0 }

> db.tuts.insert({"title" : "Hello World", "author" : "Mark", "created_at" : new Date(), "votes" : 0 })
WriteResult({ "nInserted" : 1 })

> db.tuts.aggregate({'$project': {'title': '$title', 'author': '$author'}}, {'$group': {'_id': '$title', 'num': {'$sum': 1}}})
{ "_id" : "WordPress Article", "num" : 1 }
{ "_id" : "Post Title", "num" : 1 }
{ "_id" : "Hello World", "num" : 2 }

> db.tuts.find({}).pretty()
{
    "_id" : ObjectId("571e06a776dcd773abde06c7"),
    "title" : "Hello World",
    "author" : "Mark",
    "created_at" : ISODate("2016-04-25T11:59:35.742Z"),
    "votes" : 0
}
{
    "_id" : ObjectId("571e06a776dcd773abde06c8"),
    "title" : "Post Title",
    "author" : "Helen",
    "created_at" : ISODate("2016-04-25T11:59:35.742Z"),
    "votes" : 0
}
{
    "_id" : ObjectId("571e06a776dcd773abde06c9"),
    "title" : "WordPress Article",
    "author" : "Jenny",
    "created_at" : ISODate("2016-04-25T11:59:35.742Z"),
    "votes" : 0
}
{
    "_id" : ObjectId("571e776916393222690de28e"),
    "title" : "Hello World",
    "author" : "Mark",
    "created_at" : ISODate("2016-04-25T20:00:41.442Z"),
    "votes" : 0
}

> db.tuts.aggregate({'$project': {'title': '$title', 'author': '$author'}}, {'$group': {'_id': '$title', 'num': {'$sum': 1}}})
{ "_id" : "WordPress Article", "num" : 1 }
{ "_id" : "Post Title", "num" : 1 }
{ "_id" : "Hello World", "num" : 2 }

> db.tuts.aggregate({'$project': {'title': '$title', 'author': '$author'}}, {'$group': {'_id': '$title', 'num': {'$sum': 0}}})
{ "_id" : "WordPress Article", "num" : 0 }
{ "_id" : "Post Title", "num" : 0 }
{ "_id" : "Hello World", "num" : 0 }

> db.tuts.update({'title': 'Hello World'}, {'$inc': {'votes': 50}})
WriteResult({ "nMatched" : 1, "nUpserted" : 0, "nModified" : 1 })

> db.tuts.update({'title': 'Post Title'}, {'$inc': {'votes': 40}})
WriteResult({ "nMatched" : 1, "nUpserted" : 0, "nModified" : 1 })

> db.tuts.update({'title': 'WordPress Article'}, {'$inc': {'votes': 80}})
WriteResult({ "nMatched" : 1, "nUpserted" : 0, "nModified" : 1 })

> db.tuts.find({}).pretty()
{
    "_id" : ObjectId("571e06a776dcd773abde06c7"),
    "title" : "Hello World",
    "author" : "Mark",
    "created_at" : ISODate("2016-04-25T11:59:35.742Z"),
    "votes" : 50
}
{
    "_id" : ObjectId("571e06a776dcd773abde06c8"),
    "title" : "Post Title",
    "author" : "Helen",
    "created_at" : ISODate("2016-04-25T11:59:35.742Z"),
    "votes" : 40
}
{
    "_id" : ObjectId("571e06a776dcd773abde06c9"),
    "title" : "WordPress Article",
    "author" : "Jenny",
    "created_at" : ISODate("2016-04-25T11:59:35.742Z"),
    "votes" : 80
}
{
    "_id" : ObjectId("571e776916393222690de28e"),
    "title" : "Hello World",
    "author" : "Mark",
    "created_at" : ISODate("2016-04-25T20:00:41.442Z"),
    "votes" : 0
}

> db.tuts.aggregate({'$project': {'title': '$title', 'author': '$author'}}, {'$group': {'_id': '$title', 'max_votes': {'$max': '$votes'}}})
{ "_id" : "WordPress Article", "max_votes" : null }
{ "_id" : "Post Title", "max_votes" : null }
{ "_id" : "Hello World", "max_votes" : null }

> db.tuts.aggregate({'$project': {'title': '$title', 'author': '$author', 'votes': '$votes'}}, {'$group': {'_id': '$title', 'max_votes': {'$max': '$votes'}}})
{ "_id" : "WordPress Article", "max_votes" : 80 }
{ "_id" : "Post Title", "max_votes" : 40 }
{ "_id" : "Hello World", "max_votes" : 50 }

> db.tuts.aggregate({'$project': {'title': '$title', 'author': '$author', 'votes': '$votes'}}, {'$group': {'_id': '$title', 'max_votes': {'$min': '$votes'}}})
{ "_id" : "WordPress Article", "max_votes" : 80 }
{ "_id" : "Post Title", "max_votes" : 40 }
{ "_id" : "Hello World", "max_votes" : 0 }

> db.tuts.aggregate({'$project': {'title': '$title', 'author': '$author', 'votes': '$votes'}}, {'$group': {'_id': '$title', 'min_votes': {'$min': '$votes'}}})
{ "_id" : "WordPress Article", "min_votes" : 80 }
{ "_id" : "Post Title", "min_votes" : 40 }
{ "_id" : "Hello World", "min_votes" : 0 }

> db.tuts.aggregate({'$project': {'title': '$title', 'author': '$author', 'votes': '$votes'}}, {'$group': {'_id': '$title', 'max_votes': {'$max': '$votes'}}})
{ "_id" : "WordPress Article", "max_votes" : 80 }
{ "_id" : "Post Title", "max_votes" : 40 }
{ "_id" : "Hello World", "max_votes" : 50 }

> db.tuts.aggregate({'$project': {'title': '$title', 'author': '$author', 'votes': '$votes'}}, {'$group': {'_id': '$title', 'min_votes': {'$min': '$votes'}}})
{ "_id" : "WordPress Article", "min_votes" : 80 }
{ "_id" : "Post Title", "min_votes" : 40 }
{ "_id" : "Hello World", "min_votes" : 0 }

> db.tuts.aggregate({'$project': {'title': '$title', 'author': '$author', 'votes': '$votes'}}, {'$group': {'_id': '$title', 'first': {'$first': '$votes'}}})
{ "_id" : "WordPress Article", "first" : 80 }
{ "_id" : "Post Title", "first" : 40 }
{ "_id" : "Hello World", "first" : 50 }

> db.tuts.aggregate({'$project': {'title': '$title', 'author': '$author', 'votes': '$votes'}}, {'$group': {'_id': '$title', 'last': {'$last': '$votes'}}})
{ "_id" : "WordPress Article", "last" : 80 }
{ "_id" : "Post Title", "last" : 40 }
{ "_id" : "Hello World", "last" : 0 }

> db.tuts.aggregate({'$match': {'title': 'WordPress Article'}}, {'$project': {'comments': '$comments'}}, {'$unwind': '$comments'})
{ "_id" : ObjectId("571e06a776dcd773abde06c9"), "comments" : { "content" : "bla bla1" } }
{ "_id" : ObjectId("571e06a776dcd773abde06c9"), "comments" : { "content" : "bla bla2" } }
{ "_id" : ObjectId("571e06a776dcd773abde06c9"), "comments" : { "content" : "bla bla3" } }
{ "_id" : ObjectId("571e06a776dcd773abde06c9"), "comments" : { "content" : "bla bla4" } }

> db.tuts.aggregate({'$match': {'title': 'WordPress Article'}}, {'$project': {'comments': '$comments'}}, {'$unwind': '$comments'}, {'$skip': 1})
{ "_id" : ObjectId("571e06a776dcd773abde06c9"), "comments" : { "content" : "bla bla2" } }
{ "_id" : ObjectId("571e06a776dcd773abde06c9"), "comments" : { "content" : "bla bla3" } }
{ "_id" : ObjectId("571e06a776dcd773abde06c9"), "comments" : { "content" : "bla bla4" } }

> db.tuts.aggregate({'$match': {'title': 'WordPress Article'}}, {'$project': {'comments': '$comments'}}, {'$unwind': '$comments'}, {'$skip': 1}, {'$limit': 2})
{ "_id" : ObjectId("571e06a776dcd773abde06c9"), "comments" : { "content" : "bla bla2" } }
{ "_id" : ObjectId("571e06a776dcd773abde06c9"), "comments" : { "content" : "bla bla3" } }

> db.tuts.aggregate({'$match': {'title': 'WordPress Article'}}, {'$project': {'comments': '$comments'}}, {'$unwind': '$comments'}, {'$skip': 1}, {'$limit': 2}, {'$sort':{'comments.content': 1}})
{ "_id" : ObjectId("571e06a776dcd773abde06c9"), "comments" : { "content" : "bla bla2" } }
{ "_id" : ObjectId("571e06a776dcd773abde06c9"), "comments" : { "content" : "bla bla3" } }

> db.tuts.aggregate({'$match': {'title': 'WordPress Article'}}, {'$project': {'comments': '$comments'}}, {'$unwind': '$comments'}, {'$skip': 1}, {'$limit': 2}, {'$sort':{'comments.content': -1}})
{ "_id" : ObjectId("571e06a776dcd773abde06c9"), "comments" : { "content" : "bla bla3" } }
{ "_id" : ObjectId("571e06a776dcd773abde06c9"), "comments" : { "content" : "bla bla2" } }
```

## MapReduce
```
db.orders.insert([{'cust_id': 'A123', 'amount': 500, 'status': 'A'},{'cust_id': 'A124', 'amount': 600, 'status': 'B'},{'cust_id': 'A125', 'amount': 700, 'status': 'C'}])
db.orders.mapReduce(function(){ emit(this.cust_id, this.amount); }, function(key, values){ return Array.sum(values); },{ 'query': {'status':'A'}, 'out': 'order_totals' })
```
