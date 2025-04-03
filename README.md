# MongoDB
Mongodb learning
# MongoDB Database Operations Tutorial

This tutorial demonstrates common MongoDB operations using a simple e-commerce database example. We'll cover creating databases and collections, inserting data, querying, updating, deleting, and performing aggregation operations.

## 1. Creating a Database and Collections

```javascript
// Create and switch to a new database
use Exercise-1;

// Create collections
db.createCollection("Users");
db.createCollection("Products");
db.createCollection("Orders");
db.createCollection("Payments");
```

## 2. Inserting Data

### Inserting Users
```javascript
db.Users.insertMany([
  { "u_id": 1, "name": "Kathir", "city": "Madurai" },
  { "u_id": 2, "name": "Jeeva", "city": "Paramakudi" },
  { "u_id": 3, "name": "Arun", "city": "Chennai" },
  { "u_id": 4, "name": "Mohan", "city": "Coimbatore" },
  { "u_id": 5, "name": "Ravi", "city": "Trichy" }
]);
```

### Inserting Products
```javascript
db.Products.insertMany([
  { "p_id": 101, "name": "Pen", "price": 20, "category": "Stationery", "stock": 25 },
  { "p_id": 102, "name": "Headphone", "price": 500, "category": "Electronics", "stock": 10 },
  { "p_id": 103, "name": "Carrot", "price": 25, "category": "Vegetables", "stock": 100 },
  { "p_id": 104, "name": "Notebook", "price": 50, "category": "Stationery", "stock": 30 },
  { "p_id": 105, "name": "Mouse", "price": 400, "category": "Electronics", "stock": 15 }
]);
```

### Inserting Orders
```javascript
db.Orders.insertMany([
  { "o_id": 1000, "u_id": 2, "p_id": 101, "qty": 3, "status": "Pending" },
  { "o_id": 1005, "u_id": 2, "p_id": 103, "qty": 25, "status": "Delivered" },
  { "o_id": 1010, "u_id": 1, "p_id": 102, "qty": 1, "status": "Pending" },
  { "o_id": 1015, "u_id": 3, "p_id": 101, "qty": 2, "status": "Shipped" },
  { "o_id": 1020, "u_id": 5, "p_id": 104, "qty": 4, "status": "Pending" }
]);
```

### Inserting Payments
```javascript
db.Payments.insertMany([
  { "pay_id": 29109, "o_id": 1000, "amount": 1000, "status": "Paid", "method": "UPI" },
  { "pay_id": 34522, "o_id": 1005, "amount": 625, "status": "Paid", "method": "Net-banking" },
  { "pay_id": 89231, "o_id": 1010, "amount": 345, "status": "Not-Paid", "method": "Cash" },
  { "pay_id": 56218, "o_id": 1015, "amount": 40, "status": "Paid", "method": "UPI" },
  { "pay_id": 90743, "o_id": 1020, "amount": 200, "status": "Pending", "method": "Card" }
]);
```

## 3. Basic Find Operations

### Find All Documents in a Collection
```javascript
db.Users.find();
db.Products.find();
db.Orders.find();
db.Payments.find();
```

## 4. Querying Specific Documents

### Find by Specific Field Value
```javascript
// Find user by name
db.Users.find({name: "Kathir"});

// Find orders by user ID
db.Orders.find({u_id: 2});
```

## 5. Update Operations

### Update a Single Document
```javascript
// Update order status
db.Orders.updateOne(
  { o_id: 1000 }, 
  { $set: { status: "Delivered" } }
);

// Verify the update
db.Orders.find({o_id: 1000});
```

### Increment/Decrement Values
```javascript
// Decrease product stock after purchase
db.Products.updateOne(
  {p_id: 103},
  {$inc: {stock: -25}}
);

// Verify the stock update
db.Products.find({p_id: 103});
```

## 6. Delete Operations

### Delete Single Documents
```javascript
// Delete a user
db.Users.deleteOne({u_id: 1});

// Delete an order
db.Orders.deleteOne({o_id: 1005});
```

## 7. Advanced Query Operations

### Find by Status
```javascript
// Find pending orders
db.Orders.find({status: "Pending"});
```

### Comparison Operators
```javascript
// Find products with price >= 25
db.Products.find({price: {$gte: 25}});

// Find products with price between 121 and 500
db.Products.find({ price: { $gte: 121, $lte: 500 } });
```

### Regular Expression Queries
```javascript
// Find users whose name starts with "K"
db.Users.find({name: /^K/});

// Find users whose name ends with "r"
db.Users.find({name: /r$/});

// Find users whose city contains "re" (case-insensitive)
db.Users.find({city: {$regex: "re", $options: "i"}});
```

### Inclusion and Exclusion of Fields
```javascript
// Include only name and city (with ID)
db.Users.find({}, {name: 1, city: 1});

// Include only name and city (exclude ID)
db.Users.find({}, {name: 1, city: 1, _id: 0});
```

### Query Operators: $in, $ne, $exists
```javascript
// Find users whose name is in the specified list
db.Users.find({name: {$in: ["Kathir", "Jeeva", "Ganesan"]}});

// Find users whose name is not "Kathir"
db.Users.find({name: {$ne: "Kathir"}});

// Find users where age field is not specified
db.Users.find({age: {$exists: false}});

// Find users where age field is specified
db.Users.find({age: {$exists: true}});
```

## 8. Update Multiple Documents

### Update All Documents
```javascript
// Increase all product prices by 100
db.Products.updateMany({}, {$inc: {price: 100}});
```

### Replace an Entire Document
```javascript
// Replace user document and add new field
db.Users.replaceOne(
  {name: "Kathir"},
  {name: "Kathir", city: "Paramakudi", age: 34}
);

// Verify the replacement
db.Users.find({name: "Kathir"});
```

### Delete Multiple Documents
```javascript
// Delete products with price less than 25
db.Products.deleteMany({price: {$lt: 25}});

// Delete all documents in a collection
db.Users.deleteMany({});
```

## 9. Aggregation Operations

### Join Collections with $lookup
```javascript
// Join order details with user information
db.Orders.aggregate([
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

// Extract user name and city as separate fields
db.Orders.aggregate([
  {
    $lookup: {
      from: "Users",
      localField: "u_id",
      foreignField: "u_id",
      as: "users"
    }
  },
  {
    $unwind: "$users"
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
```

### Group By Operations
```javascript
// Count orders by user
db.Orders.aggregate([
  {
    $group: {
      _id: "$u_id",
      totalOrders: {$sum: 1}
    }
  }
]);

// Count orders by user with renamed fields
db.Orders.aggregate([
  {
    $group: {
      _id: "$u_id",
      totalOrder: {$sum: 1}
    }
  },
  {
    $project: {
      userId: "$_id",
      _id: 0,
      totalOrder: 1
    }
  }
]);
```

### Sorting Operations
```javascript
// Sort orders by quantity (descending)
db.Orders.aggregate([
  {
    $sort: {qty: -1}
  }
]);

// Sort and project specific fields
db.Orders.aggregate([
  {
    $sort: {qty: -1}
  },
  {
    $project: {
      qty: 1,
      o_id: 1,
      _id: 0
    }
  }
]);

// Sort orders by quantity (ascending)
db.Orders.aggregate([
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
```

### Combining Match and Group
```javascript
// Total sales for delivered orders only
db.Orders.aggregate([
  {
    $match: {status: "Delivered"}
  },
  {
    $group: {
      _id: null,
      totalSales: {$sum: "$qty"}
    }
  }
]);
```

## Key Aggregation Concepts

- **$lookup**: Performs a left outer join with another collection
- **$unwind**: Flattens array fields into separate documents
- **$group**: Groups documents by specified fields and applies aggregation operations
- **$project**: Selects, renames, or modifies fields in documents
- **$match**: Filters documents similar to the find() method
- **$sort**: Sorts documents by specified fields
