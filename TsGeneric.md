Let’s build from beginner → advanced using your exact helper:

```ts id="r0s94z"
const ensureFound = <T>(data: T | null, message = "Not found"): T => {
  if (!data) {
    throw new Error(message);
  }
  return data;
};
```

---

# 1. What problem do generics solve?

Without generics:

```ts id="2z93uh"
function identity(value: string): string {
  return value;
}
```

Works only for strings.

---

If you want numbers too:

```ts id="o9h3xw"
function identity(value: number): number {
  return value;
}
```

Now you're duplicating code.

That gets ugly fast.

---

## Generic solution

```ts id="z1q8fn"
function identity<T>(value: T): T {
  return value;
}
```

Now:

```ts id="vdbi9x"
identity("hello"); // string
identity(42); // number
identity(true); // boolean
```

---

## Meaning of `<T>`

`T` means:

> “I don’t know the type yet. Tell me later.”

Like a placeholder.

You can mentally read:

```ts id="iycq5h"
function identity<T>(value: T): T;
```

as:

> takes some type T, returns same type T.

---

---

# 2. How TypeScript fills T automatically

You call:

```ts id="p8jcv0"
identity("hi");
```

TypeScript sees:

```ts id="y9ul8k"
T = string;
```

So internally:

```ts id="xmq5qv"
function identity(value: string): string;
```

---

Call:

```ts id="xtclku"
identity(123);
```

Now:

```ts id="43wjlwm"
T = number;
```

---

You usually don’t manually write T.

But you can:

```ts id="0z5bgu"
identity<string>("hello");
```

Same thing.

---

---

# 3. Your helper explained

Your code:

```ts id="m6d5qo"
const ensureFound = <T>(data: T | null): T => {
  if (!data) throw new Error();
  return data;
};
```

Let’s decode it.

---

## Input

```ts id="u0fuh5"
data: T | null;
```

means:

data can be:

- some type T
- OR null

Examples:

```ts id="d5j2yb"
User | null;
Consultation | null;
string | null;
```

---

## Output

```ts id="o7zq0r"
: T
```

means:

After this function succeeds,
I guarantee it is no longer null.

---

That’s the magic.

---

## Example

Without helper:

```ts id="l7iyu7"
const user = await getUser();

if (!user) {
  throw new Error("Not found");
}

console.log(user.name);
```

---

With helper:

```ts id="lyw0b6"
const user = ensureFound(await getUser());
console.log(user.name);
```

Because `ensureFound` guarantees:

```ts id="kl6b5v"
User | null -> User
```

Null removed.

---

---

# 4. Step-by-step type inference

Suppose:

```ts id="ltc3ys"
const consultation = await getConsultation();
```

returns:

```ts id="ztw2kk"
Consultation | null;
```

Then:

```ts id="4xv9ku"
ensureFound(consultation);
```

TypeScript infers:

```ts id="1hjlwm"
T = Consultation;
```

So function becomes:

```ts id="cvl6k3"
(data: Consultation | null): Consultation
```

Perfect.

---

---

# 5. Why not use `any`

Bad:

```ts id="p7o8wu"
function ensureFound(data: any) {
  return data;
}
```

Now TypeScript loses safety.

You can do nonsense:

```ts id="26xgjg"
data.thisDoesNotExist;
```

No errors.

Dangerous.

---

Generic keeps type.

---

# 6. Multiple generics

You can have more than one.

Example:

```ts id="ndk8vf"
function pair<T, U>(first: T, second: U) {
  return { first, second };
}
```

Usage:

```ts id="xvclm1"
pair("doctor", 123);
```

TypeScript infers:

```ts id="vduvwe"
T = string;
U = number;
```

Output:

```ts id="o1bd0x"
{
  first: string;
  second: number;
}
```

---

---

# 7. Generic arrays

```ts id="2uvyk6"
function getFirst<T>(items: T[]): T {
  return items[0];
}
```

Usage:

```ts id="m3rll5"
getFirst([1, 2, 3]); // number
getFirst(["a", "b"]); // string
```

---

---

# 8. Generic constraints (advanced)

Sometimes you want:

> generic, but only certain shapes allowed

Example:

```ts id="0qfww2"
function printId<T extends { id: string }>(item: T) {
  console.log(item.id);
}
```

This means:

T can be anything **as long as it has id**

---

Works:

```ts id="z0n1pq"
printId({ id: "123", name: "John" });
```

Fails:

```ts id="t6v4eg"
printId({ name: "John" });
```

No `id`.

---

---

# 9. Your helper with constraint (advanced)

You could even do:

```ts id="h8m88q"
const ensureEntity = <T extends { id: string }>(data: T | null): T => {
  if (!data) throw new Error("Not found");
  return data;
};
```

Now only objects with `id` allowed.

---

---

# 10. Real-world backend use cases

Generics are everywhere in backend.

---

## API response wrapper

```ts id="swtz8r"
type ApiResponse<T> = {
  success: boolean;
  data: T;
};
```

Usage:

```ts id="6k2xw8"
type UserResponse = ApiResponse<User>;
type ConsultationResponse = ApiResponse<Consultation>;
```

---

---

## Pagination

```ts id="snjlwm"
type Paginated<T> = {
  items: T[];
  total: number;
};
```

Usage:

```ts id="i7k7su"
Paginated<User>;
Paginated<Message>;
```

---

---

# 11. Read generics like English

When you see:

```ts id="r73p1p"
<T>
```

Read:

> some unknown type

---

When you see:

```ts id="9u9n11"
<T extends Something>
```

Read:

> some type constrained by Something

---

When you see:

```ts id="u9kizf"
function test<T>(arg: T): T;
```

Read:

> whatever goes in comes out same type

---

---

# Final mental model

Generic = **type variable**

Like:

```ts id="e4k6n5"
x = 5;
```

but for types:

```ts id="a5xvtt"
T = User;
```

---

So your helper:

```ts id="tm8fnd"
const ensureFound = <T>(data: T | null): T
```

really means:

> Give me any nullable type, and I’ll return the non-null version or throw.

That’s actually a very elegant backend helper.

---

## Your helper in plain English

```ts id="slr4kh"
const consultation = ensureFound(await getById(id));
```

means:

> get consultation, crash with 404 if missing, otherwise give me typed consultation.

Very clean.

---

Perfect. We’ll go layer by layer from absolute beginner to “okay now I can read backend TypeScript without feeling personally attacked by angle brackets.”

---

# 1. Before generics (hardcoded types)

Start with normal function.

```ts id="26kk7t"
function echo(value: string): string {
  return value;
}
```

Usage:

```ts id="m4k7f0"
const result = echo("hello");
```

Works.

But:

```ts id="pjm3zx"
echo(42);
```

Fails because function only accepts string.

---

## Problem

What if you want same logic for:

- string
- number
- boolean

You’d do:

```ts id="2nqjlwm"
function echoString(value: string): string {
  return value;
}

function echoNumber(value: number): number {
  return value;
}
```

This is repetitive.

Code duplication is the villain here.

---

---

# 2. First generic

Instead of hardcoding string:

```ts id="od7b5f"
function echo<T>(value: T): T {
  return value;
}
```

Now:

```ts id="sxsyww"
echo("hello");
echo(42);
echo(true);
```

All work.

---

## What is T?

`T` is just a variable for a type.

Like:

```ts id="wm4rsh"
let age = 10;
```

Here:

- variable = age
- value = 10

---

Generic:

```ts id="awhx3f"
<T>
```

means:

- type variable = T

---

So:

```ts id="kljlwm"
function echo<T>(value: T): T;
```

means:

- receive type T
- return same type T

---

Think:

```ts id="o0mj9t"
T = string;
```

or

```ts id="wzmjrn"
T = number;
```

depending on what user passes.

---

---

# 3. Type inference

Call:

```ts id="vzj0pr"
echo("doctor");
```

TypeScript automatically infers:

```ts id="m7j8md"
T = string;
```

So internally:

```ts id="ep04g4"
function echo(value: string): string;
```

---

Call:

```ts id="rjymys"
echo(99);
```

Now:

```ts id="1vszdh"
T = number;
```

---

You usually don’t manually specify T.

But can:

```ts id="70v6vu"
echo<string>("doctor");
```

Same thing.

---

---

# 4. More examples

## Number

```ts id="4z1jlwm"
const age = echo(20);
```

Type:

```ts id="5lm4xg"
number;
```

---

## Boolean

```ts id="z9tr4x"
const online = echo(true);
```

Type:

```ts id="rk8c3v"
boolean;
```

---

## Object

```ts id="gr9du7"
const user = echo({
  id: "1",
  name: "John",
});
```

Type:

```ts id="njlwm8"
{
  id: string;
  name: string;
}
```

Preserved automatically.

This is why generics are powerful.

---

---

# 5. Generic arrays

Instead of:

```ts id="pn7lko"
function first(items: string[]): string {
  return items[0];
}
```

Only strings.

---

Generic version:

```ts id="7wvgoz"
function first<T>(items: T[]): T {
  return items[0];
}
```

Usage:

```ts id="2ix2l0"
first(["a", "b"]); // string
first([1, 2, 3]); // number
```

---

Explanation:

```ts id="kxslvz"
T[]
```

means:

array of T.

---

Examples:

```ts id="fg0azt"
string[]
number[]
User[]
```

---

---

# 6. Multiple generics

Sometimes one type isn’t enough.

---

Example:

```ts id="kjecux"
function pair<T, U>(first: T, second: U) {
  return { first, second };
}
```

Usage:

```ts id="hjlwmn"
const result = pair("doctor", 1);
```

TypeScript infers:

```ts id="u5z40h"
T = string;
U = number;
```

Result:

```ts id="ehwjlwm"
{
  first: string;
  second: number;
}
```

---

Another:

```ts id="h1x4o7"
pair(true, { id: "1" });
```

Works too.

---

---

# 7. Generic type alias

Instead of function, use generics in types.

---

Example:

```ts id="18jsu4"
type ApiResponse<T> = {
  success: boolean;
  data: T;
};
```

Usage:

```ts id="0mdu6x"
type UserResponse = ApiResponse<User>;
```

Now becomes:

```ts id="djlwm3"
{
  success: boolean;
  data: User;
}
```

---

Another:

```ts id="r1t7h4"
type MessageResponse = ApiResponse<Message>;
```

Becomes:

```ts id="o4j6sv"
{
  success: boolean;
  data: Message;
}
```

---

Very common in backend.

---

---

# 8. Generic interface

```ts id="d7bqce"
interface Box<T> {
  value: T;
}
```

Usage:

```ts id="5tjlwm"
const box: Box<string> = {
  value: "hello",
};
```

---

Or:

```ts id="8n6x0x"
const ageBox: Box<number> = {
  value: 25,
};
```

---

---

# 9. Generic constraints (intermediate)

Sometimes any type is too loose.

Example:

```ts id="7o8r83"
function printLength<T>(item: T) {
  console.log(item.length);
}
```

Problem:

Not all types have length.

```ts id="ysjlwm"
printLength(42);
```

Boom.

---

Fix:

```ts id="v9w9n4"
function printLength<T extends { length: number }>(item: T) {
  console.log(item.length);
}
```

Now allowed:

```ts id="zj85z6"
printLength("hello");
printLength([1, 2, 3]);
```

Not allowed:

```ts id="jlwm1h"
printLength(42);
```

---

Meaning:

```ts id="4j7bgx"
T extends { length: number }
```

means:

T can be anything with length.

---

---

# 10. Constraint with backend objects

Example:

```ts id="jvjlwm"
function getId<T extends { id: string }>(item: T) {
  return item.id;
}
```

Usage:

```ts id="1k9u17"
getId({
  id: "123",
  name: "John",
});
```

Works.

---

Fails:

```ts id="48psjq"
getId({
  name: "John",
});
```

No id.

---

---

# 11. Your helper (advanced practical)

Now your code makes sense:

```ts id="6ksjlwm"
const ensureFound = <T>(data: T | null, message = "Not found"): T => {
  if (!data) {
    throw new Error(message);
  }

  return data;
};
```

---

## Input

```ts id="gbpkj7"
T | null;
```

means:

- maybe T
- maybe null

---

## Output

```ts id="jlwm5z"
T;
```

means:

guaranteed non-null.

---

Example:

```ts id="l0s5pq"
const user = ensureFound(await getUser());
```

If:

```ts id="2ww9vf"
User | null;
```

goes in

Then:

```ts id="jlwmhr"
User;
```

comes out.

---

Magic:

```ts id="h0u0p6"
null removed
```

---

---

# 12. Real backend examples

---

## Pagination

```ts id="7njlwm"
type Pagination<T> = {
  items: T[];
  total: number;
};
```

Usage:

```ts id="u93hzi"
Pagination<User>;
Pagination<Message>;
```

---

---

## Repository pattern

```ts id="t8s2gm"
class Repository<T> {
  findById(id: string): Promise<T | null> {
    // logic
  }
}
```

Usage:

```ts id="jlwmvf"
Repository<User>;
Repository<Consultation>;
```

---

---

# 13. Expert mental model

Generic is simply:

> variable but for types

Normal variable:

```ts id="frk03r"
let x = 5;
```

Type variable:

```ts id="jlwmz4"
T = User;
```

---

So:

```ts id="2d4pt0"
function test<T>(arg: T): T;
```

means:

> whatever type enters leaves unchanged.

---

---

# Cheat sheet

## One type

```ts id="jlwm0k"
<T>
```

---

## Two types

```ts id="jlwmv8"
<T, U>
```

---

## Array

```ts id="zclb8u"
T[]
```

---

## Constraint

```ts id="jlwm70"
<T extends Something>
```

---

## Nullable input

```ts id="jlwm8a"
T | null;
```

---

## Return guaranteed T

```ts id="jlwm2x"
(): T
```

---

---

# Final progression

You learned:

### Beginner

```ts id="jlwm5i"
function echo<T>(value: T): T;
```

---

### Intermediate

```ts id="jlwmya"
function first<T>(items: T[]): T;
```

---

### Advanced

```ts id="jlwm4s"
type ApiResponse<T>
```

---

### Expert practical backend

```ts id="jlwmrh"
const ensureFound = <T>(data: T | null): T
```

---

Now when you see:

```ts id="jlwmzq"
<T extends { id: string }>
```

you shouldn’t panic anymore 😄

It just means:

> some type with an id field.
