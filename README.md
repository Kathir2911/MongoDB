# MongoDB
Mongodb learning
# MongoDB Database Operations Tutorial

# MongoDB Operations Examples

This guide covers essential MongoDB operations with practical examples. Each section includes command usage with sample outputs.

## Table of Contents
- [Database and Collection Setup](#database-and-collection-setup)
- [Basic CRUD Operations](#basic-crud-operations)
- [Query Operations](#query-operations)
- [Update Operations](#update-operations)
- [Deletion Operations](#deletion-operations)
- [Search and Filtering](#search-and-filtering)
- [Aggregation Framework](#aggregation-framework)
- [Import and Export Operations](#import-and-export-operations)

## Database and Collection Setup

### Creating a Database
```javascript
test> use Exercise-1;
switched to db Exercise-1
```

### Creating Collections
```javascript
Exercise-1> db.createCollection("Users");
{ ok: 1 }
Exercise-1> db.createCollection("Products");
{ ok: 1 }
Exercise-1> db.createCollection("Orders");
{ ok: 1 }
Exercise-1> db.createCollection("Payments");
{ ok: 1 }
```

## Basic CRUD Operations

### Inserting Documents
```javascript
db.Users.insertMany([
  { "u_id": 1, "name": "Kathir", "city": "Madurai" },
  { "u_id": 2, "name": "Jeeva", "city": "Paramakudi" },
  { "u_id": 3, "name": "Arun", "city": "Chennai" },
  { "u_id": 4, "name": "Mohan", "city": "Coimbatore" },
  { "u_id": 5, "name": "Ravi", "city": "Trichy" }
]);

db.Products.insertMany([
  { "p_id": 101, "name": "Pen", "price": 20, "category": "Stationery", "stock": 25 },
  { "p_id": 102, "name": "Headphone", "price": 500, "category": "Electronics", "stock": 10 },
  { "p_id": 103, "name": "Carrot", "price": 25, "category": "Vegetables", "stock": 100 },
  { "p_id": 104, "name": "Notebook", "price": 50, "category": "Stationery", "stock": 30 },
  { "p_id": 105, "name": "Mouse", "price": 400, "category": "Electronics", "stock": 15 }
]);

db.Orders.insertMany([
  { "o_id": 1000, "u_id": 2, "p_id": 101, "qty": 3, "status": "Pending" },
  { "o_id": 1005, "u_id": 2, "p_id": 103, "qty": 25, "status": "Delivered" },
  { "o_id": 1010, "u_id": 1, "p_id": 102, "qty": 1, "status": "Pending" },
  { "o_id": 1015, "u_id": 3, "p_id": 101, "qty": 2, "status": "Shipped" },
  { "o_id": 1020, "u_id": 5, "p_id": 104, "qty": 4, "status": "Pending" }
]);

db.Payments.insertMany([
  { "pay_id": 29109, "o_id": 1000, "amount": 1000, "status": "Paid", "method": "UPI" },
  { "pay_id": 34522, "o_id": 1005, "amount": 625, "status": "Paid", "method": "Net-banking" },
  { "pay_id": 89231, "o_id": 1010, "amount": 345, "status": "Not-Paid", "method": "Cash" },
  { "pay_id": 56218, "o_id": 1015, "amount": 40, "status": "Paid", "method": "UPI" },
  { "pay_id": 90743, "o_id": 1020, "amount": 200, "status": "Pending", "method": "Card" }
]);
```

### Reading Documents
```javascript
// View all documents in a collection
Exercise-1> db.Users.find();
[
  {
    _id: ObjectId('67ee0db1d5b35e1b0e6b140b'),
    u_id: 1,
    name: 'Kathir',
    city: 'Madurai'
  },
  {
    _id: ObjectId('67ee0db1d5b35e1b0e6b140c'),
    u_id: 2,
    name: 'Jeeva',
    city: 'Paramakudi'
  },
  {
    _id: ObjectId('67ee0db1d5b35e1b0e6b140d'),
    u_id: 3,
    name: 'Arun',
    city: 'Chennai'
  },
  {
    _id: ObjectId('67ee0db1d5b35e1b0e6b140e'),
    u_id: 4,
    name: 'Mohan',
    city: 'Coimbatore'
  },
  {
    _id: ObjectId('67ee0db1d5b35e1b0e6b140f'),
    u_id: 5,
    name: 'Ravi',
    city: 'Trichy'
  }
]
```

### Finding Specific Documents
```javascript
// Find by name
Exercise-1> db.Users.find({name:"Kathir"});
[
  {
    _id: ObjectId('67ee0db1d5b35e1b0e6b140b'),
    u_id: 1,
    name: 'Kathir',
    city: 'Madurai'
  }
]

// Find by user ID
Exercise-1> db.Orders.find({u_id:2});
[
  {
    _id: ObjectId('67ee0f9ad5b35e1b0e6b1410'),
    o_id: 1000,
    u_id: 2,
    p_id: 101,
    qty: 3,
    status: 'Pending'
  },
  {
    _id: ObjectId('67ee0f9ad5b35e1b0e6b1411'),
    o_id: 1005,
    u_id: 2,
    p_id: 103,
    qty: 25,
    status: 'Delivered'
  }
]
```

## Update Operations

### Updating a Single Document
```javascript
// Update order status
Exercise-1> db.Orders.updateOne({ o_id: 1000 }, { $set: { status: "Delivered" } });
{
  acknowledged: true,
  insertedId: null,
  matchedCount: 1,
  modifiedCount: 1,
  upsertedCount: 0
}

// Verify update
Exercise-1> db.Orders.find({o_id:1000});
[
  {
    _id: ObjectId('67ee0f9ad5b35e1b0e6b1410'),
    o_id: 1000,
    u_id: 2,
    p_id: 101,
    qty: 3,
    status: 'Delivered'
  }
]
```

### Updating with Increment Operation
```javascript
// Decrease stock after purchase
Exercise-1> db.Products.updateOne({p_id:103},{$inc: {stock: -25}});
{
  acknowledged: true,
  insertedId: null,
  matchedCount: 1,
  modifiedCount: 1,
  upsertedCount: 0
}

// Verify stock decrease
Exercise-1> db.Products.find({p_id:103});
[
  {
    _id: ObjectId('67ee0e92d5b35e1b0e6b140f'),
    p_id: 103,
    name: 'Carrot',
    price: 25,
    category: 'Vegetables',
    stock: 75
  }
]
```

### Updating Multiple Documents
```javascript
// Increase all product prices by 100
Exercise-1> db.Products.updateMany({},{$inc:{price:100}});
{
  acknowledged: true,
  insertedId: null,
  matchedCount: 5,
  modifiedCount: 5,
  upsertedCount: 0
}
```

### Replacing an Entire Document
```javascript
// Replace document with new fields
Exercise-1> db.Users.replaceOne({name:"Kathir"},{name:"Kathir",city:"Paramakudi",age:34});
{
  acknowledged: true,
  insertedId: null,
  matchedCount: 1,
  modifiedCount: 1,
  upsertedCount: 0
}

// Verify replacement
Exercise-1> db.Users.find({name:"Kathir"});
[
  {
    _id: ObjectId('67ee0db1d5b35e1b0e6b140b'),
    name: 'Kathir',
    city: 'Paramakudi',
    age: 34
  }
]
```

## Deletion Operations

### Deleting a Single Document
```javascript
// Delete a user
db.Users.deleteOne({u_id:1});

// Delete an order
db.Orders.deleteOne({o_id: 1005});
```

### Deleting Multiple Documents
```javascript
// Delete products with price less than 25
Exercise-1> db.Products.deleteMany({price:{$lt:25}});
{ acknowledged: true, deletedCount: 0 }

// Delete all documents in a collection
db.users.deleteMany({})
```

## Search and Filtering

### Status-Based Filtering
```javascript
// Find orders with "Pending" status
Exercise-1> db.Orders.find({status:"Pending"});
[
  {
    _id: ObjectId('67ee0f9ad5b35e1b0e6b1412'),
    o_id: 1010,
    u_id: 1,
    p_id: 2,
    qty: 1,
    status: 'Pending'
  },
  {
    _id: ObjectId('67ee0f9ad5b35e1b0e6b1414'),
    o_id: 1020,
    u_id: 5,
    p_id: 104,
    qty: 4,
    status: 'Pending'
  }
]
```

### Comparison Operators
```javascript
// Find products with price >= 25
Exercise-1> db.Products.find({price:{$gte:25}});
[
  {
    _id: ObjectId('67ee0e92d5b35e1b0e6b140e'),
    p_id: 102,
    name: 'Headphone',
    price: 500,
    category: 'Electronics',
    stock: 10
  },
  {
    _id: ObjectId('67ee0e92d5b35e1b0e6b140f'),
    p_id: 103,
    name: 'Carrot',
    price: 25,
    category: 'Vegetables',
    stock: 75
  },
  {
    _id: ObjectId('67ee0e92d5b35e1b0e6b1410'),
    p_id: 104,
    name: 'Notebook',
    price: 50,
    category: 'Stationery',
    stock: 30
  },
  {
    _id: ObjectId('67ee0e92d5b35e1b0e6b1411'),
    p_id: 105,
    name: 'Mouse',
    price: 400,
    category: 'Electronics',
    stock: 15
  }
]

// Find products with price between 121 and 500
Exercise-1> db.Products.find({ price: { $gte: 121, $lte: 500 } });
[
  {
    _id: ObjectId('67ee0e92d5b35e1b0e6b140f'),
    p_id: 103,
    name: 'Carrot',
    price: 125,
    category: 'Vegetables',
    stock: 75
  },
  {
    _id: ObjectId('67ee0e92d5b35e1b0e6b1410'),
    p_id: 104,
    name: 'Notebook',
    price: 150,
    category: 'Stationery',
    stock: 30
  },
  {
    _id: ObjectId('67ee0e92d5b35e1b0e6b1411'),
    p_id: 105,
    name: 'Mouse',
    price: 500,
    category: 'Electronics',
    stock: 15
  }
]
```

### Regular Expression Queries
```javascript
// Find users whose name starts with "K"
Exercise-1> db.Users.find({name:/^K/});
[
  {
    _id: ObjectId('67ee0db1d5b35e1b0e6b140b'),
    u_id: 1,
    name: 'Kathir',
    city: 'Madurai'
  }
]

// Find users whose name ends with "r"
Exercise-1> db.Users.find({name:/r$/});
[
  {
    _id: ObjectId('67ee0db1d5b35e1b0e6b140b'),
    u_id: 1,
    name: 'Kathir',
    city: 'Madurai'
  }
]

// Find users whose city contains "re" (case-insensitive)
Exercise-1> db.Users.find({city:{$regex:"re",$options:"i"}});
[
  {
    _id: ObjectId('67ee0db1d5b35e1b0e6b140e'),
    u_id: 4,
    name: 'Mohan',
    city: 'Coimbatore'
  }
]

// Find users whose name contains "ka" (case-insensitive)
Exercise-1> db.Users.find({name:{$regex:"ka",$options:"i"}});
[
  {
    _id: ObjectId('67ee0db1d5b35e1b0e6b140b'),
    name: 'Kathir',
    city: 'Paramakudi',
    age: 34
  }
]
```

### Inclusion and Exclusion Operators
```javascript
// Find users whose name is in a list
Exercise-1> db.Users.find({name:{$in:["Kathir","Jeeva","Ganesan"]}});
[
  {
    _id: ObjectId('67ee0db1d5b35e1b0e6b140b'),
    name: 'Kathir',
    city: 'Paramakudi',
    age: 34
  },
  {
    _id: ObjectId('67ee0db1d5b35e1b0e6b140c'),
    u_id: 2,
    name: 'Jeeva',
    city: 'Paramakudi'
  }
]

// Find users whose name is not "Kathir"
Exercise-1> db.Users.find({name:{$ne:"Kathir"}});
[
  {
    _id: ObjectId('67ee0db1d5b35e1b0e6b140c'),
    u_id: 2,
    name: 'Jeeva',
    city: 'Paramakudi'
  },
  {
    _id: ObjectId('67ee0db1d5b35e1b0e6b140d'),
    u_id: 3,
    name: 'Arun',
    city: 'Chennai'
  },
  {
    _id: ObjectId('67ee0db1d5b35e1b0e6b140e'),
    u_id: 4,
    name: 'Mohan',
    city: 'Coimbatore'
  },
  {
    _id: ObjectId('67ee0db1d5b35e1b0e6b140f'),
    u_id: 5,
    name: 'Ravi',
    city: 'Trichy'
  }
]
```

### Projection
```javascript
// Show only name and city with ID
Exercise-1> db.Users.find({},{name:1,city:1});
[
  {
    _id: ObjectId('67ee0db1d5b35e1b0e6b140b'),
    name: 'Kathir',
    city: 'Madurai'
  },
  {
    _id: ObjectId('67ee0db1d5b35e1b0e6b140c'),
    name: 'Jeeva',
    city: 'Paramakudi'
  },
  {
    _id: ObjectId('67ee0db1d5b35e1b0e6b140d'),
    name: 'Arun',
    city: 'Chennai'
  },
  {
    _id: ObjectId('67ee0db1d5b35e1b0e6b140e'),
    name: 'Mohan',
    city: 'Coimbatore'
  },
  {
    _id: ObjectId('67ee0db1d5b35e1b0e6b140f'),
    name: 'Ravi',
    city: 'Trichy'
  }
]

// Show only name and city, exclude ID
Exercise-1> db.Users.find({},{name:1,city:1,_id:0});
[
  { name: 'Kathir', city: 'Madurai' },
  { name: 'Jeeva', city: 'Paramakudi' },
  { name: 'Arun', city: 'Chennai' },
  { name: 'Mohan', city: 'Coimbatore' },
  { name: 'Ravi', city: 'Trichy' }
]

// Exclude ID when finding users whose name is not "Kathir"
Exercise-1> db.Users.find({name:{$ne:"Kathir"}},{_id:0});
[
  { u_id: 2, name: 'Jeeva', city: 'Paramakudi' },
  { u_id: 3, name: 'Arun', city: 'Chennai' },
  { u_id: 4, name: 'Mohan', city: 'Coimbatore' },
  { u_id: 5, name: 'Ravi', city: 'Trichy' }
]
```

### Existence Checks
```javascript
// Find users where age is not specified
Exercise-1> db.Users.find({age:{$exists:false}});
[
  {
    _id: ObjectId('67ee0db1d5b35e1b0e6b140c'),
    u_id: 2,
    name: 'Jeeva',
    city: 'Paramakudi'
  },
  {
    _id: ObjectId('67ee0db1d5b35e1b0e6b140d'),
    u_id: 3,
    name: 'Arun',
    city: 'Chennai'
  },
  {
    _id: ObjectId('67ee0db1d5b35e1b0e6b140e'),
    u_id: 4,
    name: 'Mohan',
    city: 'Coimbatore'
  },
  {
    _id: ObjectId('67ee0db1d5b35e1b0e6b140f'),
    u_id: 5,
    name: 'Ravi',
    city: 'Trichy'
  }
]

// Find users where city is not specified
Exercise-1> db.Users.find({city:{$exists:false}});
// (Empty result as all users have city specified)

// Find users where age is specified
Exercise-1> db.Users.find({age:{$exists:true}});
[
  {
    _id: ObjectId('67ee0db1d5b35e1b0e6b140b'),
    name: 'Kathir',
    city: 'Paramakudi',
    age: 34
  }
]
```

## Aggregation Framework

### Using $lookup for Joins
```javascript
// Join order details with user information
Exercise-1> db.Orders.aggregate([
  { 
    $lookup: { 
      from: "Users", 
      localField: "u_id", 
      foreignField: "u_id", 
      as: "user" 
    } 
  }, 
  { $unwind: "$user" }, 
  { 
    $project: { 
      _id: 0, 
      "user.name": 1, 
      p_id: 1, 
      qty: 1, 
      status: 1, 
      "user.city": 1 
    } 
  }
]);
[
  {
    p_id: 101,
    qty: 3,
    status: 'Delivered',
    user: { name: 'Jeeva', city: 'Paramakudi' }
  },
  {
    p_id: 103,
    qty: 25,
    status: 'Delivered',
    user: { name: 'Jeeva', city: 'Paramakudi' }
  },
  {
    p_id: 101,
    qty: 2,
    status: 'Shipped',
    user: { name: 'Arun', city: 'Chennai' }
  },
  {
    p_id: 104,
    qty: 4,
    status: 'Pending',
    user: { name: 'Ravi', city: 'Trichy' }
  }
]
```

### Join with Flattened Fields
```javascript
// Extract user details to root level
Exercise-1> db.Orders.aggregate([
  {
    $lookup:{
      from:"Users",
      localField:"u_id",
      foreignField:"u_id",
      as:"users"
    }
  },
  {
    $unwind: "$users"
  },
  {
    $project:{
      _id:0,
      name:"$users.name",   // Extract 'name' from 'user' object
      city:"$users.city",
      p_id:1,
      qty:1,
      status:1
    }
  }
]);
[
  {
    p_id: 101,
    qty: 3,
    status: 'Delivered',
    name: 'Jeeva',
    city: 'Paramakudi'
  },
  {
    p_id: 103,
    qty: 25,
    status: 'Delivered',
    name: 'Jeeva',
    city: 'Paramakudi'
  },
  {
    p_id: 101,
    qty: 2,
    status: 'Shipped',
    name: 'Arun',
    city: 'Chennai'
  },
  {
    p_id: 104,
    qty: 4,
    status: 'Pending',
    name: 'Ravi',
    city: 'Trichy'
  }
]
```

### Lookup Without Unwind
```javascript
// Join without flattening arrays
Exercise-1> db.Orders.aggregate([
  { 
    $lookup: { 
      from: "Users", 
      localField: "u_id", 
      foreignField: "u_id", 
      as: "users" 
    } 
  }, 
  { 
    $project: { 
      _id: 0, 
      name: "$users.name", 
      city: "$users.city", 
      p_id: 1, 
      qty: 1, 
      status: 1 
    } 
  }
]);
[
  {
    p_id: 101,
    qty: 3,
    status: 'Delivered',
    name: [ 'Jeeva' ],
    city: [ 'Paramakudi' ]
  },
  {
    p_id: 103,
    qty: 25,
    status: 'Delivered',
    name: [ 'Jeeva' ],
    city: [ 'Paramakudi' ]
  },
  { p_id: 2, qty: 1, status: 'Pending', name: [], city: [] },
  {
    p_id: 101,
    qty: 2,
    status: 'Shipped',
    name: [ 'Arun' ],
    city: [ 'Chennai' ]
  },
  {
    p_id: 104,
    qty: 4,
    status: 'Pending',
    name: [ 'Ravi' ],
    city: [ 'Trichy' ]
  }
]
```

### Using $group
```javascript
// Count orders by user
Exercise-1> db.Orders.aggregate([
  {
    $group:{
      _id:"$u_id",
      totalOrders:{$sum:1}
    }
  }
]);
[
  { _id: 3, totalOrders: 1 },
  { _id: 1, totalOrders: 1 },
  { _id: 2, totalOrders: 2 },
  { _id: 5, totalOrders: 1 }
]
```

### Renaming Fields in Aggregation
```javascript
// Rename _id to userId in group result
Exercise-1> db.Orders.aggregate([
  {
    $group:{
      _id:"$u_id",
      totalOrder:{$sum:1}
    }
  },
  {
    $project:
    {
      userId:"$_id",
      _id:0,
      totalOrder:1
    }
  }
]);
[
  { totalOrder: 1, userId: 1 },
  { totalOrder: 1, userId: 3 },
  { totalOrder: 2, userId: 2 },
  { totalOrder: 1, userId: 5 }
]
```

### Sorting with Aggregation
```javascript
// Sort orders by quantity in descending order
Exercise-1> db.Orders.aggregate([
  {
    $sort:{qty:-1}
  }
]);
[
  {
    _id: ObjectId('67ee0f9ad5b35e1b0e6b1411'),
    o_id: 1005,
    u_id: 2,
    p_id: 103,
    qty: 25,
    status: 'Delivered'
  },
  {
    _id: ObjectId('67ee0f9ad5b35e1b0e6b1414'),
    o_id: 1020,
    u_id: 5,
    p_id: 104,
    qty: 4,
    status: 'Pending'
  },
  {
    _id: ObjectId('67ee0f9ad5b35e1b0e6b1410'),
    o_id: 1000,
    u_id: 2,
    p_id: 101,
    qty: 3,
    status: 'Delivered'
  },
  {
    _id: ObjectId('67ee0f9ad5b35e1b0e6b1413'),
    o_id: 1015,
    u_id: 3,
    p_id: 101,
    qty: 2,
    status: 'Shipped'
  },
  {
    _id: ObjectId('67ee0f9ad5b35e1b0e6b1412'),
    o_id: 1010,
    u_id: 1,
    p_id: 2,
    qty: 1,
    status: 'Pending'
  }
]

// Sort and project specific fields
Exercise-1> db.Orders.aggregate([
  {
    $sort:{qty:-1}
  },
  {
    $project:
    {
      qty:1,
      o_id:1,
      _id:0
    }
  }
]);
[
  { o_id: 1005, qty: 25 },
  { o_id: 1020, qty: 4 },
  { o_id: 1000, qty: 3 },
  { o_id: 1015, qty: 2 },
  { o_id: 1010, qty: 1 }
]

// Sort by quantity ascending
Exercise-1> db.Orders.aggregate([
  { 
    $sort: { qty: 1 } 
  }, 
  { 
    $project: { 
      qty: 1, 
      o_id: 1, 
      _id: 0 
    } 
  }
]);
[
  { o_id: 1010, qty: 1 },
  { o_id: 1015, qty: 2 },
  { o_id: 1000, qty: 3 },
  { o_id: 1020, qty: 4 },
  { o_id: 1005, qty: 25 }
]
```

### Filter and Aggregation
```javascript
// Calculate total sales for delivered orders
Exercise-1> db.Orders.aggregate([
  {
    $match:{status:"Delivered"}
  },
  {
    $group:{
      _id:null,
      totalSales:{$sum:"$qty"}
    }
  }
]);
[ { _id: null, totalSales: 28 } ]
```

### Complex Aggregation Operations
```javascript
// Calculate total revenue from each product
Exercise-1> db.Orders.aggregate([
  { 
    $lookup: { 
      from: "Products", 
      localField: "p_id", 
      foreignField: "p_id", 
      as: "product_details" 
    } 
  }, 
  { 
    $unwind: "$product_details" 
  }, 
  { 
    $group: { 
      _id: "$p_id", 
      totalRevenue: { 
        $sum: { 
          $multiply: ["$qty", "$product_details.price"] 
        } 
      } 
    } 
  }, 
  { 
    $sort: { 
      totalRevenue: -1 
    } 
  }
]);
[
  { _id: 103, totalRevenue: 3125 },
  { _id: 104, totalRevenue: 600 },
  { _id: 101, totalRevenue: 600 }
]
```

## Import and Export Operations

### Exporting Collections to JSON
```bash
# Export to current directory
kathir@KathirLinux:~$ mongoexport --db=Exercise-1 --collection=Users --out=Users.json --jsonArray
2025-04-03T22:37:56.295+0530	connected to: mongodb://localhost/
2025-04-03T22:37:56.305+0530	exported 5 records

# Export to Downloads folder
mongoexport --db=Exercise-1 --collection=Users --out=~/Downloads/Users.json --jsonArray
```

### Importing Collections from JSON
```bash
# First drop the collection
Exercise-1> db.Users.drop();
true
Exercise-1> db.Users.find();
# (Empty result)

# Import from JSON file
kathir@KathirLinux:~$ mongoimport --db=Exercise-1 --collection=Users --file=Users.json --jsonArray;
2025-04-03T22:43:56.480+0530	connected to: mongodb://localhost/
2025-04-03T22:43:56.561+0530	5 document(s) importe
```
## Key Aggregation Concepts

- **$lookup**: Performs a left outer join with another collection
- **$unwind**: Flattens array fields into separate documents
- **$group**: Groups documents by specified fields and applies aggregation operations
- **$project**: Selects, renames, or modifies fields in documents
- **$match**: Filters documents similar to the find() method
- **$sort**: Sorts documents by specified fields
