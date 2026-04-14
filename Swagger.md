* GET
* POST (JSON)
* PUT
* DELETE
* Multipart/form-data (file upload)
* Full User API example

---

# 🧠 1. Swagger Basic Structure (IMPORTANT)

Every route follows this pattern:

```ts
/**
 * @swagger
 * /path:
 *   method:
 *     summary: short description
 *     tags:
 *       - GroupName
 *     parameters: []
 *     requestBody: {}
 *     responses: {}
 */
```

---

# 🟢 2. GET Request Example

### 👉 Get user by ID

```ts
/**
 * @swagger
 * /api/users/{id}:
 *   get:
 *     summary: Get user by ID
 *     tags: [Users]
 *     parameters:
 *       - in: path
 *         name: id
 *         required: true
 *         schema:
 *           type: string
 *         example: 12345
 *     responses:
 *       200:
 *         description: User fetched successfully
 *         content:
 *           application/json:
 *             example:
 *               success: true
 *               data:
 *                 id: "12345"
 *                 name: "John Doe"
 */
```

---

# 🟡 3. POST (JSON BODY)

### 👉 Create User

```ts
/**
 * @swagger
 * /api/users:
 *   post:
 *     summary: Create a new user
 *     tags: [Users]
 *     requestBody:
 *       required: true
 *       content:
 *         application/json:
 *           schema:
 *             type: object
 *             required:
 *               - name
 *               - email
 *             properties:
 *               name:
 *                 type: string
 *                 example: John Doe
 *               email:
 *                 type: string
 *                 example: john@example.com
 *     responses:
 *       201:
 *         description: User created successfully
 *         content:
 *           application/json:
 *             example:
 *               success: true
 *               message: User created
 */
```

---

# 🟠 4. PUT (UPDATE)

### 👉 Update user

```ts
/**
 * @swagger
 * /api/users/{id}:
 *   put:
 *     summary: Update user
 *     tags: [Users]
 *     parameters:
 *       - in: path
 *         name: id
 *         required: true
 *         schema:
 *           type: string
 *     requestBody:
 *       required: true
 *       content:
 *         application/json:
 *           schema:
 *             type: object
 *             properties:
 *               name:
 *                 type: string
 *                 example: Updated Name
 *               email:
 *                 type: string
 *                 example: updated@email.com
 *     responses:
 *       200:
 *         description: User updated
 */
```

---

# 🔴 5. DELETE

### 👉 Delete user

```ts
/**
 * @swagger
 * /api/users/{id}:
 *   delete:
 *     summary: Delete user
 *     tags: [Users]
 *     parameters:
 *       - in: path
 *         name: id
 *         required: true
 *         schema:
 *           type: string
 *     responses:
 *       200:
 *         description: User deleted
 */
```

---

# 🟣 6. Multipart (FILE UPLOAD)

### 👉 Upload avatar

```ts
/**
 * @swagger
 * /api/users/upload-avatar:
 *   post:
 *     summary: Upload user avatar
 *     tags: [Users]
 *     requestBody:
 *       required: true
 *       content:
 *         multipart/form-data:
 *           schema:
 *             type: object
 *             required:
 *               - file
 *             properties:
 *               file:
 *                 type: string
 *                 format: binary
 *     responses:
 *       200:
 *         description: File uploaded successfully
 *         content:
 *           application/json:
 *             example:
 *               success: true
 *               url: "https://cdn.com/avatar.png"
 */
```

---

# 🔵 7. QUERY PARAMS (FILTERING)

### 👉 Get users with pagination

```ts
/**
 * @swagger
 * /api/users:
 *   get:
 *     summary: Get all users
 *     tags: [Users]
 *     parameters:
 *       - in: query
 *         name: page
 *         schema:
 *           type: number
 *         example: 1
 *
 *       - in: query
 *         name: limit
 *         schema:
 *           type: number
 *         example: 10
 *     responses:
 *       200:
 *         description: Users list
 */
```

---

# 🧩 8. REUSABLE SCHEMAS (BEST PRACTICE)

```ts
/**
 * @swagger
 * components:
 *   schemas:
 *     User:
 *       type: object
 *       properties:
 *         id:
 *           type: string
 *         name:
 *           type: string
 *         email:
 *           type: string
 */
```

Then reuse:

```ts
/**
 * @swagger
 * /api/users/{id}:
 *   get:
 *     responses:
 *       200:
 *         content:
 *           application/json:
 *             schema:
 *               $ref: '#/components/schemas/User'
 */
```

---

# 🧠 9. FULL REAL USER API (COPY TEMPLATE)

```ts
/**
 * @swagger
 * /api/users:
 *   post:
 *     summary: Create user
 *     tags: [Users]
 *     requestBody:
 *       required: true
 *       content:
 *         application/json:
 *           example:
 *             name: John
 *             email: john@email.com
 *     responses:
 *       200:
 *         description: success
 */
```

```ts
/**
 * @swagger
 * /api/users/{id}:
 *   get:
 *     summary: Get user
 *     tags: [Users]
 *     parameters:
 *       - in: path
 *         name: id
 *         required: true
 *     responses:
 *       200:
 *         description: success
 */
```

```ts
/**
 * @swagger
 * /api/users/{id}:
 *   put:
 *     summary: Update user
 *     tags: [Users]
 *     requestBody:
 *       required: true
 *       content:
 *         application/json:
 *           example:
 *             name: Updated Name
 *     responses:
 *       200:
 *         description: updated
 */
```

```ts
/**
 * @swagger
 * /api/users/{id}:
 *   delete:
 *     summary: Delete user
 *     tags: [Users]
 *     responses:
 *       200:
 *         description: deleted
 */
```

---

# 🚀 10. RULES YOU MUST REMEMBER

### ✔ Always:

* Use `tags` to group APIs
* Use `example` for frontend clarity
* Use `requestBody` for POST/PUT
* Use `parameters` for GET/DELETE path/query

---

### ❌ Avoid:

* mixing query + body incorrectly
* missing `required: true` when needed
* inconsistent naming

---

# 🔥 BONUS (your Zealy style tip)

For your project:

You should always structure like:

```
Users
Quests
Leaderboard
XP
Admin
```
