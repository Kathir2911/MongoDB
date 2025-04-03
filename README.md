# MongoDB
Mongodb learning
# MongoDB E-Commerce Database Operations Guide

This guide demonstrates common database operations for an e-commerce application using MongoDB. The examples cover creating collections, inserting data, querying, updating, and deleting documents.

## Table of Contents
1. [Database and Collection Setup](#database-and-collection-setup)
2. [Data Insertion](#data-insertion)
3. [Basic Queries](#basic-queries)
4. [Document Updates](#document-updates)
5. [Inventory Management](#inventory-management)
6. [Document Deletion](#document-deletion)
7. [Advanced Queries and Filtering](#advanced-queries-and-filtering)
8. [Projection Operations](#projection-operations)
9. [Bulk Operations](#bulk-operations)
10. [Document Replacement](#document-replacement)

## Database and Collection Setup

Create a new database:
```javascript
use Exercise-1;
```

Create collections for the e-commerce system:
```javascript
db.createCollection("Users");
db.createCollection("Products");
db.createCollection("Orders");
db.createCollection("Payments");
```

## Data Insertion

Insert user data:
```javascript
db.Users.insertMany([
  { "u_id": 1, "name": "Kathir", "city": "Madurai" },
  { "u_id": 2, "name": "Jeeva", "city": "Paramakudi" },
  { "u_id": 3, "name": "Arun", "city": "Chennai" },
  { "u_id": 4, "name": "Mohan", "city": "Coimbatore" },
  { "u_id": 5, "name": "Ravi", "city": "Trichy" }
]);
```

Insert product data:
```javascript
db.Products.insertMany([
  { "p_id": 101, "name": "Pen", "price": 20, "category": "Stationery", "stock": 25 },
  { "p_id": 102, "name": "Headphone", "price": 500, "category": "Electronics", "stock": 10 },
  { "p_id": 103, "name": "Carrot", "price": 25, "category": "Vegetables", "stock": 100 },
  { "p_id": 104, "name": "Notebook", "price": 50, "category": "Stationery", "stock": 30 },
  { "p_id": 105, "name": "Mouse", "price": 400, "category": "Electronics", "stock": 15 }
]);
```

Insert order data:
```javascript
db.Orders.insertMany([
  { "o_id": 1000, "u_id": 2, "p_id": 101, "qty": 3, "status": "Pending" },
  { "o_id": 1005, "u_id": 2, "p_id": 103, "qty": 25, "status": "Delivered" },
  { "o_id": 1010, "u_id": 1, "p_id": 102, "qty": 1, "status": "Pending" },
  { "o_id": 1015, "u_id": 3, "p_id": 101, "qty": 2, "status": "Shipped" },
  { "o_id": 1020, "u_id": 5, "p_id": 104, "qty": 4, "status": "Pending" }
]);
```

Insert payment data:
```javascript
db.Payments.insertMany([
  { "pay_id": 29109, "o_id": 1000, "amount": 1000, "status": "Paid", "method": "UPI" },
  { "pay_id": 34522, "o_id": 1005, "amount": 625, "status": "Paid", "method": "Net-banking" },
  { "pay_id": 89231, "o_id": 1010, "amount": 345, "status": "Not-Paid", "method": "Cash" },
  { "pay_id": 56218, "o_id": 1015, "amount": 40, "status": "Paid", "method": "UPI" },
  { "pay_id": 90743, "o_id": 1020, "amount": 200, "status": "Pending", "method": "Card" }
]);
```

## Basic Queries

View all users:
```javascript
db.Users.find();
```

View all products:
```javascript
db.Products.find();
```

View all orders:
```javascript
db.Orders.find();
```

View all payments:
```javascript
db.Payments.find();
```

Find a specific user:
```javascript
db.Users.find({name:"Kathir"});
```

Find orders by a specific user:
```javascript
db.Orders.find({u_id:2});
```

## Document Updates

Update an order status:
```javascript
db.Orders.updateOne({ o_id: 1000 }, { $set: { status: "Delivered" } });
```

Verify the update:
```javascript
db.Orders.find({o_id:1000});
```

## Inventory Management

Decrease product stock after purchase:
```javascript
db.Products.updateOne({p_id:103}, {$inc: {stock: -25}});
```

Check updated stock:
```javascript
db.Products.find({p_id:103});
```

## Document Deletion

Delete a specific user:
```javascript
db.Users.deleteOne({u_id:1});
```

Delete a specific order:
```javascript
db.Orders.deleteOne({o_id: 1005});
```

## Advanced Queries and Filtering

Find orders with "Pending" status:
```javascript
db.Orders.find({status:"Pending"});
```

Find products with price greater than or equal to 25:
```javascript
db.Products.find({price:{$gte:25}});
```

Find users whose name starts with "K":
```javascript
db.Users.find({name:/^K/});
```

Find users whose name ends with "r":
```javascript
db.Users.find({name:/r$/});
```

## Projection Operations

Show only name and city fields (includes _id by default):
```javascript
db.Users.find({}, {name:1, city:1});
```

Show only name and city fields (excluding _id):
```javascript
db.Users.find({}, {name:1, city:1, _id:0});
```

## Bulk Operations

Update price of all products:
```javascript
db.Products.updateMany({}, {$inc:{price:100}});
```

## Document Replacement

Replace an entire document and add new fields:
```javascript
db.Users.replaceOne(
  {name:"Kathir"}, 
  {name:"Kathir", city:"Paramakudi", age:34}
);
```

Verify the replacement:
```javascript
db.Users.find({name:"Kathir"});
```

## Additional Operations

Delete products with price less than 26:
```javascript
db.Products.deleteMany({price:{$lt:25}});
```

Delete all documents in a collection:
```javascript
db.Users.deleteMany({});
```

---

This guide covers fundamental MongoDB operations for managing an e-commerce database system. These examples demonstrate how to perform CRUD operations, apply filters, use projections, and manage inventory in a MongoDB database.
