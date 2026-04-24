Here’s a **complete SQL guide (beginner → advanced)** you can actually learn from and reuse. I’ll keep it structured like a mini-reference + tutorial so you can build real skills.

---

## 🧱 1. SQL Basics (Beginner)

SQL = Structured Query Language
Used to interact with databases like MySQL, PostgreSQL, etc.

---

### 📌 Create a Table

```sql
CREATE TABLE users (
    id INT PRIMARY KEY,
    name VARCHAR(100),
    email VARCHAR(100),
    age INT
);
```

---

### 📌 Insert Data

```sql
INSERT INTO users (id, name, email, age)
VALUES (1, 'John', 'john@example.com', 25);
```

---

### 📌 Select Data

```sql
SELECT * FROM users;
```

---

### 📌 Filter Data

```sql
SELECT * FROM users
WHERE age > 18;
```

---

### 📌 Update Data

```sql
UPDATE users
SET age = 26
WHERE id = 1;
```

---

### 📌 Delete Data

```sql
DELETE FROM users
WHERE id = 1;
```

---

## 🔎 2. Intermediate Queries

### 📊 Sorting

```sql
SELECT * FROM users
ORDER BY age DESC;
```

---

### 🔍 Searching (LIKE)

```sql
SELECT * FROM users
WHERE name LIKE 'J%';
```

---

### 🔢 Limit Results

```sql
SELECT * FROM users
LIMIT 5;
```

---

### 📦 Aggregate Functions

```sql
SELECT COUNT(*) FROM users;
SELECT AVG(age) FROM users;
SELECT MAX(age) FROM users;
```

---

### 📊 GROUP BY

```sql
SELECT age, COUNT(*)
FROM users
GROUP BY age;
```

---

### 🔎 HAVING (filter groups)

```sql
SELECT age, COUNT(*)
FROM users
GROUP BY age
HAVING COUNT(*) > 2;
```

---

## 🔗 3. Joins (Very Important)

Assume:

- `users`
- `orders`

---

### INNER JOIN

```sql
SELECT users.name, orders.amount
FROM users
INNER JOIN orders ON users.id = orders.user_id;
```

---

### LEFT JOIN

```sql
SELECT users.name, orders.amount
FROM users
LEFT JOIN orders ON users.id = orders.user_id;
```

---

### RIGHT JOIN

```sql
SELECT users.name, orders.amount
FROM users
RIGHT JOIN orders ON users.id = orders.user_id;
```

---

## 🧠 4. Advanced SQL

### 🔁 Subqueries

```sql
SELECT name FROM users
WHERE id IN (
    SELECT user_id FROM orders
);
```

---

### 📌 EXISTS

```sql
SELECT name FROM users u
WHERE EXISTS (
    SELECT 1 FROM orders o WHERE o.user_id = u.id
);
```

---

### 🧮 CASE Statements

```sql
SELECT name,
CASE
    WHEN age < 18 THEN 'Minor'
    WHEN age >= 18 THEN 'Adult'
END AS category
FROM users;
```

---

### 📊 Window Functions

```sql
SELECT name, age,
RANK() OVER (ORDER BY age DESC) AS rank
FROM users;
```

---

### 🔄 Common Table Expressions (CTE)

```sql
WITH adult_users AS (
    SELECT * FROM users WHERE age >= 18
)
SELECT * FROM adult_users;
```

---

## ⚡ 5. Indexing (Performance)

```sql
CREATE INDEX idx_users_email
ON users(email);
```

---

## 🔐 6. Constraints

```sql
CREATE TABLE users (
    id INT PRIMARY KEY,
    email VARCHAR(100) UNIQUE,
    age INT CHECK (age >= 0)
);
```

---

## 🧱 7. Transactions

```sql
BEGIN;

UPDATE users SET age = 30 WHERE id = 1;

COMMIT;
-- or
ROLLBACK;
```

---

## 🧬 8. Database Design (Important)

### Relationships:

- One-to-One
- One-to-Many
- Many-to-Many

Example:

```sql
CREATE TABLE orders (
    id INT PRIMARY KEY,
    user_id INT,
    FOREIGN KEY (user_id) REFERENCES users(id)
);
```

---

## 🚀 9. Expert-Level Concepts

### 🔥 Query Optimization

```sql
EXPLAIN SELECT * FROM users WHERE email = 'test@example.com';
```

---

### 🔁 Upsert (Insert or Update)

**PostgreSQL:**

```sql
INSERT INTO users (id, name)
VALUES (1, 'John')
ON CONFLICT (id)
DO UPDATE SET name = 'John Updated';
```

---

### 🔐 Views

```sql
CREATE VIEW adult_users AS
SELECT * FROM users WHERE age >= 18;
```

---

### 🧩 Stored Procedures

```sql
CREATE PROCEDURE get_users()
BEGIN
    SELECT * FROM users;
END;
```

---

### 🔁 Triggers

```sql
CREATE TRIGGER before_insert_user
BEFORE INSERT ON users
FOR EACH ROW
SET NEW.name = UPPER(NEW.name);
```

---

## 🧪 10. Real-World Query Example

```sql
SELECT u.name, SUM(o.amount) AS total_spent
FROM users u
JOIN orders o ON u.id = o.user_id
GROUP BY u.name
ORDER BY total_spent DESC;
```

---

## 🎯 Learning Path

**Beginner**

- SELECT, INSERT, UPDATE, DELETE

**Intermediate**

- JOIN, GROUP BY, INDEX

**Advanced**

- CTE, Window functions, Subqueries

**Expert**

- Optimization, scaling, transactions

---

You’re right to call that out—those are **important advanced filtering patterns**, and they’re used all the time in real queries. Let’s go through them clearly with examples so you actually understand when to use each.

---

# 🔍 1. `IN` and `NOT IN`

### ✅ `IN` (match any value in a list)

```sql
SELECT * FROM users
WHERE id IN (1, 2, 3);
```

👉 Equivalent to:

```sql
WHERE id = 1 OR id = 2 OR id = 3
```

---

### ❌ `NOT IN`

```sql
SELECT * FROM users
WHERE id NOT IN (1, 2, 3);
```

👉 Excludes those values.

---

### ⚠️ Important Gotcha (VERY COMMON BUG)

If the subquery returns `NULL`, `NOT IN` can return **no rows at all**.

```sql
SELECT * FROM users
WHERE id NOT IN (SELECT user_id FROM orders);
```

If `orders.user_id` contains NULL → ❌ broken result

---

# 🔁 2. `EXISTS` (Very Powerful)

### ✅ Basic EXISTS

```sql
SELECT * FROM users u
WHERE EXISTS (
    SELECT 1 FROM orders o
    WHERE o.user_id = u.id
);
```

👉 Meaning:
“Give me users who HAVE at least one order”

---

### ❌ `NOT EXISTS`

```sql
SELECT * FROM users u
WHERE NOT EXISTS (
    SELECT 1 FROM orders o
    WHERE o.user_id = u.id
);
```

👉 Meaning:
“Give me users who have NO orders”

---

# 🧠 Difference: `IN` vs `EXISTS`

### 🔹 Using `IN`

```sql
SELECT * FROM users
WHERE id IN (SELECT user_id FROM orders);
```

### 🔹 Using `EXISTS`

```sql
SELECT * FROM users u
WHERE EXISTS (
    SELECT 1 FROM orders o
    WHERE o.user_id = u.id
);
```

---

### ⚡ Key Differences

| Feature              | IN  | EXISTS |
| -------------------- | --- | ------ |
| Works with list      | ✅  | ❌     |
| Works with subquery  | ✅  | ✅     |
| Stops early (faster) | ❌  | ✅     |
| Safe with NULL       | ❌  | ✅     |

👉 **Rule of thumb:**

- Small dataset → `IN`
- Large dataset → `EXISTS`

---

# 🚫 3. `NOT EXISTS` vs `NOT IN` (Critical)

### ❌ Risky (can break)

```sql
SELECT * FROM users
WHERE id NOT IN (SELECT user_id FROM orders);
```

---

### ✅ Safe (recommended)

```sql
SELECT * FROM users u
WHERE NOT EXISTS (
    SELECT 1 FROM orders o
    WHERE o.user_id = u.id
);
```

👉 This is the **correct production approach**

---

# 🧩 4. Using `WHERE (...)` with Subqueries

You asked about:

> `SELECT * WHERE ()`

Here’s how it actually works:

---

### ✅ Subquery returning single value

```sql
SELECT * FROM users
WHERE age > (
    SELECT AVG(age) FROM users
);
```

👉 Users older than average

---

### ✅ Subquery returning multiple values

```sql
SELECT * FROM users
WHERE id IN (
    SELECT user_id FROM orders
);
```

---

### ❌ Wrong usage

```sql
SELECT * FROM users
WHERE (SELECT * FROM orders); -- ❌ invalid
```

👉 Subqueries must return:

- a **single value**
- or a **single column list**

---

# 🧠 5. Real-World Examples

### 🧾 Users who made orders

```sql
SELECT * FROM users u
WHERE EXISTS (
    SELECT 1 FROM orders o
    WHERE o.user_id = u.id
);
```

---

### 🧾 Users who never ordered

```sql
SELECT * FROM users u
WHERE NOT EXISTS (
    SELECT 1 FROM orders o
    WHERE o.user_id = u.id
);
```

---

### 💰 Orders above average amount

```sql
SELECT * FROM orders
WHERE amount > (
    SELECT AVG(amount) FROM orders
);
```

---

### 🧍 Users NOT in a banned list

```sql
SELECT * FROM users
WHERE id NOT IN (1, 5, 9);
```

---

# ⚡ 6. Performance Insight (Advanced)

- `EXISTS` → stops when it finds first match → faster
- `IN` → builds full list → slower on large data

👉 That’s why big systems prefer:

```sql
EXISTS / NOT EXISTS
```

You’re right—those are core SQL building blocks, not optional extras. Let’s fill the gaps properly and tie them together so you can actually use them in real queries.

I’ll group them into **filtering, grouping, indexing, and constraints** with clear examples.

---

# 🔍 1. Advanced Filtering (`AND`, `OR`, `%`, etc.)

### ✅ `AND` (all conditions must match)

```sql
SELECT * FROM users
WHERE age > 18 AND country = 'Nigeria';
```

---

### ✅ `OR` (any condition can match)

```sql
SELECT * FROM users
WHERE country = 'Nigeria' OR country = 'Ghana';
```

---

### 🔀 Combine `AND` + `OR` (use parentheses!)

```sql
SELECT * FROM users
WHERE (country = 'Nigeria' OR country = 'Ghana')
AND age > 18;
```

👉 Without parentheses → wrong results

---

### 🔍 `%` with `LIKE` (pattern matching)

```sql
SELECT * FROM users
WHERE name LIKE 'J%';
```

👉 Starts with “J”

---

More patterns:

```sql
-- Ends with 'n'
WHERE name LIKE '%n';

-- Contains 'oh'
WHERE name LIKE '%oh%';

-- Exactly 4 letters
WHERE name LIKE '____';
```

---

# 📊 2. Grouping (`GROUP BY` + Aggregation)

### ✅ Basic grouping

```sql
SELECT country, COUNT(*) AS total_users
FROM users
GROUP BY country;
```

---

### ✅ With condition on groups (`HAVING`)

```sql
SELECT country, COUNT(*) AS total_users
FROM users
GROUP BY country
HAVING COUNT(*) > 5;
```

👉 `WHERE` = before grouping
👉 `HAVING` = after grouping

---

### ✅ Multiple grouping

```sql
SELECT country, age, COUNT(*)
FROM users
GROUP BY country, age;
```

---

# 🧮 3. Aggregate Functions

```sql
SELECT
    COUNT(*) AS total,
    AVG(age) AS average_age,
    MAX(age) AS max_age,
    MIN(age) AS min_age
FROM users;
```

---

# ⚡ 4. Indexes (Performance)

Indexes make queries faster (like a book index).

---

### ✅ Create index

```sql
CREATE INDEX idx_users_email
ON users(email);
```

---

### ✅ Unique index

```sql
CREATE UNIQUE INDEX idx_unique_email
ON users(email);
```

---

### ✅ Composite index

```sql
CREATE INDEX idx_user_country_age
ON users(country, age);
```

---

### ❗ When to use indexes

Use on:

- `WHERE`
- `JOIN`
- `ORDER BY`

---

### ❌ When NOT to use

- Small tables
- Frequently updated columns (slows writes)

---

# 🔐 5. Constraints (Data Integrity)

Constraints control what data is allowed.

---

### ✅ Primary Key

```sql
id INT PRIMARY KEY
```

👉 Unique + not null

---

### ✅ NOT NULL

```sql
name VARCHAR(100) NOT NULL
```

---

### ✅ UNIQUE

```sql
email VARCHAR(100) UNIQUE
```

---

### ✅ DEFAULT

```sql
age INT DEFAULT 18
```

---

### ✅ CHECK

```sql
age INT CHECK (age >= 0)
```

---

### ✅ FOREIGN KEY

```sql
CREATE TABLE orders (
    id INT PRIMARY KEY,
    user_id INT,
    FOREIGN KEY (user_id) REFERENCES users(id)
);
```

---

# 🔁 6. Putting It All Together (Real Example)

```sql
SELECT country, COUNT(*) AS total_users
FROM users
WHERE age > 18
AND name LIKE 'J%'
GROUP BY country
HAVING COUNT(*) > 2
ORDER BY total_users DESC;
```

👉 This combines:

- filtering (`WHERE`)
- pattern (`LIKE %`)
- grouping (`GROUP BY`)
- filtering groups (`HAVING`)
- sorting (`ORDER BY`)

---

# 🧠 7. Bonus: Logical Operators

### BETWEEN

```sql
SELECT * FROM users
WHERE age BETWEEN 18 AND 30;
```

---

### IS NULL

```sql
SELECT * FROM users
WHERE email IS NULL;
```

---

### IS NOT NULL

```sql
SELECT * FROM users
WHERE email IS NOT NULL;
```

---
