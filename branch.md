Good question — branch naming matters a lot for clean teamwork and CI/CD.

Here are **best-practice Git branch naming conventions** you should follow.

---

# 🧠 1. Standard format (MOST IMPORTANT)

```bash
type/short-description
```

---

# 🔥 2. Common branch types

| Type        | Use for                         |
| ----------- | ------------------------------- |
| `feature/`  | new features                    |
| `fix/`      | bug fixes                       |
| `chore/`    | cleanup, refactor, config       |
| `refactor/` | code restructuring              |
| `docs/`     | documentation (Swagger, README) |
| `hotfix/`   | urgent production fix           |

---

# 🚀 3. Examples for YOUR project (Zealy API)

### 🟢 Feature branches

```bash
feature/zealy-quest-create
feature/zealy-leaderboard-api
feature/community-secrets
feature/xp-system
```

---

### 🟡 Swagger / docs work (perfect for you right now)

```bash
docs/swagger-zealy-api
docs/quest-api-swagger
docs/community-endpoints
```

---

### 🔧 Fix branches

```bash
fix/leaderboard-query-bug
fix/swagger-schema-error
fix/quest-task-parser
```

---

### ♻️ Refactor branches

```bash
refactor/zealy-service-cleanup
refactor/task-builder-architecture
refactor/condition-parser
```

---

# 🧠 4. BEST naming for YOUR current work

Since you are currently working on:

- Swagger
- Zealy API
- Controllers cleanup

👉 Best branch name:

```bash
docs/zealy-api-swagger
```

or

```bash
feature/zealy-api-clean-swagger
```

or (if you're improving structure heavily)

```bash
refactor/zealy-api-architecture
```

---

# ⚡ 5. Pro-level tip (important in real teams)

If you're using Jira or tickets:

```bash
feature/ZEALY-23-quest-creation
fix/ZEALY-45-leaderboard-bug
```

---

# 🧱 6. RULES TO ALWAYS FOLLOW

✔ lowercase only
✔ use hyphens (-) not spaces
✔ keep it short but meaningful
✔ always prefix with type

