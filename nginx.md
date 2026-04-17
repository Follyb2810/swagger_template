# Docker-style / internal proxy

```bash
## nginx.conf
events {}

http {
    server {
        listen 80;

        location /socket.io/ {
            proxy_pass http://app:8400;

            proxy_http_version 1.1;
            proxy_set_header Upgrade $http_upgrade;
            proxy_set_header Connection "Upgrade";

            proxy_set_header Host $host;
            proxy_set_header X-Forwarded-Proto $scheme;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;

            proxy_cache_bypass $http_upgrade;
        }

        location / {
            proxy_pass http://app:8400;

            proxy_http_version 1.1;
            proxy_set_header Upgrade $http_upgrade;
            proxy_set_header Connection "Upgrade";

            proxy_set_header Host $host;
            proxy_set_header X-Forwarded-Proto $scheme;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        }
    }
}

```

# Server config

```bash
server {
    server_name follyb.com;

    location /socket.io/ {
        proxy_pass http://localhost:5000;
        ...
    }

    location / {
        proxy_pass http://localhost:5000;
        ...
    }
    location /socket.io/ {
        proxy_pass http://localhost:5000;

        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "Upgrade";

        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;

        proxy_cache_bypass $http_upgrade;
    }

    location / {
        proxy_pass http://localhost:5000;

        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "Upgrade";

        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }
}
```

## Structure of Nginx

```bash
events {}

http {
    server {
        location / {
            # your routing
        }
    }
}
```

| Block         | What it is                  |
| ------------- | --------------------------- |
| `events {}`   | low-level stuff (ignore it) |
| `http {}`     | global HTTP config          |
| `server {}`   | one website/domain          |
| `location {}` | routes inside that domain   |
