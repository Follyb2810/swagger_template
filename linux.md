# 🐧 1. Basic Navigation (START HERE)

## 📂 Where am I?

```bash
pwd
```

## 📁 List files

```bash
ls
ls -l        # detailed
ls -a        # hidden files
ls -lh       # human readable sizes
```

## 📁 Change directory

```bash
cd folder
cd ..        # go back
cd ~         # home directory
cd /         # root
```

---

# 📄 2. File & Folder Management

## Create

```bash
touch file.txt
mkdir folder
mkdir -p a/b/c   # nested folders
```

## Copy

```bash
cp file.txt copy.txt
cp -r folder folder-copy
```

## Move / Rename

```bash
mv file.txt new.txt
mv file.txt folder/
```

## Delete ⚠️

```bash
rm file.txt
rm -r folder
rm -rf folder   # force delete (danger ⚠️)
```

---

# 📖 3. Viewing Files

```bash
cat file.txt
less file.txt
head file.txt
tail file.txt
tail -f logs.txt   # live logs 🔥
```

---

# ✍️ 4. Editing Files

## Nano (easy)

```bash
nano file.txt
```

## Vim (advanced)

```bash
vim file.txt
```

---

# 🔍 5. Searching

## Find file

```bash
find . -name "file.txt"
```

## Search text inside files

```bash
grep "hello" file.txt
grep -r "hello" .
```

---

# 📦 6. Package Management (Ubuntu/Debian)

```bash
sudo apt update
sudo apt upgrade
sudo apt install git
sudo apt remove git
```

---

# 🌐 7. Networking

## Check IP

```bash
ip a
```

## Ping

```bash
ping google.com
```

## Curl (API requests 🔥)

```bash
curl https://api.example.com
```

## Download file

```bash
wget https://file.com/file.zip
```

---

# 🔐 8. Permissions (VERY IMPORTANT)

## View permissions

```bash
ls -l
```

## Change permission

```bash
chmod 755 file.sh
```

## Change owner

```bash
chown user:user file.txt
```

---

# 👤 9. Users & Sudo

```bash
whoami
sudo command
adduser newuser
```

---

# ⚙️ 10. Processes

## View processes

```bash
ps aux
top
htop   # better UI (install first)
```

## Kill process

```bash
kill PID
kill -9 PID
```

---

# 💾 11. Disk & Storage

```bash
df -h        # disk usage
du -sh *     # folder sizes
```

---

# 📦 12. Archive & Compress

```bash
tar -cvf file.tar folder/
tar -xvf file.tar

zip file.zip file.txt
unzip file.zip
```

---

# 🔄 13. Environment Variables

```bash
export NAME=value
echo $NAME
```

---

# 🔗 14. Pipes & Redirection (POWERFUL)

## Redirect output

```bash
echo "hello" > file.txt
```

## Append

```bash
echo "more" >> file.txt
```

## Pipe

```bash
cat file.txt | grep hello
```

---

# 🧠 15. Useful Shortcuts

```bash
CTRL + C   # stop process
CTRL + Z   # pause
CTRL + L   # clear screen
TAB        # auto-complete
```

---

# 🔥 16. Advanced Commands

## Watch command (live)

```bash
watch -n 1 ls
```

## xargs

```bash
cat file.txt | xargs rm
```

## Alias

```bash
alias ll="ls -la"
```

---

# 🐳 17. DevOps Commands (YOU NEED THIS)

## Git

```bash
git clone repo
git status
git add .
git commit -m "msg"
git push
```

## Node

```bash
node -v
npm install
npm run dev
```

## Docker

```bash
docker ps
docker build .
docker run image
```

---

# 🔐 18. SSH (Remote Server)

```bash
ssh user@ip
```

Copy file:

```bash
scp file.txt user@ip:/path
```

---

# 📡 19. Logs & Monitoring

```bash
tail -f /var/log/syslog
```

---

# ⚡ 20. Power User Tricks

## Run in background

```bash
node app.js &
```

## Keep running after logout

```bash
nohup node app.js &
```

---

# 🧩 21. Real Developer Workflow (Your Case)

```bash
# Navigate project
cd zealy-api

# Install deps
npm install

# Run server
npm run dev

# Watch logs
tail -f logs.txt

# Git push
git add .
git commit -m "update"
git push
```

---

# ⚠️ 22. Dangerous Commands (Be Careful)

```bash
rm -rf /
```

👉 NEVER run this (deletes everything)

---

# 🧠 23. Mental Model

| Command Type | Example     |
| ------------ | ----------- |
| Navigation   | cd, ls      |
| Files        | cp, mv, rm  |
| System       | ps, top     |
| Network      | curl, ping  |
| Dev          | git, docker |

---

# 🚀 If you want next level

I can teach you:

- Linux for **backend engineers (real-world)**
- Setup **production server (NGINX + Docker + SSL)**
- Build **CI/CD pipelines**
- Advanced bash scripting

Just say 👍
