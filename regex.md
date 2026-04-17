# 🧠 1. What is Regex (in plain English)

Regex = a pattern used to **match text**

Example:

- Find emails
- Validate phone numbers
- Extract IDs from strings

---

# 🔤 2. Basic Building Blocks

### Characters

```
a       → matches "a"
abc     → matches "abc"
```

### Special Characters

```
.       → any character
\d      → digit (0-9)
\w      → word (a-z, A-Z, 0-9, _)
\s      → space
```

---

# 🔢 3. Quantifiers (VERY IMPORTANT)

```
a*      → 0 or more "a"
a+      → 1 or more "a"
a?      → optional (0 or 1)

a{3}    → exactly 3
a{2,5}  → between 2 and 5
```

Example:

```
\d{3} → matches "123"
```

---

# 🎯 4. Anchors (Start & End)

```
^       → start of string
$       → end of string
```

Example:

```
^\d{11}$ → exactly 11 digits (phone number)
```

---

# 📦 5. Character Sets

```
[abc]      → a or b or c
[a-z]      → lowercase letters
[A-Z]      → uppercase
[0-9]      → digits
```

Example:

```
[a-zA-Z0-9] → alphanumeric
```

---

# 🚫 6. Negation

```
[^a-z] → NOT lowercase letters
```

---

# 🔀 7. OR Operator

```
cat|dog
```

Matches:

- "cat"
- "dog"

---

# 🧩 8. Groups

```
(abc)
```

Used for:

- grouping
- extracting values

Example:

```
(\d{3})-(\d{4})
```

Matches:

```
123-4567
```

---

# 🔁 9. Real Examples (Backend Ready)

---

## ✅ Email Validation

```
^[\w.-]+@[\w.-]+\.[a-zA-Z]{2,}$
```

✔ matches:

```
test@gmail.com
user123@mail.co
```

---

## 📱 Phone Number (Nigeria style)

```
^0\d{10}$
```

✔ matches:

```
08012345678
```

---

## 🔐 Strong Password

```
^(?=.*[A-Z])(?=.*\d).{8,}$
```

✔ must contain:

- 1 uppercase
- 1 number
- 8+ chars

---

## 🌐 URL

```
https?:\/\/[^\s]+
```

---

## 🆔 Extract ID from string

```
user_(\d+)
```

From:

```
user_12345
```

→ Extracts `12345`

---

# ⚡ 10. Lookaheads (Advanced but powerful)

```
(?=...)   → must contain
(?!...)   → must NOT contain
```

Example:

```
^(?=.*[A-Z])(?=.*\d).+$
```

---

# 🛠️ 11. Using Regex in Code

## ✅ JavaScript / Node.js

```ts
const regex = /^\d{11}$/;

regex.test("08012345678"); // true
```

---

## ✅ Replace

```ts
"hello123".replace(/\d+/g, "");
// "hello"
```

---

## ✅ Extract

```ts
const match = "user_123".match(/user_(\d+)/);
console.log(match?.[1]); // 123
```

---

# 🚀 12. Common Backend Use Cases

### ✔ Validate DTO input

```ts
if (!/^\d{11}$/.test(phone)) {
  throw new Error("Invalid phone");
}
```

### ✔ Clean input

```ts
const clean = input.replace(/[^\w]/g, "");
```

### ✔ Parse logs / APIs

```ts
const id = log.match(/id=(\d+)/)?.[1];
```

---

# ⚠️ 13. Common Mistakes

❌ Forgetting `^` and `$`

```
\d{3} → matches inside string (bad)
```

✔ Correct:

```
^\d{3}$
```

---

❌ Using `.` without escaping

```
. → ANY character
```

✔ To match dot:

```
\.
```

---

# 🧪 14. Test Your Regex

Use:

- regex101.com (VERY IMPORTANT)
- shows explanation + matches

---

# 🧠 15. Quick Cheat Sheet

```
\d     digit
\w     word
\s     space
.      any char

^      start
$      end

*      0+
+      1+
?      optional

[]     set
[^]    not set

()     group
|      OR
```

---
