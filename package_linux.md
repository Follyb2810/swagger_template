
# 🧰 Core Linux Server Packages (Cheat Sheet)

## 🌐 1. Web Servers

### 🔹 Nginx (fast, modern)

```bash
sudo apt install nginx -y
sudo systemctl start nginx
sudo systemctl enable nginx
sudo systemctl status nginx
```

---

### 🔹 Apache HTTP Server (classic)

```bash
sudo apt install apache2 -y
sudo systemctl start apache2
sudo systemctl enable apache2
```

---

## 🗄️ 2. Databases

### 🔹 MySQL

```bash
sudo apt install mysql-server -y
sudo systemctl start mysql
sudo mysql_secure_installation
```

---

### 🔹 PostgreSQL

```bash
sudo apt install postgresql -y
sudo systemctl start postgresql
sudo -u postgres psql
```

---

### 🔹 MongoDB

```bash
sudo apt install mongodb -y
sudo systemctl start mongodb
```

---

## ⚡ 3. Caching / Message Brokers

### 🔹 Redis

```bash
sudo apt install redis-server -y
sudo systemctl start redis
redis-cli ping
```

---

### 🔹 Memcached

```bash
sudo apt install memcached -y
sudo systemctl start memcached
```

---

### 🔹 RabbitMQ

```bash
sudo apt install rabbitmq-server -y
sudo systemctl start rabbitmq-server
```

---

## 🐳 4. Containers & Virtualization

### 🔹 Docker

```bash
sudo apt install docker.io -y
sudo systemctl start docker
docker run hello-world
```

---

### 🔹 Docker Compose

```bash
sudo apt install docker-compose -y
docker-compose up
```

---

## 🔐 5. Security Tools

### 🔹 Fail2Ban

```bash
sudo apt install fail2ban -y
sudo systemctl start fail2ban
```

---

### 🔹 UFW

```bash
sudo apt install ufw -y
sudo ufw enable
sudo ufw allow 22
```

---

## 🌍 6. DNS / Networking

### 🔹 Bind9

```bash
sudo apt install bind9 -y
sudo systemctl start bind9
```

---

### 🔹 Net-tools

```bash
sudo apt install net-tools -y
netstat -tulnp
```

---

## 📡 7. File & Storage Servers

### 🔹 Samba

```bash
sudo apt install samba -y
sudo systemctl start smbd
```

---

### 🔹 NFS

```bash
sudo apt install nfs-kernel-server -y
sudo systemctl start nfs-server
```

---

## 🧪 8. Development Runtimes

### 🔹 Node.js

```bash
sudo apt install nodejs npm -y
node -v
```

---

### 🔹 Python

```bash
sudo apt install python3 python3-pip -y
python3 --version
```

---

### 🔹 OpenJDK

```bash
sudo apt install openjdk-17-jdk -y
java -version
```

---

## 🔄 9. Process Managers

### 🔹 PM2

```bash
sudo npm install -g pm2
pm2 start app.js
pm2 save
```

---

## 📊 10. Monitoring Tools

### 🔹 htop

```bash
sudo apt install htop -y
htop
```

---

### 🔹 Prometheus

```bash
sudo apt install prometheus -y
```

---

### 🔹 Grafana

```bash
sudo apt install grafana -y
sudo systemctl start grafana-server
```

---

## 🔑 11. SSH Server

### 🔹 OpenSSH

```bash
sudo apt install openssh-server -y
sudo systemctl start ssh
ssh user@server-ip
```

