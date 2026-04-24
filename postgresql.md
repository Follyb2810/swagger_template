Good—focusing on PostgreSQL specifically is the right move. It has some powerful features that go beyond “generic SQL”.

Here’s a **PostgreSQL-focused guide (beginner → advanced)** with real commands and examples you’ll actually use.

---

# 🐘 PostgreSQL Complete Guide

---

# 🚀 1. Install & Start PostgreSQL

```bash
sudo apt update
sudo apt install postgresql postgresql-contrib -y
```

Start service:

```bash
sudo systemctl start postgresql
sudo systemctl enable postgresql
```

---

# 🔑 2. Access PostgreSQL

```bash
sudo -u postgres psql
```

Create database:

```sql
CREATE DATABASE myapp;
```

Connect:

```sql
\c myapp
```

---

# 🧱 3. Create Tables (With Constraints)

```sql
CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    name TEXT NOT NULL,
    email TEXT UNIQUE NOT NULL,
    age INT CHECK (age >= 0),
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

---

# 📥 4. Insert Data

```sql
INSERT INTO users (name, email, age)
VALUES ('John', 'john@example.com', 25);
```

---

# 🔍 5. Filtering (Postgres Style)

### AND / OR

```sql
SELECT * FROM users
WHERE age > 18 AND name LIKE 'J%';
```

---

### ILIKE (case-insensitive 🔥 PostgreSQL feature)

```sql
SELECT * FROM users
WHERE name ILIKE 'j%';
```

---

### IN / NOT IN

```sql
SELECT * FROM users
WHERE id IN (1,2,3);
```

---

### EXISTS (recommended)

```sql
SELECT * FROM users u
WHERE EXISTS (
    SELECT 1 FROM orders o WHERE o.user_id = u.id
);
```

---

# 📊 6. Grouping & Aggregation

```sql
SELECT age, COUNT(*)
FROM users
GROUP BY age
HAVING COUNT(*) > 1;
```

---

# 🔗 7. Joins

```sql
SELECT u.name, o.amount
FROM users u
JOIN orders o ON u.id = o.user_id;
```

---

# ⚡ 8. Indexing (Postgres Power)

### Basic index

```sql
CREATE INDEX idx_users_email ON users(email);
```

---

### Partial index (🔥 advanced)

```sql
CREATE INDEX idx_active_users
ON users(age)
WHERE age > 18;
```

---

### GIN index (for JSON/search)

```sql
CREATE INDEX idx_json_data
ON users USING GIN (data);
```

---

# 🧠 9. JSON Support (Postgres Superpower)

```sql
CREATE TABLE products (
    id SERIAL PRIMARY KEY,
    data JSONB
);
```

Insert:

```sql
INSERT INTO products (data)
VALUES ('{"name": "Phone", "price": 500}');
```

Query:

```sql
SELECT * FROM products
WHERE data->>'name' = 'Phone';
```

---

# 🔁 10. Upsert (Insert or Update)

```sql
INSERT INTO users (id, name)
VALUES (1, 'John')
ON CONFLICT (id)
DO UPDATE SET name = EXCLUDED.name;
```

---

# 🧩 11. CTE (WITH Queries)

```sql
WITH adult_users AS (
    SELECT * FROM users WHERE age >= 18
)
SELECT * FROM adult_users;
```

---

# 🧮 12. Window Functions (Advanced)

```sql
SELECT name, age,
RANK() OVER (ORDER BY age DESC) AS rank
FROM users;
```

---

# 🔐 13. Transactions

```sql
BEGIN;

UPDATE users SET age = 30 WHERE id = 1;

COMMIT;
-- or
ROLLBACK;
```

---

# 🔄 14. Views

```sql
CREATE VIEW adult_users AS
SELECT * FROM users WHERE age >= 18;
```

---

# ⚙️ 15. Performance Optimization

### Explain query

```sql
EXPLAIN ANALYZE
SELECT * FROM users WHERE email = 'test@example.com';
```

---

# 🔒 16. Roles & Permissions

```sql
CREATE USER appuser WITH PASSWORD 'securepass';

GRANT ALL PRIVILEGES ON DATABASE myapp TO appuser;
```

---

# 🧠 17. Advanced Filtering Patterns

### NOT EXISTS (safe)

```sql
SELECT * FROM users u
WHERE NOT EXISTS (
    SELECT 1 FROM orders o WHERE o.user_id = u.id
);
```

---

### Subquery in WHERE

```sql
SELECT * FROM users
WHERE age > (
    SELECT AVG(age) FROM users
);
```

---

# 📦 18. Full Real Example

```sql
SELECT u.name, COUNT(o.id) AS total_orders
FROM users u
LEFT JOIN orders o ON u.id = o.user_id
WHERE u.age > 18
GROUP BY u.name
HAVING COUNT(o.id) > 2
ORDER BY total_orders DESC;
```

---

# ⚡ 19. Postgres-Specific Features You Should Know

- `ILIKE` → case-insensitive search
- `JSONB` → structured data storage
- `ON CONFLICT` → upsert
- `CTE (WITH)` → clean queries
- `GIN indexes` → fast search

---

# ⚠️ Common Mistakes

- Using `NOT IN` with NULL values
- Missing indexes on large tables
- Using `SELECT *` in production
- Ignoring `EXPLAIN ANALYZE`

---

# 🎯 Final Learning Path

### Beginner

- CRUD (SELECT, INSERT, UPDATE, DELETE)

### Intermediate

- JOIN, GROUP BY, INDEX

### Advanced

- CTE, JSONB, window functions
