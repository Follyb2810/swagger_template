Good move—learning MongoDB will complement your SQL knowledge, especially for flexible, JSON-style data.

Here’s a **complete MongoDB guide (beginner → advanced → expert)** focused on real usage.

---

# 🍃 MongoDB Complete Guide

---

# 🚀 1. Install & Start MongoDB (Linux)

```bash
sudo apt update
sudo apt install mongodb -y
sudo systemctl start mongodb
sudo systemctl enable mongodb
```

Open shell:

```bash
mongo
```

---

# 🧱 2. Basic Concepts

- **Database** → like a container
- **Collection** → like a table
- **Document** → like a row (JSON)

---

# 📦 3. Create Database & Collection

```js
use myapp
```

Insert (auto-creates collection):

```js
db.users.insertOne({
  name: "John",
  age: 25,
  country: "Nigeria",
});
```

---

# 🔍 4. Read Data (Queries)

### Get all

```js
db.users.find();
```

---

### Filter

```js
db.users.find({ age: { $gt: 18 } });
```

---

### AND condition

```js
db.users.find({
  age: { $gt: 18 },
  country: "Nigeria",
});
```

---

### OR condition

```js
db.users.find({
  $or: [{ country: "Nigeria" }, { country: "Ghana" }],
});
```

---

# 🔎 5. Query Operators

### Comparison

```js
$gt; // greater than
$lt; // less than
$gte; // >=
$lte; // <=
$ne; // not equal
$in; // in array
$nin; // not in
```

Example:

```js
db.users.find({
  age: { $in: [20, 25, 30] },
});
```

---

# 🔍 6. Pattern Search (LIKE equivalent)

```js
db.users.find({
  name: { $regex: "^J" },
});
```

👉 starts with "J"

---

# 📊 7. Projection (Select fields)

```js
db.users.find({ age: { $gt: 18 } }, { name: 1, age: 1, _id: 0 });
```

---

# 🔄 8. Update Data

```js
db.users.updateOne({ name: "John" }, { $set: { age: 26 } });
```

---

### Increment

```js
db.users.updateOne({ name: "John" }, { $inc: { age: 1 } });
```

---

# ❌ 9. Delete Data

```js
db.users.deleteOne({ name: "John" });
```

---

# 📊 10. Sorting & Limiting

```js
db.users.find().sort({ age: -1 }).limit(5);
```

---

# 🧮 11. Aggregation (GROUP BY equivalent)

```js
db.users.aggregate([
  {
    $group: {
      _id: "$country",
      total: { $sum: 1 },
    },
  },
]);
```

---

### HAVING equivalent

```js
db.users.aggregate([
  { $group: { _id: "$country", total: { $sum: 1 } } },
  { $match: { total: { $gt: 2 } } },
]);
```

---

# 🔗 12. Lookup (JOIN equivalent)

```js
db.orders.aggregate([
  {
    $lookup: {
      from: "users",
      localField: "user_id",
      foreignField: "_id",
      as: "user",
    },
  },
]);
```

---

# ⚡ 13. Indexing

```js
db.users.createIndex({ email: 1 });
```

---

### Compound index

```js
db.users.createIndex({ country: 1, age: -1 });
```

---

### Unique index

```js
db.users.createIndex({ email: 1 }, { unique: true });
```

---

# 🧠 14. Advanced Queries

### Exists

```js
db.users.find({ email: { $exists: true } });
```

---

### Not exists

```js
db.users.find({ email: { $exists: false } });
```

---

### NOT

```js
db.users.find({
  age: { $not: { $gt: 30 } },
});
```

---

# 📦 15. Schema Design (Important)

MongoDB is schema-less, but you should still design:

### Embedded

```js
{
  name: "John",
  orders: [
    { item: "Phone", price: 500 }
  ]
}
```

---

### Referenced

```js
{
  user_id: ObjectId("...");
}
```

---

# 🔁 16. Transactions (Advanced)

```js
session = db.getMongo().startSession();
session.startTransaction();

// operations

session.commitTransaction();
```

---

# ⚙️ 17. Performance Optimization

### Explain query

```js
db.users.find({ email: "test@example.com" }).explain("executionStats");
```

---

# 🔐 18. Index Strategy

Use indexes on:

- frequently queried fields
- joins (`lookup`)
- sorting

---

# 🚨 19. Common Mistakes

- No indexes → slow queries
- Overusing `$lookup` → slow joins
- Poor schema design
- Storing too much nested data

---

# 🧠 20. SQL vs MongoDB Mapping

| SQL      | MongoDB    |
| -------- | ---------- |
| Table    | Collection |
| Row      | Document   |
| JOIN     | `$lookup`  |
| GROUP BY | `$group`   |
| WHERE    | `find()`   |

---

# 🎯 Final Learning Path

### Beginner

- insertOne, find, update, delete

### Intermediate

- operators, sorting, indexes

### Advanced

- aggregation, lookup

### Expert

- schema design, scaling, performance

---
