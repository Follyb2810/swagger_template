# 🚀 Step 1: Create a repo in Azure

1. Go to **Azure DevOps**
2. Create a **Project**
3. Click **Repos**
4. Click **New Repository**
5. Choose **Git**
6. Copy the repo URL (it looks like this):

```text
https://dev.azure.com/your-org/your-project/_git/your-repo
```

---

# 🔗 Step 2: Connect your local project

Go to your existing project folder:

```bash
cd your-project
```

### If Git is NOT initialized yet:

```bash
git init
git add .
git commit -m "initial commit"
```

---

# 🔌 Step 3: Add Azure as remote

```bash
git remote add origin https://dev.azure.com/your-org/your-project/_git/your-repo
```

Check it:

```bash
git remote -v
```

---

# ⬆️ Step 4: Push your code

```bash
git branch -M main
git push -u origin main
```

👉 It will ask for login:

### 🔐 Authentication options:

- Use **Personal Access Token (PAT)** (recommended)
- Or sign in via browser (if prompted)

---

# 🔑 Step 5: Create Personal Access Token (PAT)

In Azure:

1. Click profile icon (top right)
2. Go to **Security**
3. Create **Personal Access Token**
4. Give permissions:
   - Code → Read & Write

Use it as your password when pushing.

---

# ✅ Step 6: Confirm it worked

Go back to Azure → Repos

👉 You should now see:

- your files
- commit history

---

# 🔀 Step 7 (optional but important): Try a branch

```bash
git checkout -b feature-test
echo "hello azure" >> test.txt
git add .
git commit -m "test branch"
git push origin feature-test
```

Then:

- Go to Azure
- Create a **Pull Request**

---

# 🧠 What just happened

You basically told Git:

```text
"My remote repo is now Azure instead of GitHub or nowhere"
```

Everything else is the same Git workflow you already know.

---

# ⚡ Common issues (watch out)

### ❌ Authentication failed

→ Use PAT instead of password

### ❌ remote already exists

```bash
git remote remove origin
git remote add origin <url>
```

### ❌ push rejected

```bash
git pull origin main --rebase
```

---
