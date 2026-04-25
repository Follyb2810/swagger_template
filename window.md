# 🪟 Windows CMD (Command Prompt) Guide

---

# 📘 1. Basic Navigation (Beginner)

### 📂 Show current directory

```bat
cd
```

---

### 📁 List files

```bat
dir
```

---

### 📂 Change directory

```bat
cd foldername
```

Go back:

```bat
cd ..
```

---

### 🧱 Create folder

```bat
mkdir myfolder
```

---

### ❌ Delete folder

```bat
rmdir myfolder
```

---

### 📄 Create file

```bat
echo Hello > file.txt
```

---

# 📄 2. File Management

### Copy file

```bat
copy file.txt backup.txt
```

---

### Move / rename

```bat
move file.txt newfile.txt
```

---

### Delete file

```bat
del file.txt
```

---

### View file content

```bat
type file.txt
```

---

# 🔍 3. Searching & Filtering

### Find text in file

```bat
find "hello" file.txt
```

---

### Advanced search

```bat
findstr "error" log.txt
```

---

# 🌐 4. Networking Commands

### Show IP

```bat
ipconfig
```

---

### Detailed network info

```bat
ipconfig /all
```

---

### Test connection

```bat
ping google.com
```

---

### Trace route

```bat
tracert google.com
```

---

### Check open ports

```bat
netstat -ano
```

---

# ⚙️ 5. System Commands

### Show running processes

```bat
tasklist
```

---

### Kill process

```bat
taskkill /PID 1234 /F
```

---

### System info

```bat
systeminfo
```

---

# 🔐 6. User & Permissions

### Show users

```bat
net user
```

---

### Add user

```bat
net user username password /add
```

---

### Add to admin

```bat
net localgroup administrators username /add
```

---

# 📦 7. Disk & Storage

### Check disk

```bat
chkdsk
```

---

### Format disk

```bat
format D:
```

---

### Show disk usage

```bat
dir
```

---

# 🔄 8. Environment Variables

### Show variables

```bat
set
```

---

### Set variable

```bat
set MY_VAR=hello
```

---

### Permanent (advanced)

```bat
setx MY_VAR "hello"
```

---

# 🧪 9. Batch Scripting (Automation)

Create `.bat` file:

```bat
@echo off
echo Hello World
pause
```

---

### Variables

```bat
set name=John
echo %name%
```

---

### Loop

```bat
for %%i in (*.txt) do echo %%i
```

---

# ⚡ 10. Advanced Commands

### Run command as admin

```bat
runas /user:Administrator cmd
```

---

### Shutdown system

```bat
shutdown /s /t 0
```

Restart:

```bat
shutdown /r /t 0
```

---

### Schedule task

```bat
schtasks /create /tn "MyTask" /tr "script.bat" /sc daily
```

---

# 🔍 11. Pipes & Redirection (Powerful)

### Redirect output

```bat
dir > files.txt
```

Append:

```bat
dir >> files.txt
```

---

### Pipe commands

```bat
dir | find "txt"
```

---

# 🧠 12. Advanced Networking

### Flush DNS

```bat
ipconfig /flushdns
```

---

### Release/Renew IP

```bat
ipconfig /release
ipconfig /renew
```

---

### Test port

```bat
telnet google.com 80
```

---

# 🧱 13. File Permissions

```bat
icacls file.txt
```

Grant permission:

```bat
icacls file.txt /grant username:F
```

---

# 🧠 14. Power User Tricks

### Open CMD in folder

```bat
cd /d D:\Projects
```

---

### Clear screen

```bat
cls
```

---

### Command history

```bat
doskey /history
```

---

# 🚨 15. Dangerous Commands (Be Careful ⚠️)

```bat
del /s /q *
```

👉 deletes everything in folder

---

```bat
format C:
```

👉 wipes drive

---

# 🎯 16. Real-World Use Cases

### 🔹 Find large files

```bat
dir /s
```

---

### 🔹 Monitor network

```bat
netstat -ano
```

---

### 🔹 Kill app by name

```bat
taskkill /IM chrome.exe /F
```

---

# 📊 17. CMD vs PowerShell

CMD is:

- simpler
- faster for basic tasks

PowerShell is:

- more powerful
- scripting-heavy

---

# 🧠 Final Learning Path

### Beginner

- cd, dir, mkdir

### Intermediate

- networking, processes

### Advanced

- scripting, pipes

### Expert

- automation, system management

---
