# ✍️ 1. NANO (Beginner Friendly)

👉 Best for quick edits

---

## 🟢 Open file

```bash
nano file.txt
```

---

## 🟢 Basic Controls (BOTTOM of screen shows shortcuts)

| Action   | Command    |
| -------- | ---------- |
| Save     | `CTRL + O` |
| Exit     | `CTRL + X` |
| Cut line | `CTRL + K` |
| Paste    | `CTRL + U` |
| Search   | `CTRL + W` |
| Replace  | `CTRL + \` |

---

## 🟢 Example Workflow

```bash
nano app.js
```

1. Edit file
2. Save:

```bash
CTRL + O → ENTER
```

3. Exit:

```bash
CTRL + X
```

---

## ⚡ Tip

Nano shows this at bottom:

```bash
^O Write Out   ^X Exit
```

👉 `^` = CTRL

---

# 🧠 2. VIM (Powerful but confusing at first)

👉 Used by pros, faster once learned

---

## 🟢 Open file

```bash
vim file.txt
```

---

# ⚠️ IMPORTANT: Vim Modes

| Mode    | Purpose   |
| ------- | --------- |
| Normal  | commands  |
| Insert  | typing    |
| Command | save/exit |

---

# 🟢 Basic Usage

## 1. Enter INSERT mode (to type)

```bash
i
```

Now you can type normally.

---

## 2. Exit INSERT mode

```bash
ESC
```

---

## 3. Save file

```bash
:w
```

---

## 4. Exit

```bash
:q
```

---

## 5. Save & Exit

```bash
:wq
```

---

## 6. Force exit (no save)

```bash
:q!
```

---

# 🟢 Full Example

```bash
vim app.js
```

Steps:

1. Press:

```bash
i
```

2. Type your code

3. Press:

```bash
ESC
```

4. Save & exit:

```bash
:wq
```

---

# ⚡ 3. Useful Vim Shortcuts

| Action      | Command    |
| ----------- | ---------- |
| Delete line | `dd`       |
| Copy line   | `yy`       |
| Paste       | `p`        |
| Undo        | `u`        |
| Redo        | `CTRL + r` |
| Go to line  | `:10`      |
| Search      | `/text`    |

---

# 🔍 4. Search in Vim

```bash
/text
```

Next result:

```bash
n
```

---

# ⚡ 5. Replace in Vim

```bash
:%s/old/new/g
```

---

# ⚖️ 6. Nano vs Vim

| Feature           | Nano   | Vim       |
| ----------------- | ------ | --------- |
| Easy              | ✅     | ❌        |
| Powerful          | ❌     | ✅        |
| Beginner friendly | ✅     | ❌        |
| Speed             | Medium | Very fast |

---

# 🧠 7. When to Use What

Use **nano** when:

- Quick edits
- Beginner
- Config files

Use **vim** when:

- Coding heavily
- SSH servers
- Advanced workflows

---

# 🔥 8. Real Dev Usage (Your Case)

Edit `.env`:

```bash
nano .env
```

Edit config fast:

```bash
vim docker-compose.yml
```

---

# 🚨 9. Common Mistake (VERY COMMON)

People get stuck in vim 😅

👉 Fix:

```bash
ESC → :q!
```

---
