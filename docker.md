# 🐳 1. What is Docker (Simple Explanation)

Docker lets you:

- Package your app + dependencies
- Run it anywhere (same environment)
- Avoid “it works on my machine” problems

👉 Think of it as a **portable mini-server**

---

# 📦 2. Core Concepts (Must Know)

| Term       | Meaning                          |
| ---------- | -------------------------------- |
| Image      | Blueprint of your app            |
| Container  | Running instance of image        |
| Dockerfile | Instructions to build image      |
| Docker Hub | Like npm but for images          |
| Volume     | Persistent storage               |
| Network    | Communication between containers |

---

# 🚀 3. Install Docker

Download:
👉 [https://www.docker.com/products/docker-desktop](https://www.docker.com/products/docker-desktop)

Check:

```bash
docker --version
```

---

# 🧱 4. Your First Dockerfile (Node.js)

```dockerfile
# Use Node base image
FROM node:18

# Set working directory
WORKDIR /app

# Copy package files
COPY package*.json ./

# Install dependencies
RUN npm install

# Copy all files
COPY . .

# Expose port
EXPOSE 3000

# Start app
CMD ["npm", "run", "dev"]
```

---

# ▶️ 5. Build & Run Container

### Build image

```bash
docker build -t my-app .
```

### Run container

```bash
docker run -p 3000:3000 my-app
```

👉 Now your app runs on:

```
http://localhost:3000
```

---

# 🔁 6. Auto Reload (Dev Mode)

Use volume so changes reflect instantly:

```bash
docker run -p 3000:3000 -v $(pwd):/app my-app
```

---

# 📂 7. .dockerignore (VERY IMPORTANT)

```bash
node_modules
.git
.env
```

👉 Prevents unnecessary files from being copied

---

# 🧪 8. Docker Commands (Daily Use)

```bash
docker ps              # running containers
docker ps -a           # all containers
docker images          # list images
docker stop <id>       # stop container
docker rm <id>         # delete container
docker rmi <id>        # delete image
docker logs <id>       # logs
```

---

# 🧩 9. Docker Compose (Multi-service setup)

Used when you have:

- Backend
- Database
- Redis

---

## Example: docker-compose.yml

```yaml
version: "3.9"

services:
  app:
    build: .
    ports:
      - "3000:3000"
    volumes:
      - .:/app
    depends_on:
      - redis

  redis:
    image: redis:alpine
    ports:
      - "6379:6379"
```

---

### Run everything

```bash
docker-compose up
```

---

# 🗄️ 10. Add Database (PostgreSQL)

```yaml
db:
  image: postgres:15
  environment:
    POSTGRES_USER: user
    POSTGRES_PASSWORD: password
    POSTGRES_DB: mydb
  ports:
    - "5432:5432"
```

---

# 🔐 11. Environment Variables

Create `.env`:

```env
PORT=3000
DB_URL=postgres://user:password@db:5432/mydb
```

Use in Docker:

```yaml
env_file:
  - .env
```

---

# 🧠 12. Production Dockerfile (Optimized)

```dockerfile
FROM node:18-alpine

WORKDIR /app

COPY package*.json ./
RUN npm ci --only=production

COPY . .

EXPOSE 3000

CMD ["node", "dist/index.js"]
```

---

# ⚡ 13. Multi-stage Build (Advanced)

```dockerfile
# Build stage
FROM node:18 AS builder
WORKDIR /app
COPY . .
RUN npm install && npm run build

# Production stage
FROM node:18-alpine
WORKDIR /app
COPY --from=builder /app/dist ./dist
COPY package*.json ./
RUN npm ci --only=production

CMD ["node", "dist/index.js"]
```

👉 Reduces image size 🔥

---

# 🌍 14. Push to Docker Hub

### Login

```bash
docker login
```

### Tag

```bash
docker tag my-app yourusername/my-app
```

### Push

```bash
docker push yourusername/my-app
```

---

# 🚢 15. Deploy Anywhere

Run on server:

```bash
docker pull yourusername/my-app
docker run -d -p 3000:3000 yourusername/my-app
```

---

# 🧪 16. Debugging Tips

```bash
docker exec -it <container_id> sh
```

👉 Access container shell

---

# ⚠️ 17. Common Mistakes

❌ Forgetting `.dockerignore`
❌ Using `npm install` instead of `npm ci`
❌ Not exposing correct port
❌ Hardcoding environment variables

---

# 🧩 18. Real Setup for YOUR Project

For your Zealy API:

### docker-compose.yml

```yaml
version: "3.9"

services:
  api:
    build: .
    ports:
      - "3000:3000"
    env_file:
      - .env
    volumes:
      - .:/app
```

---

# 🧠 19. Mental Model (Important)

| Without Docker        | With Docker   |
| --------------------- | ------------- |
| Install Node manually | Pre-installed |
| Setup DB manually     | Container     |
| Version issues        | Fixed         |
| Hard to deploy        | Easy          |

---

# 🏁 20. When to Use Docker

Use Docker when:
✅ Building APIs (like yours)
✅ Working with teams
✅ Deploying apps
✅ Using multiple services

Good — now you’re stepping into **real multi-stack Docker usage**. I’ll show you:

1. Docker for **.NET**
2. Docker for **Python**
3. What `pwd` means (and why you used it earlier)

---

# 🧱 1. Docker for .NET (ASP.NET Core)

## 📄 Basic Dockerfile (.NET 7/8)

```dockerfile
# Build stage
FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
WORKDIR /app

COPY *.csproj ./
RUN dotnet restore

COPY . ./
RUN dotnet publish -c Release -o out

# Runtime stage
FROM mcr.microsoft.com/dotnet/aspnet:8.0
WORKDIR /app

COPY --from=build /app/out .

EXPOSE 5000

ENTRYPOINT ["dotnet", "YourApp.dll"]
```

👉 Replace:

```bash
YourApp.dll
```

with your actual project DLL name.

---

## ▶️ Run .NET container

```bash
docker build -t dotnet-app .
docker run -p 5000:5000 dotnet-app
```

---

## ⚡ Dev Mode (Hot Reload)

```bash
docker run -p 5000:5000 -v $(pwd):/app dotnet-app
```

---

# 🐍 2. Docker for Python (Flask / FastAPI)

## 📄 Dockerfile (Python)

```dockerfile
FROM python:3.11

WORKDIR /app

COPY requirements.txt .
RUN pip install -r requirements.txt

COPY . .

EXPOSE 8000

CMD ["python", "app.py"]
```

---

## ▶️ Run Python container

```bash
docker build -t python-app .
docker run -p 8000:8000 python-app
```

---

## ⚡ FastAPI Example

```dockerfile
CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "8000"]
```

---

# 📂 3. What is `pwd` (VERY IMPORTANT)

## 🧠 Meaning

```bash
pwd
```

👉 Stands for:

> **Print Working Directory**

It shows your current folder path.

---

## 📌 Example

```bash
pwd
```

Output:

```bash
/Users/john/projects/my-app
```

---

## 🔥 Why it's used in Docker

When you do:

```bash
docker run -v $(pwd):/app my-app
```

👉 It means:

| Part     | Meaning             |
| -------- | ------------------- |
| `$(pwd)` | your current folder |
| `/app`   | container folder    |
| `-v`     | mount (link them)   |

---

## 🧩 So basically:

```bash
$(pwd):/app
```

=
👉 “Use my current project folder inside the container”

---

## 🧠 Real Example

If you're in:

```bash
/home/user/zealy-api
```

Then:

```bash
docker run -v $(pwd):/app my-app
```

means:

```bash
/home/user/zealy-api → /app (inside container)
```

---

# ⚠️ 4. Windows Users Note

Use:

```bash
${PWD}
```

or in PowerShell:

```bash
${PWD}
```

---

# 🧠 5. When to Use `pwd`

Use it when:

- You want **live code updates**
- You’re in **development**
- You don’t want to rebuild image every time

---

# ⚡ 6. Quick Comparison

| Stack  | Port | Run Command               |
| ------ | ---- | ------------------------- |
| Node   | 3000 | `docker run -p 3000:3000` |
| .NET   | 5000 | `docker run -p 5000:5000` |
| Python | 8000 | `docker run -p 8000:8000` |

---

# 🧩 7. Pro Tip (Important)

### ❌ Don't use volume in production

```bash
-v $(pwd):/app
```

### ✅ Use only built image in production

```bash
docker run my-app
```

---

# 🐳 1. Docker Networking (VERY IMPORTANT)

## 🔹 Types of networks

| Type   | Use                                  |
| ------ | ------------------------------------ |
| bridge | default (containers talk internally) |
| host   | use host network                     |
| none   | no network                           |

---

## 🔹 List networks

```bash
docker network ls
```

---

## 🔹 Create custom network

```bash
docker network create my-network
```

---

## 🔹 Run container inside network

```bash
docker run -d --name app --network my-network my-app
```

---

## 🔹 Connect multiple containers

```bash
docker run -d --name api --network my-network my-api
docker run -d --name redis --network my-network redis
```

👉 Now they can talk using:

```bash
redis:6379
```

---

## 🔥 Test connection (ping inside container)

```bash
docker exec -it api sh
ping redis
```

👉 If it works → network is correct

---

# 🌐 2. Docker Compose Networking (Auto)

```yaml
version: "3.9"

services:
  api:
    build: .
    depends_on:
      - redis

  redis:
    image: redis
```

👉 You can access Redis with:

```bash
redis:6379
```

No manual network needed ✅

---

# 💾 3. Volumes (Persistent Data)

## 🔹 Why volumes?

Without volumes:
👉 Data is LOST when container stops

---

## 🔹 Create volume

```bash
docker volume create my-volume
```

---

## 🔹 Use volume

```bash
docker run -v my-volume:/data my-app
```

---

## 🔹 Bind mount (your system folder)

```bash
docker run -v $(pwd):/app my-app
```

👉 You already used this ✔️

---

## 🔹 Example (Postgres with volume)

```bash
docker run -d \
  -v pgdata:/var/lib/postgresql/data \
  postgres
```

👉 DB data persists 🔥

---

# 🧱 4. Docker Image Deep Understanding

## 🔹 What is an image?

Image = layers

```bash
docker history my-app
```

---

## 🔹 Inspect image

```bash
docker inspect my-app
```

---

## 🔹 Tagging images

```bash
docker tag my-app myname/my-app:v1
```

---

## 🔹 Save image to file

```bash
docker save my-app > my-app.tar
```

## 🔹 Load image

```bash
docker load < my-app.tar
```

---

# 🏗️ 5. Advanced Build

## 🔹 Build with name + tag

```bash
docker build -t my-app:v1 .
```

---

## 🔹 Build without cache

```bash
docker build --no-cache -t my-app .
```

---

## 🔹 Build specific stage

```bash
docker build --target builder -t my-app .
```

---

# 🧪 6. Debugging Containers

## 🔹 Enter container

```bash
docker exec -it container_id sh
```

---

## 🔹 Check logs

```bash
docker logs container_id
```

---

## 🔹 Inspect running container

```bash
docker inspect container_id
```

---

# 🌐 7. Ping, Curl & Networking Debug

Inside container:

```bash
ping google.com
```

If ping not available:

```bash
apk add iputils      # Alpine
apt update && apt install iputils-ping -y
```

---

## 🔹 Test API inside container

```bash
curl http://redis:6379
```

---

# 🔐 8. Port Mapping (IMPORTANT)

```bash
docker run -p 3000:3000 my-app
```

| Host | Container |
| ---- | --------- |
| 3000 | 3000      |

---

## 🔹 Multiple ports

```bash
docker run -p 3000:3000 -p 9229:9229 my-app
```

---

# 🧩 9. Environment Variables

```bash
docker run -e PORT=3000 my-app
```

---

## With file:

```bash
docker run --env-file .env my-app
```

---

# 🧱 10. Full Real Setup (Your Backend)

```yaml
version: "3.9"

services:
  api:
    build: .
    container_name: zealy-api
    ports:
      - "3000:3000"
    env_file:
      - .env
    depends_on:
      - redis
    networks:
      - app-net

  redis:
    image: redis:alpine
    container_name: redis
    volumes:
      - redis-data:/data
    networks:
      - app-net

volumes:
  redis-data:

networks:
  app-net:
```

---

# 🔥 11. Run Everything

```bash
docker-compose up -d
```

---

# 🧠 12. Health Check (Advanced)

```dockerfile
HEALTHCHECK CMD curl --fail http://localhost:3000 || exit 1
```

---

# 📡 13. Restart Policies

```yaml
restart: always
```

---

# ⚡ 14. Cleanup (IMPORTANT)

```bash
docker system prune
```

Remove everything:

```bash
docker system prune -a
```

---

# 🧠 15. Real Debug Flow (IMPORTANT)

When something breaks:

1. Check container running:

```bash
docker ps
```

2. Check logs:

```bash
docker logs container
```

3. Enter container:

```bash
docker exec -it container sh
```

4. Ping service:

```bash
ping redis
```

5. Curl API:

```bash
curl http://localhost:3000
```

---

# 🧩 16. Mental Model (FINAL)

| Concept   | Meaning         |
| --------- | --------------- |
| Image     | Blueprint       |
| Container | Running app     |
| Volume    | Storage         |
| Network   | Communication   |
| Port      | Access          |
| Compose   | Multi-app setup |
