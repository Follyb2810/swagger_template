Here’s a clean **README-style guide** you can drop directly into your project. It goes from beginner → production-ready.

---

# Coturn (TURN/STUN Server) Setup Guide

This guide explains how to install, configure, and verify a Coturn server on a Linux machine (tested on Ubuntu/Debian), including production best practices.

---

## 📌 What is Coturn?

Coturn is a TURN/STUN server used for NAT traversal, commonly required for WebRTC applications (video calls, voice, real-time data).

---

## 🚀 1. Installation (Linux)

Update packages and install Coturn:

```bash
sudo apt update
sudo apt install coturn -y
```

Verify installation:

```bash
turnserver -V
```

---

## ⚙️ 2. Basic Configuration

Main config file:

```bash
/etc/turnserver.conf
```

Minimal working configuration:

```ini
# Network
listening-port=3478
tls-listening-port=5349

# Replace with your server public IP
listening-ip=YOUR_SERVER_IP
external-ip=YOUR_SERVER_IP

# Authentication
fingerprint
lt-cred-mech

# Static user (for testing only)
user=testuser:testpassword

# Domain / Realm
realm=yourdomain.com

# Logging
log-file=/var/log/turn.log
simple-log

# Security
no-loopback-peers
no-multicast-peers
```

---

## ▶️ 3. Enable and Start Service

Enable Coturn service:

```bash
sudo nano /etc/default/coturn
```

Uncomment:

```bash
TURNSERVER_ENABLED=1
```

Start service:

```bash
sudo systemctl restart coturn
sudo systemctl enable coturn
```

Check status:

```bash
sudo systemctl status coturn
```

---

## 🔥 4. Firewall Configuration

Open required ports:

```bash
sudo ufw allow 3478
sudo ufw allow 3478/udp
sudo ufw allow 5349/tcp
sudo ufw allow 5349/udp
```

Allow relay ports (important):

```bash
sudo ufw allow 10000:20000/udp
```

> If using a cloud firewall (e.g., DigitalOcean), ensure these ports are also allowed there.

---

## 🔐 5. TLS Setup (Recommended)

Use SSL certificates (e.g., Let's Encrypt):

```ini
cert=/etc/letsencrypt/live/yourdomain.com/fullchain.pem
pkey=/etc/letsencrypt/live/yourdomain.com/privkey.pem
```

---

## 🧪 6. Testing the Server

Use a WebRTC testing tool like Trickle ICE.

### Input:

```
turn:YOUR_SERVER_IP:3478
username: testuser
password: testpassword
```

### Expected Result:

You should see:

```
relay ...
```

✔️ "relay" = TURN working
❌ No relay = something is wrong

---

## 🧰 7. Debugging

Check logs:

```bash
sudo tail -f /var/log/turn.log
```

Check service logs:

```bash
sudo journalctl -u coturn
```

---

## ⚡ 8. Common Issues

### ❌ No relay candidates

* Firewall blocking UDP
* Ports not open in cloud firewall
* Wrong external IP

### ❌ Server not starting

* Config syntax error
* Port already in use

---

## 🏗️ 9. Production Configuration

### 🔁 Use Dynamic Authentication (Recommended)

Instead of static users:

```ini
use-auth-secret
static-auth-secret=YOUR_SECRET_KEY
```

Used with backend-generated credentials.

---

### 🔒 Restrict Relay Ports

```ini
min-port=10000
max-port=20000
```

---

### 🌍 Use Domain Instead of IP

```ini
realm=turn.yourdomain.com
```

---

### 🔐 Force Secure Connections

```ini
no-udp
tls-listening-port=5349
```

---

### 📊 Performance Tuning

```ini
total-quota=100
bps-capacity=0
stale-nonce=600
```

---

### 🚫 Extra Security

```ini
no-cli
no-tcp-relay
```

---

## ☁️ 10. DigitalOcean Notes

* Ensure Droplet has a public IP
* Check **Cloud Firewalls**
* Allow:

  * 3478 (TCP/UDP)
  * 5349 (TLS)
  * Relay range (UDP)

---

## ✅ 11. Final Checklist

* [ ] Coturn installed
* [ ] Config updated with correct IP
* [ ] Ports open (server + cloud firewall)
* [ ] Service running
* [ ] Relay candidates confirmed

---

## 📚 12. Useful Commands

Restart server:

```bash
sudo systemctl restart coturn
```

View logs:

```bash
sudo tail -f /var/log/turn.log
```

---

## 🎯 Summary

If everything is correct:

* Server runs without errors
* Ports are open
* You see **relay candidates**

Then your TURN server is fully functional.

---

---

If you want, I can also:

* convert this into a **Docker-based setup**
* or add **auto-scaling / load balancing TURN setup** (useful for production apps)
