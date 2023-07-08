# MongoDB Coding Practice

## Q. ***Mention the command to insert a document in a database called company and collection called employee?***

```js
use company;
db.employee.insert( { name: "John", email: "john.k@gmail.com" } )
```

<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>

## Q. ***Mention the command to check whether you are on the master server or not?***

```js
db.isMaster()
```

<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>

## Q. ***How to combine data from multiple collections into one collection?***

**$lookup:**

Performs a left outer join to an unsharded collection in the same database to filter in documents from the “joined” collection for processing. To each input document, the `$lookup` stage adds a new array field whose elements are the matching documents from the “joined” collection. The `$lookup` stage passes these reshaped documents to the next stage.

**Syntax:**

```js
{
   $lookup:
     {
       from: <collection to join>,
       localField: <field from the input documents>,
       foreignField: <field from the documents of the "from" collection>,
       as: <output array field>
     }
}
```

<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>

## Q. ***How do I perform the SQL JOIN equivalent in MongoDB?***

```js
// Sample Records

comments
  { uid:12345, pid:444, comment="blah" }
  { uid:12345, pid:888, comment="asdf" }
  { uid:99999, pid:444, comment="qwer" }

users
  { uid:12345, name:"john" }
  { uid:99999, name:"mia"  }
```

**Answers**

```js
{
   $lookup:
     {
       from: <collection to join>,
       localField: <field from the input documents>,
       foreignField: <field from the documents of the "from" collection>,
       as: <output array field>
     }
}
```

## Q. ***How to query MongoDB with "like"?***

```js
db.users.insert({name: 'paulo'})
db.users.insert({name: 'patric'})
db.users.insert({name: 'pedro'})

db.users.find({name: /a/})  //like '%a%'
db.users.find({name: /^pa/}) //like 'pa%'
db.users.find({name: /ro$/}) //like '%ro'
```

<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>

## Q. ***Find objects between two dates MongoDB?***

Operator `$gte` and `$lt` is used to find objects between two dates in MongoDB.

**Example**: Creating a collection

```js
>db.order.insert({"OrderId":1,"OrderAddrees":"US","OrderDateTime":ISODate("2020-02-19")};
WriteResult({ "nInserted" : 1 })

>db.order.insert({"OrderId":2,"OrderAddrees":"UK","OrderDateTime":ISODate("2020-02-26")};
WriteResult({ "nInserted" : 1 })
```

Display all documents from the collection using `find()` method.

```js
> db.order.find().pretty();

// Output
{
   "_id" : ObjectId("5c6c072068174aae23f5ef57"),
   "OrderId" : 1,
   "OrderAddrees" : "US",
   "OrderDateTime" : ISODate("2020-02-19T00:00:00Z")
}
{
   "_id" : ObjectId("5c6c073568174aae23f5ef58"),
   "OrderId" : 2,
   "OrderAddrees" : "UK",
   "OrderDateTime" : ISODate("2020-02-26T00:00:00Z")
}
```

Here is the query to find objects between two dates:

```js
> db.order.find({"OrderDateTime":{ $gte:ISODate("2020-02-10"), $lt:ISODate("2020-02-21") }
}).pretty();


// Output
{
   "_id" : ObjectId("5c6c072068174aae23f5ef57"),
   "OrderId" : 1,
   "OrderAddrees" : "US",
   "OrderDateTime" : ISODate("2020-02-19T00:00:00Z")
}
```

<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>

## Q. ***Is it possible to update MongoDB field using value of another field?***

The aggregate function can be used to update MongoDB field using the value of another field.

**Example**

```js
db.collection.<update method>(
    {},
    [
        {"$set": {"name": { "$concat": ["$firstName", " ", "$lastName"]}}}
    ]
)
```

<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>

## Q. ***How to check if a field contains a substring?***

The `$regex` operator can be used to check if a field contains a string in MongoDB.

```js
db.users.findOne({"username" : {$regex : ".*some_string.*"}});
```

<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>

## Q. ***How to get the last N records from find?***

```js
// Syntax
db.<CollectionName>.find().sort({$natural:-1}).limit(value)


// Example
db.employee.find().sort({$natural:-1}).limit(100)
```

<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>

## Q. ***How to remove a field completely from a MongoDB document?***

How do I remove words completely from all the documents in this collection?

```js
{ 
    name: 'book',
    tags: {
        words: ['abc','123'], // <-- remove it comletely
        lat: 33,
        long: 22
    }
}
```

**Answer**

```js
db.example.update({}, {$unset: {words: 1}}, false, true);
```

In the MongoDB update command you provided, the "false" and "true" values correspond to the options for the `multi` and `upsert` parameters, respectively. Here's what they mean:

```javascript
db.example.update({}, {$unset: {words: 1}}, false, true);
```

- The first parameter `{}` is the query filter, which in this case is an empty object `{}`. This means the update operation will be applied to all documents in the collection.

- The second parameter `{$unset: {words: 1}}` is the update operation itself. In this case, it uses the `$unset` operator to remove the "words" field from the document.

- The third parameter `false` is the `multi` option. When set to `false`, it specifies that only the first document matching the query filter will be updated. In this case, since the query filter is an empty object `{}`, all documents in the collection will be matched, and only the first document encountered will be updated.

- The fourth parameter `true` is the `upsert` option. When set to `true`, it indicates that if no documents match the query filter, a new document will be inserted based on the update operation. In this case, since the query filter is an empty object `{}`, it will not create a new document because all existing documents in the collection will be matched.

So, in summary, the command you provided will unset the "words" field from the first document encountered in the "example" collection, and it will not create a new document if no matching documents are found.

<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>

## Q. ***How to find MongoDB records where array field is not empty?***

```js
db.inventory.find({ pictures: { $exists: true, $ne: [] } })
```

<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>

## Q. ***How to find document with array that contains a specific value?***

Populate the inventory collection

```js
db.inventory.insertMany([
   { item: "journal", qty: 25, tags: ["blank", "red"], dim_cm: [ 14, 21 ] },
   { item: "notebook", qty: 50, tags: ["red", "blank"], dim_cm: [ 14, 21 ] },
   { item: "paper", qty: 100, tags: ["red", "blank", "plain"], dim_cm: [ 14, 21 ] },
   { item: "planner", qty: 75, tags: ["blank", "red"], dim_cm: [ 22.85, 30 ] },
   { item: "postcard", qty: 45, tags: ["blue"], dim_cm: [ 10, 15.25 ] }
]);
```

To query if the array field contains at least one element with the specified value, use the filter { `<field>`: `<value>` } where `<value>` is the element value.

```js
db.inventory.find( { tags: "red" } )
```

<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>

## Q. ***Mention the command to remove indexes and list all the indexes on a particular collection?***

**List all Indexes on a Collection**

```js
// To view all indexes on the people collection

db.people.getIndexes()
```

**List all Indexes for a Database**

```js
// To list all the collection indexes in a database

db.getCollectionNames().forEach(function(collection) {
   indexes = db[collection].getIndexes();
   print("Indexes for " + collection + ":");
   printjson(indexes);
});
```

**Remove Indexes**

MongoDB provides two methods for removing indexes from a collection:

* `db.collection.dropIndex()`
* `db.collection.dropIndexes()`

**1. Remove Specific Index**

```js
db.accounts.dropIndex( { "tax-id": 1 } )


// Output
{ "nIndexesWas" : 3, "ok" : 1 }
```

**2. Remove All Indexes**

```js
// The following command removes all indexes from the accounts collection

db.accounts.dropIndexes()
```

<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>

## Q. ***Select only selected elements from given collection?***

Consider a ```books``` collection with the following document:

```js
{
  "_id" : 1,
  title: "abc123",
  isbn: "0001122223334",
  author: { last: "zzz", first: "aaa" },
  copies: 5
}
```

The following ```$project``` stage adds the new fields isbn, lastName, and copiesSold:

```js
db.books.aggregate([
{
    $project : {
        title:1,
        isbn: {
               prefix: { $substr: [ "$isbn", 0, 3 ] },
               group: { $substr: [ "$isbn", 3, 2 ] },
               publisher: { $substr: [ "$isbn", 5, 4 ] },
               title: { $substr: [ "$isbn", 9, 3 ] },
               checkDigit: { $substr: [ "$isbn", 12, 1] }
            },
         lastName: "$author.last",
        copiesSold: "$copies"
    }
}
])
```

The operation results in the following document:

```js
{
   "_id" : 1,
   "title" : "abc123",
   "isbn" : {
      "prefix" : "000",
      "group" : "11",
      "publisher" : "2222",
      "title" : "333",
      "checkDigit" : "4"
   },
   "lastName" : "zzz",
   "copiesSold" : 5
}
```

<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>

## Q. ***How to join 3 or more collections in MongoDB?***

Let's say we have 3 hypothetical collections in MongoDB: customers, orders, and orderItems.

Each customer has multiple orders, and each order has multiple order items.

**Example:**

```js
// customers
[
    {
        customer_id: 1,
        name: "Jim Smith",
        email: "jim.smith@example.com"
    },
    {
        customer_id: 2,
        name: "Bob Jones",
        email: "bob.jones@example.com"
    }
]


// orders
[
    {
        order_id: 1,
        customer_id: 1
    },
    {
        order_id: 2,
        customer_id: 1
    }
]


// orderItems
[
    {
        order_item_id: 1,
        name: "Foo",
        price: 4.99,
        order_id: 1
    },
    {
        order_item_id: 2,
        name: "Bar",
        price: 17.99,
        order_id: 1
    },
    {
        order_item_id: 3,
        name: "baz",
        price: 24.99,
        order_id: 2
    }
]
```

**Desired Result:**

```js
[
    {
        customer_id: 1,
        name: "Jim Smith",
        email: "jim.smith@example.com"
        orders: [
            {
                order_id: 1,
                items: [
                    {
                        name: "Foo",
                        price: 4.99
                    },
                    {
                        name: "Bar",
                        price: 17.99
                    }
                ]
            },
            {
                order_id: 2,
                items: [
                    {
                        name: "baz",
                        price: 24.99
                    }
                ]
            }
        ]
    },
    {
        customer_id: 2,
        name: "Bob Jones",
        email: "bob.jones@example.com"
        orders: []
    }
]
```

### Answer

Do nested lookup using lookup with pipeline,

1. ```$lookup``` with orders collection.
2. ```let```, define variable customer_id that is from main collection, to access this reference variable inside pipeline using ```$$``` like ```$$customer_id```.
3. ```pipeline``` can add pipeline stages same as we do in root level pipeline
4. ```$expr``` whenever we match internal fields it requires expression match condition, so ```$$customer_id``` is parent collection field that declared in let and $customer_id is child collection's/current collection's field
5. ```$lookup``` with orderitems collection

```js
db.customers.aggregate([
  {
    $lookup: {
      from: "orders",
      let: { customer_id: "$customer_id" },
      pipeline: [
        { $match: { $expr: { $eq: ["$$customer_id", "$customer_id"] } } },
        {
          $lookup: {
            from: "orderitems",
            localField: "order_id",
            foreignField: "order_id",
            as: "items"
          }
        }
      ],
      as: "orders"
    }
  }
])
```

<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>

## Q. ***How to validate data in mongodb?***

`db.collection.validate(<documents>)` validates a collection. The method scans a collection data and indexes for correctness and returns the result.

**Syntax:**

```js
db.collection.validate( {
   full: <boolean>,          // Optional
   repair: <boolean>         // Optional, added in MongoDB 5.0
} )
```

**Example:**

```js
// validate a collection using the default validation setting
db.myCollection.validate({ })

// perform a full validation of collection
db.myCollection.validate( { full: true } )

// repair collection
db.myCollection.validate( { repair: true } )
```

<div align="right">
    <b><a href="#">↥ back to top</a></b>
</div>


**Group Related Example:**
Certainly! Here are a few more examples of how you can use the `$group` operator in MongoDB's aggregation framework:

Example 1: Grouping by a Field and Calculating Average
Suppose you have a collection named "sales" with documents representing sales transactions, including a "product" field and a "quantity" field. You can group the documents by the "product" field and calculate the average quantity sold for each product using the `$group` stage:

```javascript
db.sales.aggregate([
  {
    $group: {
      _id: "$product",
      averageQuantity: { $avg: "$quantity" }
    }
  }
])
```

This will group the documents by the "product" field and calculate the average quantity using the `$avg` operator.

Example 2: Grouping by Multiple Fields and Summing Values
Let's say you have a collection named "orders" with documents representing customer orders, including fields like "product", "category", and "quantity". You can group the documents by multiple fields and calculate the total quantity for each combination of "product" and "category" using the `$group` stage:

```javascript
db.orders.aggregate([
  {
    $group: {
      _id: { product: "$product", category: "$category" },
      totalQuantity: { $sum: "$quantity" }
    }
  }
])
```

This will group the documents by the combination of "product" and "category" fields and calculate the total quantity using the `$sum` operator.

Example 3: Grouping and Accumulating Array Values
Suppose you have a collection named "employees" with documents representing employees, including a "department" field and an "employees" field that contains an array of employee names. You can group the documents by the "department" field and accumulate all the employee names into an array for each department using the `$group` stage:

```javascript
db.employees.aggregate([
  {
    $group: {
      _id: "$department",
      employees: { $push: "$name" }
    }
  }
])
```

This will group the documents by the "department" field and use the `$push` operator to accumulate all the employee names into the "employees" array for each department.

These examples demonstrate some common use cases of the `$group` operator in MongoDB's aggregation framework. You can explore more aggregation operators and stages to perform various calculations, transformations, and aggregations based on your specific requirements.


It's Input data

Certainly! Here are some input data examples and their corresponding results based on the queries provided:

Example 1: Grouping by a Field and Calculating Average
Input data:
```
[
  { "product": "A", "quantity": 10 },
  { "product": "B", "quantity": 5 },
  { "product": "A", "quantity": 8 },
  { "product": "B", "quantity": 12 },
  { "product": "A", "quantity": 6 }
]
```

Query result:
```
[
  { "_id": "A", "averageQuantity": 8 },
  { "_id": "B", "averageQuantity": 8.5 }
]
```

Example 2: Grouping by Multiple Fields and Summing Values
Input data:
```
[
  { "product": "A", "category": "X", "quantity": 10 },
  { "product": "A", "category": "Y", "quantity": 5 },
  { "product": "B", "category": "X", "quantity": 8 },
  { "product": "B", "category": "Y", "quantity": 12 },
  { "product": "A", "category": "X", "quantity": 6 }
]
```

Query result:
```
[
  { "_id": { "product": "A", "category": "X" }, "totalQuantity": 16 },
  { "_id": { "product": "A", "category": "Y" }, "totalQuantity": 5 },
  { "_id": { "product": "B", "category": "X" }, "totalQuantity": 8 },
  { "_id": { "product": "B", "category": "Y" }, "totalQuantity": 12 }
]
```

Example 3: Grouping and Accumulating Array Values
Input data:
```
[
  { "department": "Sales", "name": "John Doe" },
  { "department": "Sales", "name": "Jane Smith" },
  { "department": "Marketing", "name": "David Johnson" },
  { "department": "Marketing", "name": "Emily Davis" }
]
```

Query result:
```
[
  { "_id": "Sales", "employees": [ "John Doe", "Jane Smith" ] },
  { "_id": "Marketing", "employees": [ "David Johnson", "Emily Davis" ] }
]
```

These examples illustrate the expected results when applying the corresponding queries to the given input data.


--Salary related Query and it's data 

Certainly! Here's an example of input data and the corresponding result for finding the highest salary using the aggregation query:

Input data:
```
[
  { "employeeId": 1, "name": "John Doe", "salary": 5000 },
  { "employeeId": 2, "name": "Jane Smith", "salary": 6000 },
  { "employeeId": 3, "name": "Bob Johnson", "salary": 4500 },
  { "employeeId": 4, "name": "Alice Williams", "salary": 7000 },
  { "employeeId": 5, "name": "David Davis", "salary": 5500 }
]
```
Query
```
db.employees.aggregate([
  {
    $group: {
      _id: null,
      highestSalary: { $max: "$salary" }
    }
  }
])
```

Query result:
```
[
  { "_id": null, "highestSalary": 7000 }
]
```

In this example, the input data consists of employee documents with fields such as "employeeId", "name", and "salary". The aggregation query is used to find the highest salary among all employees.

The result of the query is a single document with "_id" set to `null` since we did not group by any specific field. The "highestSalary" field contains the value 7000, which represents the highest salary among the employees.

Please note that the input data provided here is for demonstration purposes. You can modify the data or add more employee documents with different salary values to see the variations in the results based on the highest salary in the dataset.

Group Array Data

If your data is in an array and you want to group them using the MongoDB aggregation framework, you can utilize the `$unwind` operator to deconstruct the array and create separate documents for each element. Then you can proceed with the `$group` stage to perform grouping operations.

Here's an example to illustrate the process:

Input data:
```
[
  {
    "department": "Sales",
    "employees": [
      { "name": "John Doe", "salary": 5000 },
      { "name": "Jane Smith", "salary": 6000 }
    ]
  },
  {
    "department": "Marketing",
    "employees": [
      { "name": "Bob Johnson", "salary": 4500 },
      { "name": "Alice Williams", "salary": 7000 },
      { "name": "David Davis", "salary": 5500 }
    ]
  }
]
```

Query:
```javascript
db.departments.aggregate([
  { $unwind: "$employees" },
  {
    $group: {
      _id: "$department",
      highestSalary: { $max: "$employees.salary" }
    }
  }
])
```

Result:
```
[
  { "_id": "Sales", "highestSalary": 6000 },
  { "_id": "Marketing", "highestSalary": 7000 }
]
```

In this example, the input data represents departments with an "employees" field containing an array of employee documents. The aggregation query begins by using the `$unwind` operator to deconstruct the "employees" array and create separate documents for each employee.

Then, the `$group` stage groups the documents by the "department" field and calculates the maximum salary using the `$max` operator on the "employees.salary" field.

The result of the query provides the highest salary for each department, based on the employee data within the array.

Note that the collection name used in the query is assumed to be "departments". Modify it according to your actual collection name.


Finding data logic

Certainly! Here's an example of input data with an array within an array:

```javascript
[
  {
    "departments": [
      {
        "name": "Sales",
        "employees": [
          { "name": "John Doe", "salary": 5000 },
          { "name": "Jane Smith", "salary": 6000 }
        ]
      },
      {
        "name": "Marketing",
        "employees": [
          { "name": "Bob Johnson", "salary": 4500 },
          { "name": "Alice Williams", "salary": 7000 },
          { "name": "David Davis", "salary": 5500 }
        ]
      }
    ]
  }
]
```

In this example, the input data represents a collection named "employees" with documents that contain an array field called "departments". Each "departments" array element represents a department and has a "name" field and an "employees" array field.

Within each department's "employees" array, there are objects representing individual employees, with fields like "name" and "salary".

You can use the provided input data to test queries and access data within the array and object structures.

Query
```
db.employees.find({ "departments.0.employees.0.name": "John Doe" })
```


Certainly! Here's an example of input data with nested arrays, along with a query to access data inside the nested array:

Input data:
```javascript
[
  {
    "students": [
      {
        "name": "John",
        "grades": [
          { "subject": "Math", "score": 90 },
          { "subject": "Science", "score": 85 }
        ]
      },
      {
        "name": "Jane",
        "grades": [
          { "subject": "Math", "score": 95 },
          { "subject": "Science", "score": 92 }
        ]
      }
    ]
  }
]
```

Query to access data inside the nested array:
```javascript
db.students.find({ "students.name": "John", "students.grades.subject": "Math" })
```

This query will find the document where the student's name is "John" and the subject of one of their grades is "Math". It will return the following result:

```javascript
[
  {
    "students": [
      {
        "name": "John",
        "grades": [
          { "subject": "Math", "score": 90 },
          { "subject": "Science", "score": 85 }
        ]
      }
    ]
  }
]
```

In this example, the input data represents a collection named "students" with documents that contain an array field called "students". Each "students" array element represents a student and has a "name" field and a nested array field called "grades".

Within each student's "grades" array, there are objects representing individual grades, with fields like "subject" and "score".

The provided query finds the document where one of the students has the name "John" and has a grade with the subject "Math". It returns the document with the matching student and their associated grades.

Feel free to modify the input data and query according to your specific requirements to explore different scenarios.


Or Operation In it 

If you want to perform an "OR" operation in your query to match documents based on multiple conditions, you can use the `$or` operator in MongoDB. The `$or` operator allows you to specify multiple conditions, and if any of the conditions are true, the document will be considered a match.

Here's an example of using the `$or` operator to perform an "OR" operation in your query:

Input data:
```javascript
[
  {
    "students": [
      {
        "name": "John",
        "grades": [
          { "subject": "Math", "score": 90 },
          { "subject": "Science", "score": 85 }
        ]
      },
      {
        "name": "Jane",
        "grades": [
          { "subject": "Math", "score": 95 },
          { "subject": "Science", "score": 92 }
        ]
      }
    ]
  }
]
```

Query to perform an "OR" operation:
```javascript
db.students.find({
  $or: [
    { "students.name": "John" },
    { "students.grades.subject": "Math" }
  ]
})
```

In this example, the query will find the documents where either the student's name is "John" or the subject of one of their grades is "Math". It will return the following result:

```javascript
[
  {
    "students": [
      {
        "name": "John",
        "grades": [
          { "subject": "Math", "score": 90 },
          { "subject": "Science", "score": 85 }
        ]
      },
      {
        "name": "Jane",
        "grades": [
          { "subject": "Math", "score": 95 },
          { "subject": "Science", "score": 92 }
        ]
      }
    ]
  }
]
```

In the query, the `$or` operator is used to specify multiple conditions. If any of the conditions are true, the document will be considered a match. In this case, if either the "students.name" field is "John" or the "students.grades.subject" field is "Math", the document will be returned.

Feel free to modify the conditions and add more conditions inside the `$or` operator as per your requirements to perform complex "OR" operations in your queries.
