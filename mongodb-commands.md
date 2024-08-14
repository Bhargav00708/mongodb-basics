# MongoDB Commands

### Show All Databases

```js
show dbs
```

### Show Current Database

```js
db
```

### Create Or Switch Database

```js
use acme
```

### Drop

```js
db.dropDatabase()
```

### Create Collection

```js
db.createCollection('posts')
```

### Show Collections

```js
show collections
```

### Insert Row

```js
db.posts.insertOne({
  title: 'Post One',
  body: 'Body of post one',
  category: 'News',
  tags: ['news', 'events'],
  user: {
    name: 'John Doe',
    status: 'author'
  },
  date: Date()
})
```

### Insert Multiple Rows

```js
db.posts.insertMany([
  {
    title: 'Post Two',
    body: 'Body of post two',
    category: 'Technology',
    date: Date()
  },
  {
    title: 'Post Three',
    body: 'Body of post three',
    category: 'News',
    date: Date()
  },
  {
    title: 'Post Four',
    body: 'Body of post three',
    category: 'Entertainment',
    date: Date()
  }
])
```

### Get All Rows

```js
db.posts.find()
db.posts.findOne()
```

### Get All Rows Formatted

```js
db.find().pretty()
```

### Find Rows

```js
db.posts.find({ category: 'News' })
```

### Sort Rows

```js
# asc
db.posts.find().sort({ title: 1 }).pretty()
in Char it will sort according to it
# desc
db.posts.find().sort({ title: -1 }).pretty()
```

### Count Rows

```js
db.posts.find().count()
db.posts.find({ category: 'news' }).count()
```

### Limit Rows

```js
db.posts.find().limit(2).pretty()
```

### Chaining

```js
db.posts.find().limit(2).sort({ title: 1 }).pretty()
We can Also chain in any order
```

### Foreach

```js
db.posts.find().forEach(function(doc) {
  print("Blog Post: " + doc.title)
})
```

### Find One Row

```js
db.posts.findOne({ category: 'News' })
```

### Find Specific Fields

```js
db.posts.find({ title: 'Post One' }, {
  title: 1,
  author: 1
})
In the MongoDB query language, the second argument of the `find()` method is used to specify which fields of the matched documents should be included or excluded from the result set. 

If you pass a field name with a value of `1`, it means that the field should be included in the result set. In your example, the query will return all documents where the `title` field is 'Post One', and it will include the `title` and `author` fields in the result set. 

On the other hand, if you pass a field name with a value of `0`, it means that the field should be excluded from the result set. In other words, it will not be returned in the query result. 

For example, if we modify the query to `db.posts.find({ title: 'Post One' }, { _id: 0, content: 0 })`, it will return all documents where the `title` field is 'Post One', but it will exclude the `_id` and `content` fields from the result set. 

If you pass any value other than `0` or `1`, MongoDB will treat it as a `1` and include that field in the result set. This is called "projection" in MongoDB and it allows you to control the shape of the data returned from the query.
```

### Update Row

```js
Now it is generally we should use updateOne and updateMany for better control and clarity

db.posts.update({ title: 'Post Two' },
{
  title: 'Post Two',
  body: 'New body for post 2',
  date: Date()
},
{
  upsert: true
})
```

### Update Specific Field

```js
db.posts.update({ title: 'Post Two' },
{
  $set: {
    body: 'Body for post 2',
    category: 'Technology'
  }
})
```

### Increment Field (\$inc)

```js
db.posts.update({ title: 'Post Two' },
{
  $inc: {
    likes: 5
  }
})
```

### Rename Field

```js
db.posts.update({ title: 'Post Two' },
{
  $rename: {
    likes: 'views'
  }
})
```

### Delete Row

```js
We can also use deleteOne and deleteMany for better control and clarity
db.posts.remove({ title: 'Post Four' })
```

### Sub-Documents

```js
db.posts.update({ title: 'Post One' },
{
  $set: {
    comments: [
      {
        body: 'Comment One',
        user: 'Mary Williams',
        date: Date()
      },
      {
        body: 'Comment Two',
        user: 'Harry White',
        date: Date()
      }
    ]
  }
})
```

### Find By Element in Array (\$elemMatch)

```js
db.posts.find({
  comments: {
     $elemMatch: {
       user: 'Mary Williams'
       }
    }
  }
)
```

### Add Index

```js
db.posts.createIndex({ title: 'text' })
```

### Text Search

```js
db.posts.find({
  $text: {
    $search: "\"Post O\""
    }
})
```
We can't use text or search keyword without creating indexing
In mongodb if you want to search text then you have to create index

### Greater & Less Than

```js
db.posts.find({ views: { $gt: 2 } })
db.posts.find({ views: { $gte: 7 } })
db.posts.find({ views: { $lt: 7 } })
db.posts.find({ views: { $lte: 7 } })
```

### List of the indexes in a collection

```js
db.restaurants.getIndexes()
[
	{
		"v" : 2,
		"key" : {
			"_id" : 1
		},
		"name" : "_id_",
		"ns" : "test.restaurants"
	},
	{
		"v" : 2,
		"key" : {
			"borough" : 1
		},
		"name" : "borough_1",
		"ns" : "test.restaurants"
	}
]
```

### Drop an existing index

```js
db.restaurants.dropIndex("cuisine_1_grades.score_1")
{ "nIndexesWas" : 4, "ok" : 1 }
```

### CreateView
Syntax
```js
db.createView(<view>, <source>, <pipeline>, <options>)
```

Example
```js
db.createView('postsTitles', 'posts',[{$project:{title:1}}]);
{ "ok" : 1 }


db.postsTitles.find();
```




<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>

