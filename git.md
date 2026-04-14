

# 🧠 1. BASIC GIT (BEGINNER)

## 🔹 Setup

```bash
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
```

## 🔹 Start a repo

```bash
git init
```

## 🔹 Clone a repo

```bash
git clone <repo-url>
```

Example:

```bash
git clone https://github.com/user/project.git
```

---

## 🔹 Check status

```bash
git status
```

---

## 🔹 Add files

```bash
git add .
```

OR specific file:

```bash
git add index.js
```

---

## 🔹 Commit changes

```bash
git commit -m "initial commit"
```

---

## 🔹 Push to GitHub

```bash
git push origin main
```

---

## 🔹 Pull latest changes

```bash
git pull origin main
```

---

# 🌿 2. BRANCHING (VERY IMPORTANT)

## 🔹 Create branch

```bash
git branch feature/login
```

## 🔹 Switch branch

```bash
git checkout feature/login
```

OR modern way:

```bash
git switch feature/login
```

---

## 🔹 Create + switch

```bash
git checkout -b feature/login
```

---

## 🔹 List branches

```bash
git branch
```

---

## 🔹 Delete branch

```bash
git branch -d feature/login
```

Force delete:

```bash
git branch -D feature/login
```

---

# 🔄 3. MERGING

## 🔹 Merge branch into main

```bash
git checkout main
git merge feature/login
```

---

## 🔹 Abort merge (if conflict)

```bash
git merge --abort
```

---

# 🔥 4. REMOTE (GITHUB)

## 🔹 Add remote repo

```bash
git remote add origin <url>
```

## 🔹 Check remote

```bash
git remote -v
```

## 🔹 Push first time

```bash
git push -u origin main
```

---

# ⚡ 5. STASH (VERY USEFUL)

Save work temporarily:

```bash
git stash
```

Apply back:

```bash
git stash apply
```

List stashes:

```bash
git stash list
```

Drop stash:

```bash
git stash drop
```

---

# 🧹 6. CLEAN & RESET

## 🔹 Undo changes (unstaged)

```bash
git checkout -- .
```

---

## 🔹 Remove from staging

```bash
git reset HEAD file.js
```

---

## 🔹 Hard reset (danger ⚠️)

```bash
git reset --hard HEAD
```

Reset to previous commit:

```bash
git reset --hard HEAD~1
```

---

# 📜 7. HISTORY / LOGS

## 🔹 View commits

```bash
git log
```

Compact view:

```bash
git log --oneline
```

Graph view:

```bash
git log --graph --oneline --all
```

---

# 🧠 8. ADVANCED (PRO LEVEL)

## 🔥 Rebase (clean history)

```bash
git rebase main
```

Interactive rebase:

```bash
git rebase -i HEAD~3
```

---

## 🔥 Cherry-pick (pick single commit)

```bash
git cherry-pick <commit-id>
```

---

## 🔥 Fix last commit

```bash
git commit --amend
```

---

## 🔥 Force push (danger)

```bash
git push --force
```

Safer version:

```bash
git push --force-with-lease
```

---

## 🔥 Clean untracked files

```bash
git clean -f
```

Folders too:

```bash
git clean -fd
```

---

# 🌐 9. GITHUB WORKFLOW (REAL WORLD)

## Typical flow:

```bash
git checkout -b feature/quest-api

git add .
git commit -m "feat: add quest API"

git push origin feature/quest-api
```

Then:
👉 Open Pull Request on GitHub

---

# 🔥 10. BEST COMMIT STYLE (VERY IMPORTANT)

Use this format:

```bash
feat: add quest creation API
fix: leaderboard bug
refactor: clean service layer
docs: update swagger
chore: update dependencies
```

---

# 🧱 11. REAL PROJECT STRUCTURE FLOW

```bash
git checkout main
git pull origin main

git checkout -b feature/zealy-quests

# work...

git add .
git commit -m "feat: add zealy quest system"

git push origin feature/zealy-quests
```

---

# 🚀 BONUS: MOST IMPORTANT COMMANDS (MEMORIZE THESE)

```bash
git status
git add .
git commit -m ""
git push
git pull
git checkout -b
git merge
git log --oneline
git stash
```

