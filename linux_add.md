# 🐧 Linux Guide (Beginner → Advanced → Expert)

A practical, hands-on guide to mastering Linux for development, servers, and production systems.

---

# 📘 1. What is Linux?

Linux is an open-source operating system used in:

* Servers (most of the internet)
* Cloud platforms
* Dev environments
* Networking systems

Popular distributions:

* Ubuntu (beginner-friendly)
* Debian (stable servers)
* CentOS / AlmaLinux (enterprise)

---

# 🧰 2. Basic Terminal Commands (Beginner)

## 📂 File & Directory Navigation

```bash
pwd          # Show current directory
ls           # List files
ls -la       # Detailed list
cd folder    # Enter folder
cd ..        # Go back
```

---

## 📄 File Management

```bash
touch file.txt        # Create file
mkdir folder          # Create directory
rm file.txt           # Delete file
rm -r folder          # Delete folder
cp a.txt b.txt        # Copy file
mv a.txt b.txt        # Move/rename file
```

---

## 📖 Viewing Files

```bash
cat file.txt
less file.txt
head file.txt
tail file.txt
```

---

# 🔐 3. Permissions & Users

## File permissions

```bash
chmod 755 file.sh
chmod +x script.sh
```

## Ownership

```bash
chown user:user file.txt
```

---

# 📦 4. Package Management

### Ubuntu/Debian

```bash
sudo apt update
sudo apt upgrade
sudo apt install nginx
```

---

# 🌐 5. Networking Basics

```bash
ip a              # Show IP
ping google.com
curl ifconfig.me  # Get public IP
netstat -tulnp    # Open ports
```

---

# ⚙️ 6. Process Management

```bash
ps aux
top
htop
kill -9 PID
```

---

# 🧱 7. Services (Systemd)

```bash
sudo systemctl start nginx
sudo systemctl stop nginx
sudo systemctl restart nginx
sudo systemctl status nginx
sudo systemctl enable nginx
```

---

# 🔥 8. Firewall (Security Basics)

Using UFW:

```bash
sudo ufw allow 22
sudo ufw allow 80
sudo ufw enable
sudo ufw status
```

---

# 📁 9. File Editing

### Nano (easy)

```bash
nano file.txt
```

### Vim (advanced)

```bash
vim file.txt
```

---

# 🧪 10. Logs & Debugging

```bash
tail -f /var/log/syslog
journalctl -u nginx
```

---

# 🚀 11. Intermediate Skills

## 🔎 Searching

```bash
find / -name file.txt
grep "text" file.txt
```

---

## 📦 Archiving

```bash
tar -czvf file.tar.gz folder/
tar -xzvf file.tar.gz
```

---

## 🌍 SSH (Remote Access)

```bash
ssh user@server-ip
scp file.txt user@server:/path
```

---

## 🔑 SSH Key Setup

```bash
ssh-keygen
ssh-copy-id user@server-ip
```

---

# ⚡ 12. Advanced Linux

## 🧠 Resource Monitoring

```bash
htop
free -m
df -h
du -sh *
```

---

## 📊 Disk Management

```bash
lsblk
mount
umount
```

---

## 🔄 Cron Jobs (Automation)

```bash
crontab -e
```

Example:

```bash
* * * * * /path/script.sh
```

---

## 🌐 Reverse Proxy (Nginx)

Install:

```bash
sudo apt install nginx
```

Basic config:

```nginx
server {
    listen 80;
    server_name yourdomain.com;

    location / {
        proxy_pass http://localhost:3000;
    }
}
```

---

# 🔐 13. Security Hardening

## Disable root login

```bash
sudo nano /etc/ssh/sshd_config
```

Set:

```
PermitRootLogin no
```

---

## Install Fail2Ban

```bash
sudo apt install fail2ban
```

---

## Update regularly

```bash
sudo apt update && sudo apt upgrade
```

---

# 🏗️ 14. Performance Tuning (Expert)

## File limits

```bash
sudo nano /etc/security/limits.conf
```

```
* soft nofile 65535
* hard nofile 65535
```

---

## Kernel tuning

```bash
sudo nano /etc/sysctl.conf
```

```
net.core.somaxconn=65535
net.ipv4.tcp_tw_reuse=1
net.core.netdev_max_backlog=5000
```

Apply:

```bash
sudo sysctl -p
```

---

# 🐳 15. Containers (Docker)

Install:

```bash
sudo apt install docker.io
```

Run:

```bash
docker run hello-world
```

---

# ☁️ 16. Cloud Deployment Basics

Typical steps:

1. Create server (e.g., VPS)
2. SSH into server
3. Install dependencies
4. Configure firewall
5. Deploy app
6. Monitor logs

---

# 📈 17. Monitoring & Observability

Tools:

* htop (CPU/RAM)
* logs (journalctl)
* uptime

```bash
uptime
```

---

# 🧠 18. Pro Tips

* Always check logs first when something breaks
* Use `history` to track commands
* Avoid running everything as root
* Backup configs before editing

---

# ❌ 19. Common Mistakes

* Deleting wrong files with `rm -rf`
* Blocking yourself with firewall rules
* Forgetting to open ports
* Not checking logs

---

# ✅ 20. Learning Path

Beginner →

* Navigation
* File management

Intermediate →

* Networking
* Services
* SSH

Advanced →

* Security
* Performance
* Automation

Expert →

* Scaling systems
* Cloud infra
* Distributed systems

---

# 🎯 Final Summary

If you master:

* Terminal commands
* Networking
* Services
* Security

You can confidently:

* Manage servers
* Deploy applications
* Debug production systems

---
