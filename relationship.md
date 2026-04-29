Prisma is SQL-first (great for PostgreSQL/MySQL), while Mongoose is document-based for MongoDB.

---

# 1. Prisma Relationships

Prisma schema file:

```prisma
schema.prisma
```

---

# 🔹 One-to-One (User ↔ Profile)

## Schema

```prisma
model User {
  id      Int      @id @default(autoincrement())
  name    String
  profile Profile?
}

model Profile {
  id     Int  @id @default(autoincrement())
  bio    String
  userId Int  @unique
  user   User @relation(fields: [userId], references: [id], onDelete: Cascade)
}
```

---

## Create

```ts
const user = await prisma.user.create({
  data: {
    name: "John",
    profile: {
      create: {
        bio: "Backend developer",
      },
    },
  },
  include: {
    profile: true,
  },
});
```

---

## Read

```ts
const users = await prisma.user.findMany({
  include: {
    profile: true,
  },
});
```

---

## Update

```ts
await prisma.profile.update({
  where: { userId: 1 },
  data: { bio: "Senior developer" },
});
```

---

## Delete

```ts
await prisma.user.delete({
  where: { id: 1 },
});
```

Because:

```prisma
onDelete: Cascade
```

Profile is removed too.

---

---

# 🔹 One-to-Many (User → Orders)

---

## Schema

```prisma
model User {
  id     Int     @id @default(autoincrement())
  name   String
  orders Order[]
}

model Order {
  id     Int    @id @default(autoincrement())
  amount Float

  userId Int
  user   User   @relation(fields: [userId], references: [id], onDelete: Cascade)
}
```

---

## Create

```ts
await prisma.user.create({
  data: {
    name: "John",
    orders: {
      create: [{ amount: 100 }, { amount: 250 }],
    },
  },
});
```

---

## Read

```ts
const user = await prisma.user.findUnique({
  where: { id: 1 },
  include: { orders: true },
});
```

---

## Add new order

```ts
await prisma.order.create({
  data: {
    amount: 500,
    userId: 1,
  },
});
```

---

## Update order

```ts
await prisma.order.update({
  where: { id: 1 },
  data: { amount: 300 },
});
```

---

## Delete order

```ts
await prisma.order.delete({
  where: { id: 1 },
});
```

---

---

# 🔹 Many-to-Many (Students ↔ Courses)

Prisma can do this automatically.

---

## Schema

```prisma
model Student {
  id      Int      @id @default(autoincrement())
  name    String
  courses Course[]
}

model Course {
  id       Int       @id @default(autoincrement())
  title    String
  students Student[]
}
```

Prisma creates junction table automatically.

---

## Create relationship

```ts
await prisma.student.create({
  data: {
    name: "Alice",
    courses: {
      create: [{ title: "Math" }, { title: "Physics" }],
    },
  },
});
```

---

## Connect existing course

```ts
await prisma.student.update({
  where: { id: 1 },
  data: {
    courses: {
      connect: { id: 2 },
    },
  },
});
```

---

## Disconnect relationship only

```ts
await prisma.student.update({
  where: { id: 1 },
  data: {
    courses: {
      disconnect: { id: 2 },
    },
  },
});
```

Removes relationship only.

---

---

# 2. Mongoose Relationships

MongoDB is different:

- embed documents
- or reference documents

---

# 🔹 One-to-One

---

## Models

```js
const UserSchema = new mongoose.Schema({
  name: String,
});

const ProfileSchema = new mongoose.Schema({
  bio: String,
  user: {
    type: mongoose.Schema.Types.ObjectId,
    ref: "User",
  },
});
```

---

## Create

```js
const user = await User.create({
  name: "John",
});

await Profile.create({
  bio: "Backend dev",
  user: user._id,
});
```

---

## Read with populate

```js
const profiles = await Profile.find().populate("user");
```

---

## Update

```js
await Profile.updateOne({ user: userId }, { bio: "Senior dev" });
```

---

## Delete

```js
await Profile.deleteOne({ user: userId });
await User.deleteOne({ _id: userId });
```

MongoDB doesn’t auto-cascade by default.

You handle manually.

---

---

# 🔹 One-to-Many

User → posts

---

## Schema

```js
const UserSchema = new mongoose.Schema({
  name: String,
});

const PostSchema = new mongoose.Schema({
  title: String,
  user: {
    type: mongoose.Schema.Types.ObjectId,
    ref: "User",
  },
});
```

---

## Create

```js
const user = await User.create({
  name: "John",
});

await Post.create({
  title: "First post",
  user: user._id,
});
```

---

## Read

```js
const posts = await Post.find().populate("user");
```

---

## Update

```js
await Post.updateOne({ _id: postId }, { title: "Updated post" });
```

---

## Delete

```js
await Post.deleteOne({ _id: postId });
```

---

---

# 🔹 Many-to-Many

Users ↔ roles

---

## Schema

```js
const UserSchema = new mongoose.Schema({
  name: String,
  roles: [
    {
      type: mongoose.Schema.Types.ObjectId,
      ref: "Role",
    },
  ],
});

const RoleSchema = new mongoose.Schema({
  name: String,
});
```

---

## Create role

```js
const role = await Role.create({
  name: "admin",
});
```

---

## Assign role

```js
await User.updateOne(
  { _id: userId },
  {
    $push: { roles: role._id },
  },
);
```

---

## Read

```js
const user = await User.findById(userId).populate("roles");
```

---

## Remove relationship

```js
await User.updateOne(
  { _id: userId },
  {
    $pull: { roles: roleId },
  },
);
```

---

---

# Prisma vs Mongoose Relationship Thinking

| Feature            | Prisma    | Mongoose            |
| ------------------ | --------- | ------------------- |
| DB Type            | SQL       | MongoDB             |
| Relationships      | strict FK | references/embedded |
| Cascade delete     | built-in  | manual              |
| Joins              | native    | populate            |
| Schema enforcement | strong    | flexible            |

---

# Mental Model

## Prisma

Think:

- tables
- foreign keys
- joins

Good for:

- payments
- auth
- analytics
- structured systems

---

## Mongoose

Think:

- documents
- nested objects
- references

Good for:

- chats
- feeds
- flexible schemas

---

# Common Production Pattern

People often use:

### Prisma + PostgreSQL

for:

- users
- billing
- auth
- orders

### Mongoose + MongoDB

for:

- messages
- activity logs
- events

---
