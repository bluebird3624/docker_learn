## ✅ PHASE 1 – Part 5: **Exposing Ports & Docker Networking**

---

### 🧠 Why Docker Networking Matters

In real-world apps, containers need to **talk to each other**:

- Your **web app** container might need to talk to a **database** container.
    
- Your **frontend** might talk to an **API backend**.
    

Also, your app might need to be **accessible from outside Docker**, like via `localhost:3000`.

Let’s break it down:

---

## 🌐 1. **Exposing Ports (Host ↔ Container)**

### 🔧 Example:

bash

CopyEdit

`docker run -d -p 8080:80 --name web nginx`

This maps:

- Port **80** inside the container → Port **8080** on your machine.
    
- So, visiting `http://localhost:8080` hits the container’s NGINX server.
    

---

### Syntax Recap:

ruby

CopyEdit

`-p <host_port>:<container_port>`

You can also bind to a specific IP:

css

CopyEdit

`-p 127.0.0.1:8080:80`

That makes it **only accessible locally** (good for dev).

---

## 🕸️ 2. **Docker Networks (Container ↔ Container)**

### 🧠 Key Concept:

By default, **containers are isolated**. But if you put them on the **same Docker network**, they can talk to each other **by name**.

---

### 🔧 Example: Flask App + Redis

We'll simulate a web app that uses Redis.

#### Step 1: Create a user-defined bridge network

bash

CopyEdit

`docker network create my-net`

#### Step 2: Start Redis container on that network

bash

CopyEdit

`docker run -d --name my-redis --network my-net redis`

#### Step 3: Create a simple Flask app (save as `app.py`):

python

CopyEdit

`from flask import Flask import redis  app = Flask(__name__) cache = redis.Redis(host='my-redis', port=6379)  @app.route('/') def hello():     cache.incr('hits')     return f"Hello! This page has been viewed {cache.get('hits').decode()} times."  if __name__ == '__main__':     app.run(host='0.0.0.0', port=5000)`

#### Step 4: Dockerfile for the app

Dockerfile

CopyEdit

`FROM python:3.10-slim WORKDIR /app COPY . . RUN pip install flask redis EXPOSE 5000 CMD ["python", "app.py"]`

#### Step 5: Build & Run on the Same Network

bash

CopyEdit

`docker build -t flask-redis-app .  docker run -d --name my-app \   --network my-net \   -p 5000:5000 \   flask-redis-app`

---

### ✅ Now:

- Visit `http://localhost:5000`
    
- Refresh the page a few times — it counts the views!
    
- Redis is **reachable by container name** `my-redis` thanks to Docker’s DNS.
    

---

## 🔌 3. Types of Docker Networks

|Type|Description|
|---|---|
|**bridge** (default)|Containers can communicate on the same host|
|**host**|Container shares the host's network stack (Linux only)|
|**none**|Fully isolated (no networking)|
|**custom bridge**|Lets containers resolve each other by name|
|**overlay**|Used with Docker Swarm (cross-host networking)|

Use this to list and inspect:

bash

CopyEdit

`docker network ls docker network inspect my-net`

---

### 🧪 Challenge (10–15 mins):

1. Create two containers on the same custom network.
    
2. One runs a small Python HTTP server.
    
3. The other curls it by name.
    

Let me know if you want code templates.

---

### ✅ Recap

- `-p` exposes ports to the host.
    
- Containers on the same **custom network** can talk to each other by name.
    
- Docker has built-in DNS to resolve container names.
    
- Best practice: **use custom networks** in multi-container setups.