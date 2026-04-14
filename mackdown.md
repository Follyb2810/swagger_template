---

# 📘 1. What is Markdown?

Markdown is a **lightweight text format** used to write:

- GitHub README files
- API docs (Swagger/OpenAPI comments)
- Blogs
- Documentation

It converts plain text → nicely formatted HTML.

---

# ✍️ 2. Basic Markdown Syntax

## 🟢 Headings

```md
# H1 (Main title)

## H2

### H3

#### H4
```

---

## 🟢 Bold / Italic

```md
**bold text**
_italic text_
```

---

## 🟢 Lists

### Bullet list

```md
- Item 1
- Item 2
  - Sub item
```

### Numbered list

```md
1. First
2. Second
3. Third
```

---

## 🟢 Code

### Inline code

```md
Use `npm install` to install packages
```

### Code block

````md
```ts
function hello() {
  console.log("Hello World");
}
```
````

---

## 🟢 Links

```md
[Google](https://google.com)
```

---

## 🟢 Images

```md
![alt text](https://example.com/image.png)
```

---

## 🟢 Blockquote

```md
> This is a note or warning
```

---

## 🟢 Horizontal line

```md
---
```

---

# 🚀 3. Intermediate Markdown (GitHub level)

## 🟡 Tables (VERY IMPORTANT for APIs)

```md
| Field | Type   | Required | Description |
| ----- | ------ | -------- | ----------- |
| name  | string | yes      | Quest name  |
| xp    | number | no       | Reward XP   |
```

---

## 🟡 Task Lists (Checkboxes)

```md
- [x] Setup project
- [ ] Create API
- [ ] Deploy server
```

---

## 🟡 Emojis (GitHub friendly)

```md
🚀 Feature
🐛 Bug fix
⚡ Performance
🔥 Important
```

---

# 🧠 4. Advanced Markdown (Used in Swagger + Docs)

## 🟣 Collapsible Sections (GitHub)

```md
<details>
<summary>Click to expand</summary>

Hidden content here.

</details>
```

---

## 🟣 Alerts / Callouts (GitHub style)

```md
> ⚠️ Warning: This action is irreversible
```

---

## 🟣 API Documentation Style (VERY IMPORTANT)

````md
## POST /api/quest

### Request Body

```json
{
  "name": "Twitter Follow",
  "xp": 100,
  "subdomain": "my-community"
}
```
````

### Response

```json
{
  "success": true
}
```

````

---

# 📡 5. Swagger-style Markdown (What you are building)

This is how AI + devs write API docs:

```md
## Create Quest API

### Endpoint
`POST /api/zealy/quests`

### Description
Creates a quest using simplified payload.

---

### Request Body
| Field | Type | Required | Description |
|------|------|----------|-------------|
| subdomain | string | yes | community name |
| name | string | yes | quest name |
| rewardXP | number | no | default XP reward |

---

### Example
```json
{
  "subdomain": "my-community",
  "name": "Follow Twitter",
  "rewardXP": 50
}
````

---

### Response

```json
{
  "success": true,
  "data": {}
}
```

````

---

# ⚡ 6. Pro Tips (What AI + engineers do)

### ✔ Keep structure consistent
- Always: Title → Description → Request → Response

### ✔ Use tables for inputs
- Makes Swagger clean

### ✔ Use short sentences
- Docs should be scannable

### ✔ Always include examples
- This is the MOST important part

---

# 🧩 7. Real-world usage (your case)

For your Zealy project, your MD should always follow:

```md
## Feature Name

### Endpoint
### Method
### Auth
### Request Body
### Example Request
### Example Response
### Errors
````

---
